---
layout: post
title:  "WPF - Commands"
date:   2021-03-19 22:30:00 +0100
category: wpf prism command commands
---

Komendy nie są czymś wyjątkowym i unikalnym w Prism. Ten sam koncept jest dostępny w standardzie technologii WPF. W skrócie łączą gest wykonany z poziomu UI do konkretnej akcji. Aby zaimplementować komendę musimy zastosować interfejs `ICommand`. Ten interfejs definiuje dwie metody:
* Execute(object parameter) to zadanie jakie ma się wykonać po wykonaniu danego gestu z poziomu UI,
* CanExecute(object parameter) - jest wywoływana przed `Execute`. Określa czy można wykonać metodę `Execute`.
Jeśli `CanExecute` zwróci false to dany przycisk powiązany z daną komendą będzie nieaktywny.

# DelegateCommand

W Prism stosujemy `DelegateCommand` jako odpowiednik `ICommand`.
* nie wymaga event handlera,
* stosuje delegate methods,
* definiujemy je wewnątrz widoku modelu.

Prism posiada dwa typy komend. Ma `DelegateCommand` i `DelegateCommand<T>`. Druga wersja z nich umożliwia przekazywanie w metodach `Execute` i `CanExecute` parametr typu T.

{% highlight xml %}
<UserControl x:Class="ModuleA.Views.ViewA"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:prism="http://prismlibrary.com/"
             prism:ViewModelLocator.AutoWireViewModel="True">
    <StackPanel>
        <Button Content="Click Me" Command="{Binding ClickCommand}"/>
        <TextBlock Text="{Binding Text}" />
    </StackPanel>
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

    public DelegateCommand ClickCommand { get; set; }

    public ViewAViewModel()
    {
        ClickCommand = new DelegateCommand(Click, CanClick);
    }

    private bool CanClick()
    {
        return true;
    }

    private void Click()
    {
        Text = "You Clicked me!";
    }
}
{% endhighlight %}

{% highlight xml %}
<UserControl x:Class="ModuleA.Views.ViewA"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:prism="http://prismlibrary.com/"
             prism:ViewModelLocator.AutoWireViewModel="True">
    <StackPanel>
        <Button Content="Click Me" Command="{Binding ClickCommand}" CommandParameter="Hello with parameter"/>
        <TextBlock Text="{Binding Text}" />
    </StackPanel>
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

    public DelegateCommand<string> ClickCommand { get; set; }

    public ViewAViewModel()
    {
        ClickCommand = new DelegateCommand<string>(Click, CanClick);
    }

    private bool CanClick(string parameter)
    {
        return true;
    }

    private void Click(string parameter)
    {
        Text = parameter;
    }
}
{% endhighlight %}

# Raising Change Notifications
Aby powiadomić komendę o zmianie która zaszła w widoku modelu musimy zastosować jeden z trzech sposobów:
* RaiseCanExecuteChanged - to po prostu wywołanie metody `DelegateCommand.RaiseCanExecuteChanged` co prowidzi do ponownego wywołania metody `CanExecute` podłączonej do komendy,

{% highlight cs %}
public class ViewAViewModel : BindableBase
{
    private string _text = "Hello from ViewAViewModel";
    public string Text
    {
        get { return _text; }
        set { SetProperty(ref _text, value); }
    }

    private bool _canExecute;
    public bool CanExecute
    {
        get { return _canExecute; }
        set 
        { 
            SetProperty(ref _canExecute, value);

            ClickCommand.RaiseCanExecuteChanged();
        }
    }

    public DelegateCommand ClickCommand { get; set; }

    public ViewAViewModel()
    {
        ClickCommand = new DelegateCommand(Click, CanClick);
    }

    private bool CanClick()
    {
        return CanExecute;
    }

    private void Click()
    {
        Text = "You Clicked me!";
    }
}
{% endhighlight %}
	
* ObservesProperty - umożliwia określenie propercji którą komenda obserwuje i która określa czy zaszła zmiana czy nie, i jeśli tak to wykona ponownie metodę `CanExecute`. Jednocześnie wymusi na wiązaniach UI, aby odzwierciedlił zmiany w wartości `CanExecute`. Możemy w ten sposób podłączyć kilka monitorowanych propercji. Jeśli którakolwiek z nich zostanie zmieniona to wymuszone zostanie ponowne wykonanie metody `CanExecute`.

{% highlight cs %}
public class ViewAViewModel : BindableBase
{
    private string _text = "Hello from ViewAViewModel";
    public string Text
    {
        get { return _text; }
        set { SetProperty(ref _text, value); }
    }

    private bool _canExecute;
    public bool CanExecute
    {
        get { return _canExecute; }
        set { SetProperty(ref _canExecute, value); }
    }

    public DelegateCommand ClickCommand { get; set; }

    public ViewAViewModel()
    {
        ClickCommand = new DelegateCommand(Click, CanClick).ObservesProperty(() => CanExecute);
    }

    private bool CanClick()
    {
        return CanExecute;
    }

    private void Click()
    {
        Text = "You Clicked me!";
    }
}
{% endhighlight %}
	
* ObservesCanExecute - podobnie jak `ObservesProperty` monitorujemy wartość wybranej propercji. Nie dostarczamy jednak implmentacji metody `CanExecute`. Jeśli propercja ulegnie zmianie to jej wartość będzie wartością która zwróciłaby metoda `CanExecute`.

{% highlight cs %}
public class ViewAViewModel : BindableBase
{
    private string _text = "Hello from ViewAViewModel";
    public string Text
    {
        get { return _text; }
        set { SetProperty(ref _text, value); }
    }

    private bool _canExecute;
    public bool CanExecute
    {
        get { return _canExecute; }
        set { SetProperty(ref _canExecute, value); }
    }

    public DelegateCommand ClickCommand { get; set; }

    public ViewAViewModel()
    {
        ClickCommand = new DelegateCommand(Click).ObservesCanExecute(() => CanExecute);
    }

    private void Click()
    {
        Text = "You Clicked me!";
    }
}
{% endhighlight %}

# Composite Command
Composite Command to komenda z którą powiązanych jest kilka `DelegateCommand`. Dzięki temu możemy np. przy tworzeniu funkcji zbiorczej np. `SaveAll` stworzyć komendę typu composite w której zarejestrujemy kilka różnych komend `Save` z różnych widoków modelów. W momencie, gdy wywołamy metodę `CompositeCommand.Execute()` wywołane zostaną wszystkie metody `Execute` wszystkich powiązanych `DelegateCommand`.
Jednocześnie jeśli którakolwiek z instancji `DelegateCommand` będzie zwracała wynik metody `CanExecute` jako false to `CompositeCommand` będzie zwracał z metody `CanExecute` wartość false (niezależnie od tego czy którakolwiek podłączona instancja `DelegateCommand` będzie posiadał wartość wynikową funkcji `CanExecute` równą `true`).

Aby zarejestrować w `CompositeCommand` instancję `DelegateCommand` należy użyć metody `SaveCommand.RegisterCommand(delegateCommand)`. WAŻNE! Należy pamiętać, aby odrejestrować `CompositeCommand` w momencie, gdy widok i widok modelu przestanie być potrzebny, aby nie dochodziło do `wycieku pamięci`.

Tworzymy nową klasę `ApplicationCommands` i interfejs `IApplicationCommands` w wydzielonym projekcie np. PrismDemo.Core.
{% highlight cs %}
public class ApplicationCommands : IApplicationCommands
{
    public CompositeCommand SaveAllCommand { get; } = new CompositeCommand();
}
{% endhighlight %}

{% highlight cs %}
public interface IApplicationCommands
{
    CompositeCommand SaveAllCommand { get; }
}
{% endhighlight %}

Rejestrujemy w `App.cs` w kontenereze IOC klasę `ApplicationCommands` jako Singleton.
{% highlight cs %}
public partial class App : PrismApplication
{
    protected override Window CreateShell()
    {
        return Container.Resolve<MainWindow>();
    }

    protected override void ConfigureModuleCatalog(IModuleCatalog moduleCatalog)
    {
        moduleCatalog.AddModule<ModuleA.ModuleAModule>();
    }

    protected override void RegisterTypes(IContainerRegistry containerRegistry)
    {
        containerRegistry.RegisterSingleton<IApplicationCommands, ApplicationCommands>();
    }
}
{% endhighlight %}

Wstrzykujemy `ApplicationCommands` przez konstruktur do `MainWindowViewModel` i bindujemy button z właściwą metodą.
{% highlight cs %}
public class MainWindowViewModel : BindableBase
{
    private IApplicationCommands _applicationCommands;
    public IApplicationCommands ApplicationCommands
    {
        get { return _applicationCommands; }
        set { SetProperty(ref _applicationCommands, value); }
    }

    public MainWindowViewModel(IApplicationCommands applicationCommands)
    {
        ApplicationCommands = applicationCommands;
    }
}
{% endhighlight %}

{% highlight xml %}
<Window x:Class="PrismDemo.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True" 
        Height="350" Width="525">

    <Window.Resources>
        <Style TargetType="TabItem">
            <Setter Property="Header" Value="{Binding DataContext.Title}" />
        </Style>
    </Window.Resources>
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Button Content="Save All" Margin="10" Command="{Binding ApplicationCommands.SaveAllCommand}"/>

        <TabControl Grid.Row="1" Margin="10" prism:RegionManager.RegionName="ContentRegion" />
    </Grid>
</Window>
{% endhighlight %}

W widoku modelu którego `DelegateCommand` chcemy podpiąć do komendy kompozytowej wstrzykujemy przez konstruktur `IApplicationCommands` a nastepnie rejestrujemy w `CompositeCommand` właściwą instancję `DelegateCommand`.
{% highlight cs %}
public class TabViewModel : BindableBase
{
    private string _title;
    public string Title
    {
        get { return _title; }
        set { SetProperty(ref _title, value); }
    }

    private bool _canUpdate = true;
    public bool CanUpdate
    {
        get { return _canUpdate; }
        set { SetProperty(ref _canUpdate, value); }
    }

    private string _updatedText;
    public string UpdateText
    {
        get { return _updatedText; }
        set { SetProperty(ref _updatedText, value); }
    }

    public DelegateCommand UpdateCommand { get; private set; }

    public TabViewModel(IApplicationCommands applicationCommands)
    {
        UpdateCommand = new DelegateCommand(Update).ObservesCanExecute(() => CanUpdate);

        applicationCommands.SaveAllCommand.RegisterCommand(UpdateCommand);
    }

    private void Update()
    {
        UpdateText = $"Updated: {DateTime.Now}";
    }       
}
{% endhighlight %}
