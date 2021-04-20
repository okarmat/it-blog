---
layout: post
title:  "WPF - Region Navigation"
date:   2021-03-28 13:52:00 +0100
category: wpf prism region navigation
---
# Teoria
Podobnie jak w przypadku `View Composition` mechanizm `Region Navigation` dodaje widok do określonego regionu. Dokładnie mówiąc podczas nawigowania przechodzimy od jednego widoku do drugiego (i wyświetlamy je w regionie).
Chcąc przenieść się do określonego widoku przekazujemy odpowiedni string (URI) - klucz unikalny. To różni od `View Composition` który jest przydatny kiedy nie używamy modelu widoku, ale gdy jesteś w modelu widoku nie możesz użyć `View Discovering` lub `View Injection` bo to wymagałoby referencji do typu widoku lub instancji widoku. Dlatego przydatny jest w takiej sytuacji `Region Navigation` który używa stringa do nawigowania do widoku. To eliminuje powiązania od widoku do modelu widoku.
 
# Rejestrowanie widoku do nawigacji
Rejestrowania dokonujemy w klasie modułu lub w klasie dziedziczącej po `PrismApplication`.
Możemy rejestrować w następujący sposób:
 
*  Rejestrowanie za pomocą nazwy widoku jako unikalny klucz który będzie używany do nawigowania.
{% highlight cs %}
ContainerRegistry.RegisterForNavigation<ViewA>();
{% endhighlight %}

* Rejestrowanie za pomocą określonej nazwy.
{% highlight cs %}
ContainerRegistry.RegisterForNavigation<ViewA>("CustomName"); 
{% endhighlight %}

* Rejestrowanie za pomocą nazwy widoku jako unikalny klucz który będzie używany do nawigowania - jeśli mamy oryginalną konwencję nazw odbiegającą od domyślnej.
{% highlight cs %}
ContainerRegistry.RegisterForNavigation<ViewA, ViewAViewModel>();
{% endhighlight %}

* Rejestrowanie za pomocą określonej nazwy - jeśli mamy specjalną konwencję nazw odbiegającą od domyślnej.
{% highlight cs %}
ContainerRegistry.RegisterForNavigation<ViewA, ViewAViewModel>("CustomName"); 
{% endhighlight %}

# Wykonywanie nawigacji za pomocą Region Navigation

* Nawigowanie poprzez podanie nazwy widoku i regionu do `RequestNavigate`.
{% highlight cs %}
IRegionManager regionManager = ...;

regionManager.RequestNavigate("RegionName", "ViewA");
{% endhighlight %}

* Nawigowanie poprzez wyszukanie regionu po nazwie i podanie nazwy widoku do `RequestNavigate`.
{% highlight cs %}
IRegion region = regionManager.Regions["RegionName"];
region.RequestNavigate("ViewA");
{% endhighlight %}

# Przykłady zastosowania Region Navigation

* Rejestrowanie mapowania nawigacji w klasie implementującej interfejs `IModule`.
{% highlight cs %}
public class ModuleAModule : IModule
{
    public void OnInitialized(IContainerProvider containerProvider)
    {
    }

    public void RegisterTypes(IContainerRegistry containerRegistry)
    {
        containerRegistry.RegisterForNavigation<ViewA>("CustomName");
        containerRegistry.RegisterForNavigation<ViewB>();
    }
}
{% endhighlight %}

* Przygotowanie komendy która zostanie podłączona do przycisku nawigującego w widoku.
{% highlight cs %}
public class ShellWindowViewModel : BindableBase
{
    private readonly IRegionManager _regionManager;

    public DelegateCommand<string> NavigateCommand { get; private set; }

    public ShellWindowViewModel(IRegionManager regionManager)
    {
        NavigateCommand = new DelegateCommand<string>(Navigate);
        _regionManager = regionManager;
    }

    private void Navigate(string viewName)
    {
        _regionManager.RequestNavigate("ContentRegion", viewName, Callback);
    }

    private void Callback(NavigationResult result)
    {
        if (result.Error != null)
        {
            //handle error
        }
    }
}
{% endhighlight %}

* Podpięcie komendy do przycisku.
{% highlight xml %}
<Window x:Class="PrismDemo.Views.ShellWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="ShellWindow" Height="450" Width="800">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="5" >
            <Button Command="{Binding NavigateCommand}" CommandParameter="CustomName" Margin="5">Navigate to View A</Button>
            <Button Command="{Binding NavigateCommand}" CommandParameter="ViewB" Margin="5">Navigate to View B</Button>
        </StackPanel>
        <ContentControl Grid.Row="1" prism:RegionManager.RegionName="ContentRegion" />
    </Grid>
</Window>
{% endhighlight %}


Nikiedy zachodzi potrzeba, aby widok lub model widoku uczestniczył w procesie nawigacji. Aby móc tego dokonać implementujemy interfejs `INavigationAware` który wprowadza trzy metody:
* `OnNavigatedFrom`,
* `IsNavigationTarget`,
* `OnNavigatedTo`.

Cały proces INavigationAware składa się z następujących etapów:
* RequestNavigate,
* OnNavigatedFrom,
* IsNavigationTarget,
* ResolveView,
* OnNavigatedTo,
* Navigate Complete.

