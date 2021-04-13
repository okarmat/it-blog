---
layout: post
title:  "WPF - Moduły"
date:   2021-03-17 22:30:00 +0100
category: wpf prism moduły region
---

Co to jest moduł?
Większa funkcjonalna odpowiedzialność aplikacji. Można je wydzielić określając funkcjonalności aplikacji które mogą istnieć samodzielnie. Oznacza to, że wszystkie widoki, serwisy czy logika odpowiedzialna za daną funkcjonalność może działać na własną rękę.
W Visual Studio modułem nazywamy bibliotekę klas która zawiera klasę implementującą interfejs `IModule`. 

{% highlight cs %}
    public class ModuleAModule : IModule
    {
        public void RegisterTypes(IContainerRegistry containerRegistry)
        {
        }

        public void OnInitialized(IContainerProvider containerProvider)
        {
        }
    }
{% endhighlight %}

Podłączenie modułu do aplikacji możemy zrealizować na kilka sposobów:
a) uwzględnienie go w metodzie `ConfigureModuleCatalog` głównej klasy `App` dziedziczącej po `PrismApplication`.

{% highlight cs %}
    public partial class App : PrismApplication
    {
        protected override Window CreateShell()
        {
            return Container.Resolve<ShellWindow>();
        }

        protected override void RegisterTypes(IContainerRegistry containerRegistry)
        {
        }

        protected override void ConfigureModuleCatalog(IModuleCatalog moduleCatalog)
        {
            moduleCatalog.AddModule<ModuleAModule>();
        }
    }
{% endhighlight %}

b) 

Ważne, aby zdecydować kiedy należy rejestrować moduł. Trzeba sobie odpowiedzieć na pytania:
Czy jest potrzebny do uruchomienia aplikacji?
Czy jest używany zawsze?
Czy jest używany rzadko?
Czy ma zależności?

Jeśli będziemy znać odpowiedzi na te pytania będziemy mogli zdecydować kiedy załadować moduł.
- Jeśli dostępny
- Na rządanie