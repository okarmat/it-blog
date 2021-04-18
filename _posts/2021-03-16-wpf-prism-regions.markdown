---
layout: post
title:  "WPF - Regiony"
date:   2021-03-16 21:57:00 +0100
category: wpf prism regiony region
---
Czym są regiony? To miejsca w którym możemy umieszczać dynamiczną zawartość.
Możemy w jednym oknu umieścić kilka regionów. Np. "MenuRegion", "HeaderRegion", "ContentRegion".
Region nie wie co jest do niego "wstrzykiwane" - jest elastyczny, dzięki temu ma luźne powiązania z widokami.
Regiony mogą być tworzone w XAML lub w kodzie. W XAML używamy do tego `RegionManager.RegionName` a w kodzie: `RegionManager.SetRegionName`.


Czym jest RegionManager?
To klasa która zarządza kolekcją regionów. Zawsze gdy tworzymy nowy region zostaje on umieszczony w RegionManager. Dzięki temu mamy do niego dostęp z różnych miejsc. 
Dodatkowo regiony są używane w podejściu `View Composition` i `Region Naviagtion`
Oczywiście ważną funkcją RegionManagera jest definiowanie i tworzenie regionów.
Ważne, aby nazwy regionów były unikalne. Nie mogą się powtarzać.

{% highlight xml %}
<Window x:Class="Ewid.InterMap.Views.ShellWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:prism="http://prismlibrary.com/"
        mc:Ignorable="d"
        Title="ShellWindow" Height="450" Width="800">
    <Grid>
        <ContentControl prism:RegionManager.RegionName="ContentRegion"/>
    </Grid>
</Window>
{% endhighlight %}


Czym jest RegionAdapter?
To klasa która adaptuje widok do regionu. Używamy ich do umieszczania widoków w konkretnych regionach. Można powiedzieć, że są 


Prism ma wbudowane adaptery które umożliwiają używanie wybranych komponentów jak regiony:
ContentControlRegionAdapters
ItemsControlRegionAdapter
SelectorRegionAdapter
 - ComboBox
 - ListBox
 - Ribbon
 - TabControl

Wszystkie inne kontrolki wymagają stworzenia specjalnego adaptera.

Tworzenie specjalnego adaptera:
a) dziedziczenie RegionAdapterBase<T>
b) nadpisanie metody CreateRegion
c) nadpisanie metody Adapt
d) rejestrowanie adaptera w metodzie ConfigureRegionAdapterMappings

Więcej informacji można znaleźć [tutaj]:[wpf-prism].

[wpf-prism]: https://app.pluralsight.com/library/courses/prism-wpf-introduction/table-of-contents