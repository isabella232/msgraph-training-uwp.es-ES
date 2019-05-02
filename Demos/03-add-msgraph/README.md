# <a name="how-to-run-the-completed-project"></a>Cómo ejecutar el proyecto completado

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:

- [Visual Studio](https://visualstudio.microsoft.com/vs/) instalado en el equipo de desarrollo. Si no tiene Visual Studio, visite el vínculo anterior de opciones de descarga. (**Nota:** este tutorial se ha escrito con Visual Studio 2017 versión 15,81. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.
- Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.

Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

## <a name="register-a-native-application-with-the-azure-active-directory-admin-center"></a>Registrar una aplicación nativa con el centro de administración de Azure Active Directory

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones** en **administrar**.

    ![Una captura de pantalla de los registros de la aplicación ](/tutorial/images/aad-portal-app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación**, establezca los valores siguientes.

    - Establezca **Nombre** como `UWP Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - Deje **URI de redireccionamiento** vacía.

    ![Captura de pantalla de la página registrar una aplicación](/tutorial/images/aad-register-an-app.png)

1. Elija **Registrar**. En la página **tutorial de gráficos de UWP** , copie el valor del identificador de la **aplicación (cliente)** y lo guarde, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](/tutorial/images/aad-application-id.png)

1. Seleccione el vínculo **Agregar un URI de** redireccionamiento. En la página **URI** de redireccionamiento, busque la sección **URI de redireccionamiento sugeridos para clientes públicos (móvil, escritorio)** . Seleccione el `urn:ietf:wg:oauth:2.0:oob` URI y, a continuación, elija **Guardar**.

    ![Captura de pantalla de la página URI de redireccionamiento](/tutorial/images/aad-redirect-uris.png)

## <a name="configure-the-sample"></a>Configuración del ejemplo

1. Cambie el nombre `OAuth.resw.example` del archivo `OAuth.resw`a.
1. Abrir `graph-tutorial.sln` en Visual Studio.
1. Edite `OAuth.resw` el archivo en Visual Studio. Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.
1. En el explorador de soluciones, haga clic con el botón secundario en la solución del **tutorial gráfico** y elija **restaurar paquetes NuGet**.

## <a name="run-the-sample"></a>Ejecutar el ejemplo

En Visual Studio, presione **F5** o elija **depurar _GT_ iniciar**depuración.