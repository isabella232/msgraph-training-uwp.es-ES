<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="90add-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="90add-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="90add-102">Para esta aplicación, usará la [biblioteca de cliente de Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="90add-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="90add-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="90add-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="90add-104">Comience agregando una nueva página para la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="90add-104">Start by adding a new page for the calendar view.</span></span> <span data-ttu-id="90add-105">Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija **página en blanco**, `CalendarPage.xaml` escriba en el campo **nombre** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="90add-105">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

<span data-ttu-id="90add-106">Abra `CalendarPage.xaml` y agregue la siguiente línea dentro del elemento `<Grid>` existente.</span><span class="sxs-lookup"><span data-stu-id="90add-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

<span data-ttu-id="90add-107">Abra `CalendarPage.xaml.cs` y agregue las siguientes `using` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="90add-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

<span data-ttu-id="90add-108">A continuación, agregue las siguientes funciones `CalendarPage` a la clase.</span><span class="sxs-lookup"><span data-stu-id="90add-108">Then add the following functions to the `CalendarPage` class.</span></span>

```cs
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
    // Get the Graph client from the service
    var graphClient = MicrosoftGraphService.Instance.GraphProvider;

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

<span data-ttu-id="90add-109">Tenga en cuenta lo que `OnNavigatedTo` hace el código.</span><span class="sxs-lookup"><span data-stu-id="90add-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

- <span data-ttu-id="90add-110">La dirección URL a la que se `/v1.0/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="90add-110">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="90add-111">La `Select` función limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="90add-111">The `Select` function limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="90add-112">La `OrderBy` función ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="90add-112">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="90add-113">Justo antes de ejecutar la aplicación, para poder navegar a esta página del calendario, modifique el `NavView_ItemInvoked` método en el `MainPage.xaml.cs` archivo para reemplazar la línea por `throw new NotImplementedException();` el siguiente.</span><span class="sxs-lookup"><span data-stu-id="90add-113">Just before running the app, in order to be able to navigate to this calendar page, modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the `throw new NotImplementedException();` line with as follows.</span></span>

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

<span data-ttu-id="90add-114">Ahora puede ejecutar la aplicación, iniciar sesión y hacer clic en el elemento de navegación **calendario** en el menú de la izquierda.</span><span class="sxs-lookup"><span data-stu-id="90add-114">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="90add-115">Debería ver un volcado JSON de los eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="90add-115">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="90add-116">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="90add-116">Display the results</span></span>

<span data-ttu-id="90add-117">Ahora puede reemplazar el volcado de JSON con algo para mostrar los resultados de forma fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="90add-117">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="90add-118">Reemplace todo el contenido de `CalendarPage.xaml` con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="90add-118">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

```xml
<Page
    x:Class="graph_tutorial.CalendarPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:graph_tutorial"
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

<span data-ttu-id="90add-119">Esto reemplaza `TextBlock` por un `DataGrid`.</span><span class="sxs-lookup"><span data-stu-id="90add-119">This replaces the `TextBlock` with a `DataGrid`.</span></span> <span data-ttu-id="90add-120">Ahora, `CalendarPage.xaml.cs` abra y reemplace `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` la línea por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="90add-120">Now open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

<span data-ttu-id="90add-121">Si ejecuta la aplicación ahora y selecciona el calendario, debe obtener una lista de eventos en una cuadrícula de datos.</span><span class="sxs-lookup"><span data-stu-id="90add-121">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="90add-122">Sin embargo, los valores de **Inicio** y **finalización** se muestran de forma no fácil del usuario.</span><span class="sxs-lookup"><span data-stu-id="90add-122">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="90add-123">Puede controlar cómo se muestran esos valores con un convertidor de [valores](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span><span class="sxs-lookup"><span data-stu-id="90add-123">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

<span data-ttu-id="90add-124">Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y seleccione **Agregar > clase...**. Asigne un nombre `GraphDateTimeTimeZoneConverter.cs` a la clase y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="90add-124">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="90add-125">Reemplace todo el contenido del archivo por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="90add-125">Replace the entire contents of the file with the following.</span></span>

```cs
using Microsoft.Graph;
using System;

namespace graph_tutorial
{
    class GraphDateTimeTimeZoneConverter : Windows.UI.Xaml.Data.IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            DateTimeTimeZone date = value as DateTimeTimeZone;

            if (date != null)
            {
                // Resolve the time zone
                var timezone = TimeZoneInfo.FindSystemTimeZoneById(date.TimeZone);
                // Parse method assumes local time, which may not be the case
                var parsedDateAsLocal = DateTimeOffset.Parse(date.DateTime);
                // Determine the offset from UTC time for the specific date
                // Making this call adjusts for DST as appropriate
                var tzOffset = timezone.GetUtcOffset(parsedDateAsLocal.DateTime);
                // Create a new DateTimeOffset with the specific offset from UTC
                var correctedDate = new DateTimeOffset(parsedDateAsLocal.DateTime, tzOffset);
                // Return the local date time string
                return correctedDate.LocalDateTime.ToString();
            }

            return string.Empty;
        }

        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

<span data-ttu-id="90add-126">Este código toma la estructura [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) devuelta por Microsoft Graph y la analiza en un `DateTimeOffset` objeto.</span><span class="sxs-lookup"><span data-stu-id="90add-126">This code takes the [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="90add-127">A continuación, convierte el valor en la zona horaria del usuario y devuelve el valor con formato.</span><span class="sxs-lookup"><span data-stu-id="90add-127">It then converts the value into the user's time zone and returns the formatted value.</span></span>

<span data-ttu-id="90add-128">Abra `CalendarPage.xaml` y agregue lo siguiente **antes** del `<Grid>` elemento.</span><span class="sxs-lookup"><span data-stu-id="90add-128">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

<span data-ttu-id="90add-129">A continuación, reemplace `Binding="{Binding Start.DateTime}"` la línea por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="90add-129">Then, replace the `Binding="{Binding Start.DateTime}"` line with the following.</span></span>

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

<span data-ttu-id="90add-130">Reemplace la `Binding="{Binding End.DateTime}"` línea por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="90add-130">Replace the `Binding="{Binding End.DateTime}"` line with the following.</span></span>

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

<span data-ttu-id="90add-131">Ejecute la aplicación, inicie sesión y haga clic en el elemento de navegación **calendario** .</span><span class="sxs-lookup"><span data-stu-id="90add-131">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="90add-132">Debe ver la lista de eventos con los valores de **Inicio** y **finalización** con formato.</span><span class="sxs-lookup"><span data-stu-id="90add-132">You should see the list of events with the **Start** and **End** values formatted.</span></span>

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
