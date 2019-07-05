<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la [biblioteca de cliente de Microsoft Graph para .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

Comience agregando una nueva página para la vista de calendario. Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y seleccione **Agregar > nuevo elemento..**.. Elija **página en blanco**, `CalendarPage.xaml` escriba en el campo **nombre** y seleccione **Agregar**.

Abra `CalendarPage.xaml` y agregue la siguiente línea dentro del elemento `<Grid>` existente.

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

Abra `CalendarPage.xaml.cs` y agregue las siguientes `using` instrucciones en la parte superior del archivo.

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

A continuación, agregue las siguientes funciones `CalendarPage` a la clase.

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

Tenga en cuenta lo que `OnNavigatedTo` hace el código.

- La dirección URL a la que se `/v1.0/me/events`llamará es.
- La `Select` función limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
- La `OrderBy` función ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.

Justo antes de ejecutar la aplicación, para poder navegar a esta página del calendario, modifique el `NavView_ItemInvoked` método en el `MainPage.xaml.cs` archivo para reemplazar la línea por `throw new NotImplementedException();` el siguiente.

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

Ahora puede ejecutar la aplicación, iniciar sesión y hacer clic en el elemento de navegación **calendario** en el menú de la izquierda. Debería ver un volcado JSON de los eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede reemplazar el volcado de JSON con algo para mostrar los resultados de forma fácil de uso. Reemplace todo el contenido de `CalendarPage.xaml` con lo siguiente.

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

Esto reemplaza `TextBlock` por un `DataGrid`. Ahora, `CalendarPage.xaml.cs` abra y reemplace `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` la línea por lo siguiente.

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

Si ejecuta la aplicación ahora y selecciona el calendario, debe obtener una lista de eventos en una cuadrícula de datos. Sin embargo, los valores de **Inicio** y **finalización** se muestran de forma no fácil del usuario. Puede controlar cómo se muestran esos valores con un convertidor de [valores](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

Haga clic con el botón secundario en el proyecto de **tutorial gráfico** en el explorador de soluciones y seleccione **Agregar > clase...**. Asigne un nombre `GraphDateTimeTimeZoneConverter.cs` a la clase y seleccione **Agregar**. Reemplace todo el contenido del archivo por lo siguiente.

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

Este código toma la estructura [dateTimeTimeZone](https://docs.microsoft.com/graph/api/resources/datetimetimezone?view=graph-rest-1.0) devuelta por Microsoft Graph y la analiza en un `DateTimeOffset` objeto. A continuación, convierte el valor en la zona horaria del usuario y devuelve el valor con formato.

Abra `CalendarPage.xaml` y agregue lo siguiente **antes** del `<Grid>` elemento.

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

A continuación, reemplace `Binding="{Binding Start.DateTime}"` la línea por lo siguiente.

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

Reemplace la `Binding="{Binding End.DateTime}"` línea por lo siguiente.

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

Ejecute la aplicación, inicie sesión y haga clic en el elemento de navegación **calendario** . Debe ver la lista de eventos con los valores de **Inicio** y **finalización** con formato.

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
