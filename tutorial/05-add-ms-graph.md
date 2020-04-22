<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0e603-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0e603-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="0e603-102">Para esta aplicación, usará la [biblioteca de cliente de Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0e603-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="0e603-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="0e603-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="0e603-104">Agregue una nueva página para la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="0e603-104">Add a new page for the calendar view.</span></span> <span data-ttu-id="0e603-105">Haga clic con el botón secundario en el proyecto **GraphTutorial** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija **página en blanco**, `CalendarPage.xaml` escriba en el campo **nombre** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0e603-105">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="0e603-106">Abra `CalendarPage.xaml` y agregue la siguiente línea dentro del elemento `<Grid>` existente.</span><span class="sxs-lookup"><span data-stu-id="0e603-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. <span data-ttu-id="0e603-107">Abra `CalendarPage.xaml.cs` y agregue las siguientes `using` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="0e603-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="0e603-108">Agregue las siguientes funciones a la `CalendarPage` clase.</span><span class="sxs-lookup"><span data-stu-id="0e603-108">Add the following functions to the `CalendarPage` class.</span></span>

    ```csharp
    private void ShowNotification(string message)
    {
        // Get the main page that contains the InAppNotification
        var mainPage = (Window.Current.Content as Frame).Content as MainPage;

        // Get the notification control
        var notification = mainPage.FindName("Notification") as InAppNotification;

        notification.Show(message);
    }

    protected override async void OnNavigatedTo(NavigationEventArgs e)
    {
        // Get the Graph client from the provider
        var graphClient = ProviderManager.Instance.GlobalProvider.Graph;

        try
        {
            // Get the events
            var events = await graphClient.Me.Events.Request()
                .Select("subject,organizer,start,end")
                .OrderBy("createdDateTime DESC")
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch(Microsoft.Graph.ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }
    ```

    <span data-ttu-id="0e603-109">Tenga en cuenta lo que `OnNavigatedTo` hace el código.</span><span class="sxs-lookup"><span data-stu-id="0e603-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

    - <span data-ttu-id="0e603-110">La dirección URL a la que se `/v1.0/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="0e603-110">The URL that will be called is `/v1.0/me/events`.</span></span>
    - <span data-ttu-id="0e603-111">La `Select` función limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="0e603-111">The `Select` function limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="0e603-112">La `OrderBy` función ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="0e603-112">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="0e603-113">Modifique el `NavView_ItemInvoked` método en el `MainPage.xaml.cs` archivo para reemplazar la instrucción `switch` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e603-113">Modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the existing `switch` statement with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SwitchStatementSnippet" highlight="4":::

<span data-ttu-id="0e603-114">Ahora puede ejecutar la aplicación, iniciar sesión y hacer clic en el elemento de navegación **calendario** en el menú de la izquierda.</span><span class="sxs-lookup"><span data-stu-id="0e603-114">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="0e603-115">Debería ver un volcado JSON de los eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="0e603-115">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="0e603-116">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="0e603-116">Display the results</span></span>

1. <span data-ttu-id="0e603-117">Reemplace todo el contenido de `CalendarPage.xaml` con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e603-117">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

    ```xaml
    <Page
        x:Class="GraphTutorial.CalendarPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:GraphTutorial"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid>
            <controls:DataGrid x:Name="EventList" Grid.Row="1"
                    AutoGenerateColumns="False">
                <controls:DataGrid.Columns>
                    <controls:DataGridTextColumn
                            Header="Organizer"
                            Width="SizeToCells"
                            Binding="{Binding Organizer.EmailAddress.Name}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Subject"
                            Width="SizeToCells"
                            Binding="{Binding Subject}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Start"
                            Width="SizeToCells"
                            Binding="{Binding Start.DateTime}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="End"
                            Width="SizeToCells"
                            Binding="{Binding End.DateTime}"
                            FontSize="20" />
                </controls:DataGrid.Columns>
            </controls:DataGrid>
        </Grid>
    </Page>
    ```

1. <span data-ttu-id="0e603-118">Abra `CalendarPage.xaml.cs` y reemplace la `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` línea con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e603-118">Open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    <span data-ttu-id="0e603-119">Si ejecuta la aplicación ahora y selecciona el calendario, debe obtener una lista de eventos en una cuadrícula de datos.</span><span class="sxs-lookup"><span data-stu-id="0e603-119">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="0e603-120">Sin embargo, los valores de **Inicio** y **finalización** se muestran de forma no fácil del usuario.</span><span class="sxs-lookup"><span data-stu-id="0e603-120">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="0e603-121">Puede controlar cómo se muestran esos valores con un convertidor de [valores](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span><span class="sxs-lookup"><span data-stu-id="0e603-121">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

1. <span data-ttu-id="0e603-122">Haga clic con el botón derecho en el proyecto **GraphTutorial** en el explorador de soluciones y seleccione **Agregar > clase...**. Asigne un nombre `GraphDateTimeTimeZoneConverter.cs` a la clase y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="0e603-122">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="0e603-123">Reemplace todo el contenido del archivo por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e603-123">Replace the entire contents of the file with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    <span data-ttu-id="0e603-124">Este código toma la estructura [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) devuelta por Microsoft Graph y la analiza en un `DateTimeOffset` objeto.</span><span class="sxs-lookup"><span data-stu-id="0e603-124">This code takes the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="0e603-125">A continuación, convierte el valor en la zona horaria del usuario y devuelve el valor con formato.</span><span class="sxs-lookup"><span data-stu-id="0e603-125">It then converts the value into the user's time zone and returns the formatted value.</span></span>

1. <span data-ttu-id="0e603-126">Abra `CalendarPage.xaml` y agregue lo siguiente **antes** del `<Grid>` elemento.</span><span class="sxs-lookup"><span data-stu-id="0e603-126">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. <span data-ttu-id="0e603-127">Reemplace los dos `DataGridTextColumn` últimos elementos por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0e603-127">Replace the last two `DataGridTextColumn` elements with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. <span data-ttu-id="0e603-128">Ejecute la aplicación, inicie sesión y haga clic en el elemento de navegación **calendario** .</span><span class="sxs-lookup"><span data-stu-id="0e603-128">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="0e603-129">Debe ver la lista de eventos con los valores de **Inicio** y **finalización** con formato.</span><span class="sxs-lookup"><span data-stu-id="0e603-129">You should see the list of events with the **Start** and **End** values formatted.</span></span>

    ![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
