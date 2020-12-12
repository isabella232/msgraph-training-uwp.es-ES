<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="144f1-101">En esta sección, crearemos una nueva aplicación para UWP.</span><span class="sxs-lookup"><span data-stu-id="144f1-101">In this section you'll create a new UWP app.</span></span>

1. <span data-ttu-id="144f1-102">Abra Visual Studio y seleccione **crear un nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="144f1-102">Open Visual Studio, and select **Create a new project**.</span></span> <span data-ttu-id="144f1-103">Elija la opción **aplicación en blanco (Windows universal)** que usa C# y luego seleccione **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="144f1-103">Choose the **Blank App (Universal Windows)** option that uses C#, then select **Next**.</span></span>

    ![Cuadro de diálogo Crear nuevo proyecto de Visual Studio 2019](./images/vs-create-new-project.png)

1. <span data-ttu-id="144f1-105">En el cuadro de diálogo **configurar el nuevo proyecto** , escriba `GraphTutorial` en el campo **nombre del proyecto** y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="144f1-105">In the **Configure your new project** dialog, enter `GraphTutorial` in the **Project name** field and select **Create**.</span></span>

    ![Cuadro de diálogo Configurar nuevo proyecto de Visual Studio 2019](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > <span data-ttu-id="144f1-107">Asegúrese de que escribe exactamente el mismo nombre para el proyecto de Visual Studio que se especifica en estas instrucciones de la práctica.</span><span class="sxs-lookup"><span data-stu-id="144f1-107">Ensure that you enter the exact same name for the Visual Studio Project that is specified in these lab instructions.</span></span> <span data-ttu-id="144f1-108">El nombre del proyecto de Visual Studio se convierte en parte del espacio de nombres en el código.</span><span class="sxs-lookup"><span data-stu-id="144f1-108">The Visual Studio Project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="144f1-109">El código incluido en estas instrucciones depende del espacio de nombres que coincida con el nombre de proyecto de Visual Studio especificado en estas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="144f1-109">The code inside these instructions depends on the namespace matching the Visual Studio Project name specified in these instructions.</span></span> <span data-ttu-id="144f1-110">Si usa un nombre de proyecto diferente, el código no se compilará a menos que ajuste todos los espacios de nombres para que se correspondan con el nombre del proyecto de Visual Studio que ha especificado al crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="144f1-110">If you use a different project name the code will not compile unless you adjust all the namespaces to match the Visual Studio Project name you enter when you create the project.</span></span>

1. <span data-ttu-id="144f1-111">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="144f1-111">Select **OK**.</span></span> <span data-ttu-id="144f1-112">En el cuadro de diálogo **nuevo proyecto de la plataforma universal de Windows** , asegúrese de que la **versión mínima** esté establecida en `Windows 10, Version 1809 (10.0; Build 17763)` o posterior y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="144f1-112">In the **New Universal Windows Platform Project** dialog, ensure that the **Minimum version** is set to `Windows 10, Version 1809 (10.0; Build 17763)` or later and select **OK**.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="144f1-113">Instalar paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="144f1-113">Install NuGet packages</span></span>

<span data-ttu-id="144f1-114">Antes de continuar, instale algunos paquetes NuGet adicionales que usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="144f1-114">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="144f1-115">[Microsoft. Toolkit. UWP. UI. Controls. DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) para mostrar la información devuelta por Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="144f1-115">[Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) to display the information returned by Microsoft Graph.</span></span>
- <span data-ttu-id="144f1-116">[Microsoft. Toolkit. Graph. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) para controlar el inicio de sesión y la recuperación de tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="144f1-116">[Microsoft.Toolkit.Graph.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) to handle login and access token retrieval.</span></span>

1. <span data-ttu-id="144f1-117">Seleccione **herramientas > el administrador de paquetes de NuGet > consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="144f1-117">Select **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="144f1-118">En la consola del administrador de paquetes, escriba los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="144f1-118">In the Package Manager Console, enter the following commands.</span></span>

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -IncludePrerelease
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a><span data-ttu-id="144f1-119">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="144f1-119">Design the app</span></span>

<span data-ttu-id="144f1-120">En esta sección, creará la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="144f1-120">In this section you'll create the UI for the app.</span></span>

1. <span data-ttu-id="144f1-121">Empiece agregando una variable de nivel de aplicación para realizar un seguimiento del estado de autenticación.</span><span class="sxs-lookup"><span data-stu-id="144f1-121">Start by adding an application-level variable to track authentication state.</span></span> <span data-ttu-id="144f1-122">En el explorador de soluciones, expanda **app. Xaml** y Abra **app.Xaml.CS**.</span><span class="sxs-lookup"><span data-stu-id="144f1-122">In Solution Explorer, expand **App.xaml** and open **App.xaml.cs**.</span></span> <span data-ttu-id="144f1-123">Agregue la siguiente propiedad a la `App` clase.</span><span class="sxs-lookup"><span data-stu-id="144f1-123">Add the following property to the `App` class.</span></span>

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. <span data-ttu-id="144f1-124">Definir el diseño de la Página principal.</span><span class="sxs-lookup"><span data-stu-id="144f1-124">Define the layout for the main page.</span></span> <span data-ttu-id="144f1-125">Abra `MainPage.xaml` y reemplace todo el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="144f1-125">Open `MainPage.xaml` and replace its entire contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    <span data-ttu-id="144f1-126">Define un [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) básico con los vínculos de navegación de eventos de **Inicio**, **calendario** y **nuevo** para actuar como la vista principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="144f1-126">This defines a basic [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) with **Home**, **Calendar**, and **New event** navigation links to act as the main view of the app.</span></span> <span data-ttu-id="144f1-127">También agrega un control [LoginButton](https://github.com/windows-toolkit/Graph-Controls) en el encabezado de la vista.</span><span class="sxs-lookup"><span data-stu-id="144f1-127">It also adds a [LoginButton](https://github.com/windows-toolkit/Graph-Controls) control in the header of the view.</span></span> <span data-ttu-id="144f1-128">Ese control permitirá al usuario iniciar y cerrar sesión. El control no está totalmente habilitado todavía, se configurará en un ejercicio posterior.</span><span class="sxs-lookup"><span data-stu-id="144f1-128">That control will allow the user to sign in and out. The control isn't fully enabled yet, you will configure it in a later exercise.</span></span>

1. <span data-ttu-id="144f1-129">Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija **página en blanco**, escriba `HomePage.xaml` en el campo **nombre** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="144f1-129">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `HomePage.xaml` in the **Name** field, and select **Add**.</span></span> <span data-ttu-id="144f1-130">Reemplace el elemento existente del `<Grid>` archivo por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="144f1-130">Replace the existing `<Grid>` element in the file with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. <span data-ttu-id="144f1-131">Expanda **mainpage. Xaml** en el explorador de soluciones y Abra `MainPage.xaml.cs` .</span><span class="sxs-lookup"><span data-stu-id="144f1-131">Expand **MainPage.xaml** in Solution Explorer and open `MainPage.xaml.cs`.</span></span> <span data-ttu-id="144f1-132">Agregue la siguiente función a la `MainPage` clase para administrar el estado de autenticación.</span><span class="sxs-lookup"><span data-stu-id="144f1-132">Add the following function to the `MainPage` class to manage authentication state.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. <span data-ttu-id="144f1-133">Agregue el siguiente código al `MainPage()` constructor **después** de la `this.InitializeComponent();` línea.</span><span class="sxs-lookup"><span data-stu-id="144f1-133">Add the following code to the `MainPage()` constructor **after** the `this.InitializeComponent();` line.</span></span>

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    <span data-ttu-id="144f1-134">Cuando la aplicación se inicie por primera vez, inicializará el estado de autenticación en `false` y navegará a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="144f1-134">When the app first starts, it will initialize the authentication state to `false` and navigate to the home page.</span></span>

1. <span data-ttu-id="144f1-135">Agregue el siguiente controlador de eventos para cargar la página solicitada cuando el usuario selecciona un elemento de la vista de navegación.</span><span class="sxs-lookup"><span data-stu-id="144f1-135">Add the following event handler to load the requested page when the user selects an item from the navigation view.</span></span>

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
            case "new event":
                throw new NotImplementedException();
                break;
            case "calendar":
                throw new NotImplementedException();
                break;
            case "home":
            default:
                RootFrame.Navigate(typeof(HomePage));
                break;
        }
    }
    ```

1. <span data-ttu-id="144f1-136">Guarde todos los cambios y, a continuación, presione **F5** o seleccione **depurar > iniciar depuración** en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="144f1-136">Save all of your changes, then press **F5** or select **Debug > Start Debugging** in Visual Studio.</span></span>

    > [!NOTE]
    > <span data-ttu-id="144f1-137">Asegúrese de seleccionar la configuración adecuada para su equipo (ARM, x64, x86).</span><span class="sxs-lookup"><span data-stu-id="144f1-137">Make sure you select the appropriate configuration for your machine (ARM, x64, x86).</span></span>

    ![Captura de la página principal](./images/create-app-01.png)
