<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="63642-101">En este ejercicio, creará una nueva aplicación nativa de Azure AD con el centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="63642-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="63642-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com) e inicie sesión con una **cuenta personal** (aka: cuenta de Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="63642-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="63642-103">Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones (vista previa)** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="63642-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="63642-104">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="63642-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="63642-105">Seleccione **registro nuevo**.</span><span class="sxs-lookup"><span data-stu-id="63642-105">Select **New registration**.</span></span> <span data-ttu-id="63642-106">En la página **registrar una aplicación** , establezca los valores de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="63642-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="63642-107">Establezca **el nombre** en `UWP Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="63642-107">Set **Name** to `UWP Graph Tutorial`.</span></span>
    - <span data-ttu-id="63642-108">Establezca **tipos de cuenta compatibles** en **cuentas de cualquier directorio de la organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="63642-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="63642-109">Deje el **URI** de redireccionamiento vacío.</span><span class="sxs-lookup"><span data-stu-id="63642-109">Leave **Redirect URI** empty.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="63642-111">Elija **registrar**.</span><span class="sxs-lookup"><span data-stu-id="63642-111">Choose **Register**.</span></span> <span data-ttu-id="63642-112">En la página **tutorial de gráficos de UWP** , copie el valor del identificador de la **aplicación (cliente)** y lo guarde, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="63642-112">On the **UWP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. <span data-ttu-id="63642-114">Seleccione el vínculo **Agregar un URI de** redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="63642-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="63642-115">En la página **URI** de redireccionamiento, busque la sección **URI de redireccionamiento sugeridos para clientes públicos (móvil, escritorio)** .</span><span class="sxs-lookup"><span data-stu-id="63642-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="63642-116">Seleccione el `urn:ietf:wg:oauth:2.0:oob` URI y, a continuación, elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="63642-116">Select the `urn:ietf:wg:oauth:2.0:oob` URI, then choose **Save**.</span></span>

    ![Captura de pantalla de la página URI de redireccionamiento](./images/aad-redirect-uris.png)