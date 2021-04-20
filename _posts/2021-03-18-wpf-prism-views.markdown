---
layout: post
title:  "WPF - Widoki"
date:   2021-03-18 21:15:00 +0100
category: wpf prism widoki view views
---

# Co to jest widok?
Widok to część, porcja interfejsu użytkownika.
Okno może się składać z jednego lub wielu widoków które są wstrzykiwane w regiony. Np. MenuView, NavigationView, ContentView.
W VisualStudio widok to `UserControl`. 90% wstrzykiwanych widoków do regionów to UserControl.
Pojedynczy widok może posiadać inne widoki zdefiniowane wewnątrz niego. Może też posiadać regiony które będą mieć zagnieżdżone inne widoki.
Można mieć wiele instancji tego samego widoku.

{% highlight xml %}
<UserControl x:Class="ModuleA.Views.ViewA"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:ModuleA.Views"
             mc:Ignorable="d" 
             d:DesignHeight="450" d:DesignWidth="800">
    <Grid>
        <TextBlock Text="Hello from ViewA" HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="48"/>
    </Grid>
</UserControl>
{% endhighlight %}

# View Composition
View Composition to proces tworzenia widoku a dokładnie proces tworzenia instancji widoku i wyświetlenie go / wstrzyknięcie go do regionów.
Mamy do dyspozycji dwa podejścia, aby uzyskać View Composition.
* View Discovery - przeniesienie odpowiedzialności utworzenia / wyświetlenie widoku na region.
* View Injection - za utworzenie, wyświetlenie i wstrzyknięcie widoku odpowiedzialny jest developer.

# View Discovery
W tym podejściu regiony same szukają widoków. Nie ma się wyraźnej kontroli, ale dzięki temu cały proces dzieje się automatycznie.

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
        _regionManager.RegisterViewWithRegion("ContentRegion", typeof(ViewA));
    }

    public void RegisterTypes(IContainerRegistry containerRegistry)
    {
            
    }
}
{% endhighlight %}

# View Injection
W tym podejściu to developer musi dodawać i usuwać widoki. Ma wyraźną kontrolę nad procesem. Sam aktywuje i dezaktywuje widoki.
Ważne, aby region istniał przed wstrzyknięciem widoku.

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
        IRegion region = _regionManager.Regions["ContentRegion"];

        var view1 = containerProvider.Resolve<ViewA>();
        region.Add(view1);

        var view2 = containerProvider.Resolve<ViewA>();
        view2.Content = new TextBlock()
        {
            Text = "Hello from View 2",
            HorizontalAlignment = System.Windows.HorizontalAlignment.Center,
            VerticalAlignment = System.Windows.VerticalAlignment.Center
        };

        region.Add(view2);

        region.Activate(view2);

        region.Activate(view1);

        region.Deactivate(view1);

        region.Activate(view2);

        region.Remove(view2);

        region.Activate(view1);
    }

    public void RegisterTypes(IContainerRegistry containerRegistry)
    {

    }
}
{% endhighlight %}