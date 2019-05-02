<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="571ee-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="571ee-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="571ee-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="571ee-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="571ee-103">En este paso, integrará el control [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) desde el [Kit de herramientas](https://github.com/Microsoft/WindowsCommunityToolkit) de la comunidad de Windows en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="571ee-103">In this step you will integrate the [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) control from the [Windows Community Toolkit](https://github.com/Microsoft/WindowsCommunityToolkit) into the application.</span></span>

<span data-ttu-id="571ee-104">Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y elija **Agregar > nuevo elemento..**.. Elija el **archivo de recursos (. resw)**, asigne `OAuth.resw` un nombre al archivo y elija **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="571ee-104">Right-click the **graph-tutorial** project in Solution Explorer and choose **Add > New Item...**. Choose **Resources File (.resw)**, name the file `OAuth.resw` and choose **Add**.</span></span> <span data-ttu-id="571ee-105">Cuando el nuevo archivo se abra en Visual Studio, cree dos recursos de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="571ee-105">When the new file opens in Visual Studio, create two resources as follows.</span></span>

- <span data-ttu-id="571ee-106">**Name:** `AppId`, **Valor:** el identificador de la aplicación que ha generado en el portal de registro de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="571ee-106">**Name:** `AppId`, **Value:** the app ID you generated in Application Registration Portal</span></span>
- <span data-ttu-id="571ee-107">**Name:** `Scopes`, **Valor:**`User.Read Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="571ee-107">**Name:** `Scopes`, **Value:** `User.Read Calendars.Read`</span></span>

![Una captura de pantalla del archivo OAuth. resw en el editor de Visual Studio](./images/edit-resources-01.png)

> [!IMPORTANT]
> <span data-ttu-id="571ee-109">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `OAuth.resw` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="571ee-109">If you're using source control such as git, now would be a good time to exclude the `OAuth.resw` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="configure-the-aadlogin-control"></a><span data-ttu-id="571ee-110">Configurar el control AadLogin</span><span class="sxs-lookup"><span data-stu-id="571ee-110">Configure the AadLogin control</span></span>

<span data-ttu-id="571ee-111">Empiece agregando código para leer los valores del archivo de recursos.</span><span class="sxs-lookup"><span data-stu-id="571ee-111">Start by adding code to read the values out of the resources file.</span></span> <span data-ttu-id="571ee-112">Abra `MainPage.xaml.cs` y agregue la siguiente `using` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="571ee-112">Open `MainPage.xaml.cs` and add the following `using` statement to the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

<span data-ttu-id="571ee-113">Reemplace la línea `RootFrame.Navigate(typeof(HomePage));` por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="571ee-113">Replace the `RootFrame.Navigate(typeof(HomePage));` line with the following code.</span></span>

```cs
// Load OAuth settings
var oauthSettings = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("OAuth");
var appId = oauthSettings.GetString("AppId");
var scopes = oauthSettings.GetString("Scopes");

if (string.IsNullOrEmpty(appId) || string.IsNullOrEmpty(scopes))
{
    Notification.Show("Could not load OAuth Settings from resource file.");
}
else
{
    // Initialize Graph
    MicrosoftGraphService.Instance.AuthenticationModel = MicrosoftGraphEnums.AuthenticationModel.V2;
    MicrosoftGraphService.Instance.Initialize(appId,
        MicrosoftGraphEnums.ServicesToInitialize.UserProfile,
        scopes.Split(' '));

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="571ee-114">Este código carga la configuración desde `OAuth.resw` e inicializa la instancia global de `MicrosoftGraphService` con esos valores.</span><span class="sxs-lookup"><span data-stu-id="571ee-114">This code loads the settings from `OAuth.resw` and initializes the global instance of the `MicrosoftGraphService` with those values.</span></span>

<span data-ttu-id="571ee-115">Ahora, agregue un controlador de eventos `SignInCompleted` para el evento `AadLogin` en el control.</span><span class="sxs-lookup"><span data-stu-id="571ee-115">Now add an event handler for the `SignInCompleted` event on the `AadLogin` control.</span></span> <span data-ttu-id="571ee-116">Abra el `MainPage.xaml` archivo y reemplace el elemento `<graphControls:AadLogin>` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="571ee-116">Open the `MainPage.xaml` file and replace the existing `<graphControls:AadLogin>` element with the following.</span></span>

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

<span data-ttu-id="571ee-117">A continuación, agregue las siguientes funciones `MainPage` a la `MainPage.xaml.cs`clase en.</span><span class="sxs-lookup"><span data-stu-id="571ee-117">Then add the following functions to the `MainPage` class in `MainPage.xaml.cs`.</span></span>

```cs
private void Login_SignInCompleted(object sender, Microsoft.Toolkit.Uwp.UI.Controls.Graph.SignInEventArgs e)
{
    // Set the auth state
    SetAuthState(true);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}

private void Login_SignOutCompleted(object sender, EventArgs e)
{
    // Set the auth state
    SetAuthState(false);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="571ee-118">Por último, en el explorador de soluciones, expanda **homepage. Xaml** y Abra `HomePage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="571ee-118">Finally, in Solution Explorer, expand **HomePage.xaml** and open `HomePage.xaml.cs`.</span></span> <span data-ttu-id="571ee-119">Agregue el siguiente código después de `this.InitializeComponent();` la línea.</span><span class="sxs-lookup"><span data-stu-id="571ee-119">Add the following code after the `this.InitializeComponent();` line.</span></span>

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

<span data-ttu-id="571ee-120">Reinicie la aplicación y haga clic en el control de **Inicio de sesión** en la parte superior de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="571ee-120">Restart the app and click the **Sign In** control at the top of the app.</span></span> <span data-ttu-id="571ee-121">Una vez que haya iniciado sesión, la interfaz de usuario debe cambiar para indicar que ha iniciado sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="571ee-121">Once you've signed in, the UI should change to indicate that you've successfully signed-in.</span></span>

![Una captura de pantalla de la aplicación después de iniciar sesión](./images/add-aad-auth-01.png)

> [!NOTE]
> <span data-ttu-id="571ee-123">El `AadLogin` control implementa la lógica del almacenamiento y la actualización del token de acceso por usted.</span><span class="sxs-lookup"><span data-stu-id="571ee-123">The `AadLogin` control implements the logic of storing and refreshing the access token for you.</span></span> <span data-ttu-id="571ee-124">Los tokens se almacenan en el almacenamiento seguro y se actualizan según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="571ee-124">The tokens are stored in secure storage and refreshed as needed.</span></span>