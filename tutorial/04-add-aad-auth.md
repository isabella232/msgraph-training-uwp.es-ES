<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará el control **LoginButton** de los [controles de Windows Graph](https://github.com/windows-toolkit/Graph-Controls) en la aplicación.

1. Haga clic con el botón secundario en el proyecto **GraphTutorial** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija el **archivo de recursos (. resw)**, asigne un nombre al archivo `OAuth.resw` y seleccione **Agregar**. Cuando el nuevo archivo se abra en Visual Studio, cree dos recursos de la siguiente manera.

    - **Name:** `AppId` , **Value:** el identificador de la aplicación que ha generado en el portal de registro de aplicaciones
    - **Nombre:** `Scopes` , **valor:**`User.Read User.ReadBasic.All People.Read MailboxSettings.Read Calendars.ReadWrite`

    ![Una captura de pantalla del archivo OAuth. resw en el editor de Visual Studio](./images/edit-resources-01.png)

    > [!IMPORTANT]
    > Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `OAuth.resw` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.

## <a name="configure-the-loginbutton-control"></a>Configurar el control LoginButton

1. Abra `MainPage.xaml.cs` y agregue la siguiente `using` instrucción a la parte superior del archivo.

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    ```

1. Reemplace el constructor existente por lo siguiente.

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ConstructorSnippet":::

    Este código carga la configuración desde `OAuth.resw` e inicializa el proveedor de MSAL con esos valores.

1. Ahora, agregue un controlador de eventos para el `ProviderUpdated` evento en el `ProviderManager` . Agregue la siguiente función a la clase `MainPage`.

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ProviderUpdatedSnippet":::

    Este evento se desencadena cuando cambia el proveedor o cuando cambia el estado del proveedor.

1. En el explorador de soluciones, expanda **homepage. Xaml** y Abra `HomePage.xaml.cs` . Reemplace el constructor existente por lo siguiente.

    :::code language="csharp" source="../demo/GraphTutorial/HomePage.xaml.cs" id="ConstructorSnippet":::

1. Reinicie la aplicación y haga clic en el control de **Inicio de sesión** en la parte superior de la aplicación. Una vez que haya iniciado sesión, la interfaz de usuario debe cambiar para indicar que ha iniciado sesión correctamente.

    ![Una captura de pantalla de la aplicación después de iniciar sesión](./images/add-aad-auth-01.png)

    > [!NOTE]
    > El `ButtonLogin` control implementa la lógica del almacenamiento y la actualización del token de acceso por usted. Los tokens se almacenan en el almacenamiento seguro y se actualizan según sea necesario.
