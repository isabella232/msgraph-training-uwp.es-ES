<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

1. Agregue una nueva página para la nueva vista de eventos. Haga clic con el botón secundario en el proyecto **GraphTutorial** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija **página en blanco**, escriba `NewEventPage.xaml` en el campo **nombre** y seleccione **Agregar**.

1. Abra **NewEventPage. Xaml** y reemplace su contenido por lo siguiente.

    :::code language="xaml" source="../demo/GraphTutorial/NewEventPage.xaml" id="NewEventPageXamlSnippet":::

1. Abra **NewEventPage.Xaml.CS** y agregue las siguientes `using` instrucciones en la parte superior del archivo.

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="UsingStatementsSnippet":::

1. Agregue la interfaz **INotifyPropertyChange** a la clase **NewEventPage** . Reemplace la declaración de clase existente por lo siguiente.

    ```csharp
    public sealed partial class NewEventPage : Page, INotifyPropertyChanged
    {
        public NewEventPage()
        {
            this.InitializeComponent();
            DataContext = this;
        }
    }
    ```

1. Agregue las siguientes propiedades a la clase **NewEventPage** .

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="PropertiesSnippet":::

1. Agregue el siguiente código para obtener la zona horaria del usuario de Microsoft Graph cuando se cargue la página.

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="LoadTimeZoneSnippet":::

1. Agregue el código siguiente para crear el evento.

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="CreateEventSnippet":::

1. Modifique el `NavView_ItemInvoked` método en el archivo **mainpage.Xaml.CS** para reemplazar la `switch` instrucción existente por lo siguiente.

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            RootFrame.Navigate(typeof(NewEventPage));
            break;
        case "calendar":
            RootFrame.Navigate(typeof(CalendarPage));
            break;
        case "home":
        default:
            RootFrame.Navigate(typeof(HomePage));
            break;
    }
    ```

1. Guarde los cambios y ejecute la aplicación. Inicie sesión, seleccione el elemento de menú **nuevo evento** , rellene el formulario y seleccione **crear** para agregar un evento al calendario del usuario.

    ![Captura de pantalla de la página de evento nueva](images/create-event-01.png)
