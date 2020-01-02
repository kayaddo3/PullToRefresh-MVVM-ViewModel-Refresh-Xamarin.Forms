# PullToRefresh-MVVM-ViewModel-Refresh-Xamarin.Forms
This sample demonstrates how to use the Syncfusion's PullToRefresh control in MVVM and refresh the data from view model.

# How to refresh the Xamarin.Forms PullToRefresh from ViewModel?
`SfPullToRefresh` is fully MVVM compatible and can be refreshed by binding a property in the view model to the [SfPullToRefresh.IsRefreshing](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfPullToRefresh.XForms~Syncfusion.SfPullToRefresh.XForms.SfPullToRefresh~IsRefreshing.html) property.
`SfPullToRefresh` also provides support for `SfPullToRefresh.RefreshCommand` that will be executed when the pulling is completed and the pointer is released.  You can also pass a desired object as parameter to the `SfPullToRefresh.RefreshCommand` using the `SfPullToRefresh.RefreshCommandParameter`.
The `SfPullToRefresh.RefreshCommand's` `CanExecute()` will be fired when the pulling action is performed. Returning `false` in the command's `CanExecute()` will cancel the pulling and the `SfPullToRefresh.RefreshCommand` will not be executed.

```xml

<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:syncfusion="clr-namespace:Syncfusion.SfPullToRefresh.XForms;assembly=Syncfusion.SfPullToRefresh.XForms"
             xmlns:local="clr-namespace:PullToRefresh_MVVM"
             mc:Ignorable="d"
             x:Class="PullToRefresh_MVVM.MainPage">
    <ContentPage.BindingContext>
        <local:ViewModel/>
    </ContentPage.BindingContext>
    <ContentPage.Content>
        <syncfusion:SfPullToRefresh x:Name="pullToRefresh"
                                    IsRefreshing="{Binding IsRefreshing}" 
                                    PullingThreshold="100"
                                    RefreshContentHeight="30"
                                    RefreshContentThreshold="30"
                                    RefreshContentWidth="30"
                                    RefreshCommand="{Binding RefreshCommand}">
            <syncfusion:SfPullToRefresh.PullableContent>
                <StackLayout BackgroundColor="#00AFF9" Orientation="Vertical">
                    <Label Text="New York Temperature" FontSize="Large" TextColor="White" HorizontalTextAlignment="Center" Margin="20"/>
                    <Image WidthRequest="100" HorizontalOptions="Center" HeightRequest="100"  Margin="20" Source="warmselected.png"/>
                    <Label Text="{Binding Temperature}" FontSize="Large" TextColor="White" HorizontalTextAlignment="Center" Margin="20" HeightRequest="100"/>
                </StackLayout>
            </syncfusion:SfPullToRefresh.PullableContent>
        </syncfusion:SfPullToRefresh>
    </ContentPage.Content>
</ContentPage>
```

```c#

using System;
using System.ComponentModel;
using System.Threading.Tasks;
using System.Windows.Input;
using Xamarin.Forms;

namespace PullToRefresh_MVVM
{
    public class ViewModel : INotifyPropertyChanged
    {
        Random random = new Random();
        private bool isRefreshing;
        string[] temperatureArray = new string[] { "22°", "18°", "12°", "25°", "23°", "20°", "25°" };
        private string temperature = "25°";
        public ICommand RefreshCommand { get; set; }
		
        public bool IsRefreshing
        {
            get
            {
                return isRefreshing;
            }
            set
            {
                isRefreshing = value;
                RaisePropertyChanged(nameof(IsRefreshing));
            }
        }
		
        public string Temperature
        {
            get
            {
                return temperature;
            }
            set
            {
                temperature = value;
                RaisePropertyChanged(nameof(Temperature));
            }
        }
		
        public ViewModel()
        {
            RefreshCommand = new Command(Refresh);
        }
		
        async private void Refresh(object obj)
        {
            IsRefreshing = true;
            await Task.Delay(1000);
            IsRefreshing = false;
            int index = random.Next(0, 6);
            Temperature = temperatureArray[index];
        }

        public event PropertyChangedEventHandler PropertyChanged;
        void RaisePropertyChanged(string PropertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(PropertyName));
        }
    }
}
```

## <a name="requirements-to-run-the-demo"></a>Requirements to run the demo ##

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## <a name="troubleshooting"></a>Troubleshooting ##
### Path too long exception
If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.
