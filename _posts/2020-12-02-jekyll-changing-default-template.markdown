---
layout: post
title:  "Jekyll - Modyfikowanie domyślnego szablonu"
date:   2020-12-02 22:40:00 +0100
categories: jekyll default template
---
Po utworzeniu nowego projektu Jekyll otrzymujemy aplikację z domyślnym szablonem o nazwie `Minima`. Niekiedy zachodzi potrzeba wprowadzenia w nim zmian np. jeśli chcemy zmienić wygląd strony głównej czy schematu dla strony bazowej artykułów. Pliki domyślnego szablonu nie są zapisane wewnątrz katalogu z nowy projektem, lecz znajdują się w folderze głównym `Ruby`. W moim przypadku ma on adres: `C:\Ruby27-x64\lib\ruby\gems\2.7.0\gems\minima-2.5.1\_layouts`.

Jeśli chcemy zmodyfikować wygląd strony głównej to musimy do projektu dodać katalog `_layouts` a następnie skopiować do niego plik o nazwie `home.html`. W ten sposób plik `home.html` w katalogu `Ruby` zostanie podczas budowania strony zastąpiony plikiem który zmodyfikowaliśmy.