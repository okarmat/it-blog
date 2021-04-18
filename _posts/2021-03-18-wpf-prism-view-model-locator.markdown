---
layout: post
title:  "WPF - View Model Locator"
date:   2021-03-18 22:10:00 +0100
category: wpf prism view model locator
---

Jeśli używamy podczas tworzenia plikacji wzorca MVVM musimy zdecydować w jaki sposób połączymy ze sobą instancję view model i data context widoku.
Mamy kilka sposobów które pomogą nam ten cel zrealizować.
a) XAML - definiujemy instancję view modelu w pliku XAML i przypisujemy ją do propercji data context wewnątrz XAMLa,
b) Code Behinde - ręcznie przypisujemy view model do data context widoku w konstruktorze widoku,
c) Constructor Injection - wstrzykujemy view model do konstruktura widoku i później ręcznie przypisujemy go do data contextu,
d) View Model Locator - rekomendowany przy korzystaniu z frameworka Prism.

# View Model Locator

Konwencja nazw
1. Wszystkie widoki są umieszczone w przestrzeni nazw `Views`.
2. Wszystkie widoki modeli są umieszczone w przestrzeni nazw `ViewModels`.
3. Nazwa widoku + "ViewModel" = Nazwa widoku modelu
4. Jeśli nazwa widoku kończy się na "View" to nie dublujemy "View" w nazwie widoku modelu. (MainView + ViewModel = MainViewModel)

Aby aktywować automatyczne połączenie widoku z widokiem modelu ustawiamy w widoku `AutoWireViewModel` na true.
Oczywiście pamiętamy o konwencji nazw widoków i widoków modeli.

{% highlight xml %}
<UserControl x:Class="ModuleA.Views.ViewA"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:prism="http://prismlibrary.com/"
             prism:ViewModelLocator.AutoWireViewModel="True">
    <Grid>
        <TextBlock Text="{Binding Text}" HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="48"/>
    </Grid>
</UserControl>
{% endhighlight %}

{% highlight cs %}
public class ViewAViewModel : BindableBase
{
    private string _text = "Hello from ViewAViewModel";
    public string Text
    {
        get { return _text; }
        set { SetProperty(ref _text, value); }
    }
}
{% endhighlight %}

Jeśli chcemy zmienić konwencje nazw - bo np. nasz projekt odbiega od powyższego schematu nazewnictwa - to musimy nadpisać w App.xaml.cs metodę ConfigureViewModelLocator logikę odnajdowania nazwy widoku w SetDefaultTypeToViewModelTypeResolver.

Niekiedy nie jesteśmy w stanie skorzystać z domyślnej konwencji przyjętej przez Prism. Wówczas dobrze jest mieć narzędzie do zarejestrowanie np. konkretnego typu widoku z konkretnym typem widoku modelu. Do tego celu używamy `ViewModelLocationProvider.Register`. Możemy nawet za pośrednictwem tej metody przygotować fabrykę która będzie dostarczała instancję widoku modelu dla powiązanego widoku.

{% highlight cs %}
public class ModuleAModule : IModule
{
    private readonly IRegionManager _regionManager;

    public ModuleAModule(IRegionManager regionManager)
    {
        _regionManager = regionManager;
    }

    public void OnInitialized(IContainerProvider containerProvider)
    {
        _regionManager.RegisterViewWithRegion("ContentRegion", typeof(ControlA));
    }

    public void RegisterTypes(IContainerRegistry containerRegistry)
    {
	    //Using Types
        ViewModelLocationProvider.Register<ControlA, ControlAViewModel>();

		// Using Factory
        //ViewModelLocationProvider.Register<ControlA>(() => new ControlAViewModel() { Text = "Hello from Factory" });
    }
}
{% endhighlight %}

Zaletą tego sposobu poza rozwiązaniem problemu z konwencją nazw jest fakt, że jest szybsza aniżeli automatyczne łączenie widoków i widoków modeli które do tego celu wykorzystują refleksję.