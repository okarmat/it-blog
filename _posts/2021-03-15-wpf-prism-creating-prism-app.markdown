---
layout: post
title:  "WPF - Utworzenie aplikacji aplikacji Prism"
date:   2021-03-15 21:43:00 +0100
category: wpf prism
---
Aby, utworzyć aplikację WPF korzystającą z biblioteki Prism, należy utworzyć projekt będący aplikacją WPF. Następnie należy zainstalować paczkę Prism.Unity (ewentualnie Prism.Dryloc). Konieczne będzie dostosowanie plików `App.xaml` oraz `App.xaml.cs`.

{% highlight xml %}
<prism:PrismApplication x:Class="Ewid.InterMap.App"
                        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                        xmlns:prism="http://prismlibrary.com/">
</prism:PrismApplication>
{% endhighlight %}

{% highlight cs %}
    public partial class App : PrismApplication
    {
        protected override Window CreateShell()
        {
            throw new System.NotImplementedException();
        }

        protected override void RegisterTypes(IContainerRegistry containerRegistry)
        {
            throw new System.NotImplementedException();
        }
    }
{% endhighlight %}

Więcej informacji można znaleźć [tutaj]:[wpf-prism].

[wpf-prism]: https://app.pluralsight.com/library/courses/prism-wpf-introduction/table-of-contents