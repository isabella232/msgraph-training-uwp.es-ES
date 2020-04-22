<!-- markdownlint-disable MD002 MD041 -->

En esta sección, crearemos una nueva aplicación para UWP.

1. Abra Visual Studio y seleccione **crear un nuevo proyecto**. Elija la opción **aplicación en blanco (Windows universal)** que usa C# y luego seleccione **siguiente**.

    ![Cuadro de diálogo Crear nuevo proyecto de Visual Studio 2019](./images/vs-create-new-project.png)

1. En el cuadro de diálogo **configurar el nuevo proyecto** , escriba `GraphTutorial` en el campo **nombre del proyecto** y seleccione **crear**.

    ![Cuadro de diálogo Configurar nuevo proyecto de Visual Studio 2019](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > Asegúrese de que escribe exactamente el mismo nombre para el proyecto de Visual Studio que se especifica en estas instrucciones de la práctica. El nombre del proyecto de Visual Studio se convierte en parte del espacio de nombres en el código. El código incluido en estas instrucciones depende del espacio de nombres que coincida con el nombre de proyecto de Visual Studio especificado en estas instrucciones. Si usa un nombre de proyecto diferente, el código no se compilará a menos que ajuste todos los espacios de nombres para que se correspondan con el nombre del proyecto de Visual Studio que ha especificado al crear el proyecto.

1. Elija **Aceptar**. En el cuadro de diálogo **nuevo proyecto de la plataforma universal de Windows** , asegúrese de que la **versión mínima** esté establecida en `Windows 10, Version 1809 (10.0; Build 17763)` o posterior y seleccione **Aceptar**.

## <a name="install-nuget-packages"></a>Instalar paquetes NuGet

Antes de continuar, instale algunos paquetes NuGet adicionales que usará más adelante.

- [Microsoft. Toolkit. UWP. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/) para agregar controles de interfaz de usuario para notificaciones en la aplicación y indicadores de carga.
- [Microsoft. Toolkit. UWP. UI. Controls. DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) para mostrar la información devuelta por Microsoft Graph.
- [Microsoft. Toolkit. Graph. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) para controlar el inicio de sesión y la recuperación de tokens de acceso.

1. Seleccione **herramientas > el administrador de paquetes de NuGet > consola del administrador de paquetes**. En la consola del administrador de paquetes, escriba los siguientes comandos.

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls -Version 6.0.0
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -Version 6.0.0
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a>Diseñar la aplicación

En esta sección, creará la interfaz de usuario de la aplicación.

1. Empiece agregando una variable de nivel de aplicación para realizar un seguimiento del estado de autenticación. En el explorador de soluciones, expanda **app. Xaml** y Abra **app.Xaml.CS**. Agregue la siguiente propiedad a la `App` clase.

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. Definir el diseño de la Página principal. Abra `MainPage.xaml` y reemplace todo el contenido por lo siguiente.

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    Define un [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) básico con vínculos de navegación de **calendario** y **hogares** para actuar como la vista principal de la aplicación. También agrega un control [LoginButton](https://github.com/windows-toolkit/Graph-Controls) en el encabezado de la vista. Ese control permitirá al usuario iniciar y cerrar sesión. El control no está totalmente habilitado todavía, se configurará en un ejercicio posterior.

1. Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija **página en blanco**, `HomePage.xaml` escriba en el campo **nombre** y seleccione **Agregar**. Reemplace el elemento `<Grid>` existente del archivo por lo siguiente.

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. Expanda **mainpage. Xaml** en el explorador de `MainPage.xaml.cs`soluciones y abra. Agregue la siguiente función a la `MainPage` clase para administrar el estado de autenticación.

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. Agregue el siguiente código al `MainPage()` constructor **después** de la `this.InitializeComponent();` línea.

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    Cuando la aplicación se inicie por primera vez, inicializará el estado `false` de autenticación en y navegará a la Página principal.

1. Agregue el siguiente controlador de eventos para cargar la página solicitada cuando el usuario selecciona un elemento de la vista de navegación.

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
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

1. Guarde todos los cambios y, a continuación, presione **F5** o seleccione **depurar > iniciar depuración** en Visual Studio.

    > [!NOTE]
    > Asegúrese de seleccionar la configuración adecuada para su equipo (ARM, x64, x86).

    ![Captura de la página principal](./images/create-app-01.png)
