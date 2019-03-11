# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="1dbcd-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="1dbcd-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dbcd-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1dbcd-102">Prerequisites</span></span>

<span data-ttu-id="1dbcd-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1dbcd-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="1dbcd-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="1dbcd-105">Si no tiene Visual Studio, visite el vínculo anterior de opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="1dbcd-106">(**Nota:** este tutorial se ha escrito con Visual Studio 2017 versión 15,81.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="1dbcd-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="1dbcd-108">Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="1dbcd-109">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="1dbcd-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="1dbcd-110">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="1dbcd-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="1dbcd-111">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-native-application-with-the-application-registration-portal"></a><span data-ttu-id="1dbcd-112">Registro de una aplicación nativa con el portal de registro de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="1dbcd-112">Register a native application with the Application Registration Portal</span></span>

1. <span data-ttu-id="1dbcd-113">Abra un explorador y vaya al [portal de registro de aplicaciones](https://apps.dev.microsoft.com) e inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-113">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="1dbcd-114">Seleccione **Agregar una aplicación** en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-114">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="1dbcd-115">**Nota:** Si ve más de un botón **Agregar una aplicación** en la página, seleccione el que corresponda a la lista de **aplicaciones convergentes** .</span><span class="sxs-lookup"><span data-stu-id="1dbcd-115">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="1dbcd-116">En la página **registrar la aplicación** , establezca el tutorial **nombre** de la aplicación para **UWP gráfico** y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-116">On the **Register your application** page, set the **Application Name** to **UWP Graph Tutorial** and select **Create**.</span></span>

    ![Captura de pantalla de la creación de una nueva aplicación en el sitio web del portal de registro de aplicaciones](../../../Images/arp-create-app-01.png)

1. <span data-ttu-id="1dbcd-118">En la página de **registro del tutorial de gráficos de UWP** , en la sección **propiedades** , copie el identificador de la **aplicación** ya que lo necesitará más adelante.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-118">On the **UWP Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de pantalla del identificador de la aplicación recién creada](../../../Images/arp-create-app-02.png)

1. <span data-ttu-id="1dbcd-120">Desplácese hacia abajo hasta la sección **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="1dbcd-120">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="1dbcd-121">Seleccione **Agregar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-121">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="1dbcd-122">En el cuadro de diálogo **Agregar plataforma** , seleccione **aplicación nativa**.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-122">In the **Add Platform** dialog, select **Native Application**.</span></span>

        ![Captura de pantalla que crea una plataforma para la aplicación](../../../Images/arp-create-app-03.png)

1. <span data-ttu-id="1dbcd-124">Desplácese hasta la parte inferior de la página y seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-124">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="1dbcd-125">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="1dbcd-125">Configure the sample</span></span>

1. <span data-ttu-id="1dbcd-126">Cambie el nombre `OAuth.resw.example` del archivo `OAuth.resw`a.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-126">Rename the `OAuth.resw.example` file to `OAuth.resw`.</span></span>
1. <span data-ttu-id="1dbcd-127">Abrir `graph-tutorial.sln` en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-127">Open `graph-tutorial.sln` in Visual Studio.</span></span>
1. <span data-ttu-id="1dbcd-128">Edite `OAuth.resw` el archivo en Visual Studio. Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-128">Edit the `OAuth.resw` file in visual studio.Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="1dbcd-129">En el explorador de soluciones, haga clic con el botón secundario en la solución del **tutorial gráfico** y elija **restaurar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-129">In Solution Explorer, right-click the **graph-tutorial** solution and choose **Restore NuGet Packages**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="1dbcd-130">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="1dbcd-130">Run the sample</span></span>

<span data-ttu-id="1dbcd-131">En Visual Studio, presione **F5** o elija **depurar _GT_ iniciar**depuración.</span><span class="sxs-lookup"><span data-stu-id="1dbcd-131">In Visual Studio, press **F5** or choose **Debug > Start Debugging**.</span></span>