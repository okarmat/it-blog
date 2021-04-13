---
layout: post
title:  "WPF - Dialogs"
date:   2021-04-07 22:06:00 +0100
category: wpf prism dialogs dialog
---
# Definicja
Czym są dialogi?
Dialogi to formatka lub okno pojawiające się ponad aplikacją. Komunikuje się z użytkownikiem w celu:
* informowania użytkownika o czymś,
* zadania pytania,
* zdobycia informacji od użytkownika.

Dialogi moga być:
* modalne, 
* niemodalne.

Dialog to `UserControl`. Zalecane jest przygotowanie modelu widoku dla okna dialogowego. Okno dialogowe musi implementować `IDialogAware`.
Zapewnia on metody i propercje:
* `Title` - umożliwia kontrolę tytułu okna dialogowego,
* `RequestClose` - akcja umożliwiająca zamknięcie dialogu z poziomu kodu,
* `CanCloseDialog` - umożliwia zdeterminowanie czy użytkownik ma mieć możliwość zamknięcia okna czy nie,
* `OnDialogClosed` - wywoływana po zamknięciu okna dialogowego przez wywołanie `RequestClose` lub przez zamknięcie okna przez użytkownika,
* `OnDialogOpened` - wywoływana w trakcie otwierania okna dialogowego. Daje możliwość przekazania parametrów dialogu.

# Widok
{% highlight xml %}
<UserControl x:Class="PrismDemo.Dialogs.MessageDialog"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:prism="http://prismlibrary.com/"             
             prism:ViewModelLocator.AutoWireViewModel="True">
    <Grid x:Name="LayoutRoot" Margin="25">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <TextBlock Text="{Binding Message}" HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Grid.Row="0" TextWrapping="Wrap" />
        <Button Command="{Binding CloseDialogCommand}" Content="OK" Width="75" Height="25" HorizontalAlignment="Right" Margin="0,10,0,0" Grid.Row="1" IsDefault="True" />
    </Grid>
</UserControl>
{% endhighlight %}

# Model widoku
{% highlight cs %}
using Prism.Commands;
using Prism.Mvvm;
using Prism.Services.Dialogs;
using System;

namespace PrismDemo.Dialogs
{
    public class MessageDialogViewModel : BindableBase, IDialogAware
    {
        public string Title => "My Message Dialog";

        private string _message;
        public string Message
        {
            get { return _message; }
            set { SetProperty(ref _message, value); }
        }

        public DelegateCommand CloseDialogCommand { get; }

        public event Action<IDialogResult> RequestClose;

        public MessageDialogViewModel()
        {
            CloseDialogCommand = new DelegateCommand(CloseDialog);
        }

        private void CloseDialog()
        {
            var buttonResult = ButtonResult.OK;

            var parameters = new DialogParameters();
            parameters.Add("myParam", "The Dialog was closed by the user.");

            RequestClose?.Invoke(new DialogResult(buttonResult, parameters));
        }

        public bool CanCloseDialog()
        {
            return true;
        }

        public void OnDialogClosed()
        {            
        }

        public void OnDialogOpened(IDialogParameters parameters)
        {
            Message = parameters.GetValue<string>("message");
        }
    }
}
{% endhighlight %}

# Rejestrowanie dialogu

W `App.xaml.cs` rejestrujemy dialog w metodzie `RegisterTypes`.
{% highlight cs %}
using ModuleA;
using Prism.DryIoc;
using Prism.Ioc;
using Prism.Modularity;
using PrismDemo.Dialogs;
using PrismDemo.Views;
using System.Windows;

namespace PrismDemo
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : PrismApplication
    {
        protected override Window CreateShell()
        {
            return Container.Resolve<ShellWindow>();
        }

        protected override void RegisterTypes(IContainerRegistry containerRegistry)
        {
            containerRegistry.RegisterDialog<MessageDialog, MessageDialogViewModel>();
        }

        protected override void ConfigureModuleCatalog(IModuleCatalog moduleCatalog)
        {
            moduleCatalog.AddModule<ModuleAModule>();
        }
    }
}
{% endhighlight %}

Aby wyświetlić dialog z modelu widoku musimy użyć `IDialogService`. Serwis udostępnia dwie metody:
* Show (niemodalny dialog),
* ShowDialog (modalny dialog).

Ponizej przykład wywołania okna dialogowego oraz odczytanie wyniku okna dialogowego (`Callback`) a także wyłuskanie parametrów zwrotnych z `Parameters`.
{% highlight cs %}
using Prism.Commands;
using Prism.Mvvm;
using Prism.Services.Dialogs;

namespace ModuleA.ViewModels
{
    public class ViewAViewModel : BindableBase
    {
        private readonly IDialogService _dialogService;

        private string _messageReceived;
        public string MessageReceived
        {
            get { return _messageReceived; }
            set { SetProperty(ref _messageReceived, value); }
        }

        public DelegateCommand ShowDialogCommand { get; }

        public ViewAViewModel(IDialogService dialogService)
        {
            _dialogService = dialogService;

            ShowDialogCommand = new DelegateCommand(ShowDialog);
        }

        private void ShowDialog()
        {
            var p = new DialogParameters();
            p.Add("message", "Hello from ViewAViewModel");

            _dialogService.ShowDialog("MessageDialog", p, r =>
            {
                if (r.Result == ButtonResult.OK)
                {
                    MessageReceived = r.Parameters.GetValue<string>("myParam");
                }
                else
                {
                    MessageReceived = "Not closed by user";
                }
            });
        }
    }
}
{% endhighlight %}

W celu uproszczenia wywołania okna dialogowego możemy przygotować ExtensionMethod dla IDialogService.
using System;

{% highlight cs %}
namespace Prism.Services.Dialogs
{
    public static class IDialogServiceExtensions
    {
        public static void ShowMessageDialog(this IDialogService dialogService, string message, Action<IDialogResult> callback)
        {
            var p = new DialogParameters();
            p.Add("message", message);

            dialogService.ShowDialog("MessageDialog", p, callback);
        }
    }
}
{% endhighlight %}

Wówczas wywołanie okna dialogowego ma postać:
{% highlight cs %}
using Prism.Commands;
using Prism.Mvvm;
using Prism.Services.Dialogs;

namespace ModuleA.ViewModels
{
    public class ViewAViewModel : BindableBase
    {
        private readonly IDialogService _dialogService;

        private string _messageReceived;
        public string MessageReceived
        {
            get { return _messageReceived; }
            set { SetProperty(ref _messageReceived, value); }
        }

        public DelegateCommand ShowDialogCommand { get; }

        public ViewAViewModel(IDialogService dialogService)
        {
            _dialogService = dialogService;

            ShowDialogCommand = new DelegateCommand(ShowDialog);
        }

        private void ShowDialog()
        {
            _dialogService.ShowMessageDialog("Hello from ViewAViewModel", r =>
            {
                if (r.Result == ButtonResult.OK)
                {
                    MessageReceived = r.Parameters.GetValue<string>("myParam");
                }
                else
                {
                    MessageReceived = "Not closed by user";
                }
            });
        }
    }
}
{% endhighlight %}

Aby zdefiniować styl okna dialogowego konieczne jest dodanie elementu `prism:Dialog.WindowStyle` do widoku.
{% highlight xml %}
<UserControl x:Class="PrismDemo.Dialogs.MessageDialog"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:prism="http://prismlibrary.com/"             
             prism:ViewModelLocator.AutoWireViewModel="True">

    <prism:Dialog.WindowStyle>
        <Style TargetType="Window">
            <Setter Property="Height" Value="200" />
            <Setter Property="Width" Value="400"/>
            <Setter Property="ResizeMode" Value="NoResize" />
            <Setter Property="prism:Dialog.WindowStartupLocation" Value="CenterScreen" />
        </Style>
    </prism:Dialog.WindowStyle>
    
    <Grid x:Name="LayoutRoot" Margin="25">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <TextBlock Text="{Binding Message}" HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Grid.Row="0" TextWrapping="Wrap" />
        <Button Command="{Binding CloseDialogCommand}" Content="OK" Width="75" Height="25" HorizontalAlignment="Right" Margin="0,10,0,0" Grid.Row="1" IsDefault="True" />
    </Grid>
</UserControl>
{% endhighlight %}
