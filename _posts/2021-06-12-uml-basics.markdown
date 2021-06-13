---
layout: post
title:  "UML - podstawy"
date:   2021-06-12 12:55:00 +0100
category: software design uml
---

## Definicja 

Unified Modeling Language (UML, zunifikowany język modelowania) – język pół-formalny wykorzystywany do modelowania różnego rodzaju systemów, stworzony przez Grady’ego Boocha, Jamesa Rumbaugha oraz Ivara Jacobsona, obecnie rozwijany przez Object Management Group.

Służy do modelowania dziedziny problemu (opisywania-modelowania fragmentu istniejącej rzeczywistości – na przykład modelowanie tego, czym zajmuje się jakiś dział w firmie) – w przypadku stosowania go do analizy oraz do modelowania rzeczywistości, która ma dopiero powstać – tworzy się w nim głównie modele systemów informatycznych. UML jest przeważnie używany wraz ze swoją reprezentacją graficzną – jego elementom przypisane są odpowiednie symbole wiązane ze sobą na diagramach.

UML jest oficjalnie zdefiniowany przez Object Management Group (OMG) w tzw. metamodelu UML – Meta-Object Facility (MOF). Jak inne specyfikacje bazujące na Meta-Object Facility, metamodel UML i modele UML mogą być serializowane (zapisywane) w języku XML Metadata Interchange (XMI), opartym na standardzie XML. Choć UML był zaprojektowany, by definiować, wizualizować, konstruować i dokumentować systemy kładące nacisk na oprogramowanie, nie jest on ograniczony do modelowania oprogramowania. UML jest używany także do modelowania procesów biznesowych, inżynierii systemów i reprezentowania struktur organizacyjnych. Systems Modeling Language (SysML, Język Modelowania Systemów) jest językiem modelowania dla specyficznych zagadnień inżynierii systemów, zdefiniowanym jako profil UML 2.0. W UML-u do opracowywania formalnych ograniczeń można wykorzystać także język Object Constraint Language (OCL) opracowany pierwotnie przez IBM.

## Modele

# Pudełka

Klasa - jest prostokątnym pudełkiem podzielonym na wiele sekcji. Ma nazwę, atrybuty, kilka operacji.

Przypadek użycia - jest elipsą z nazwą. Jest funkcją którą oczekujemy od systemu.

Komponent - prostokąt z ikonką w prawym górnym rogu. To miejsce w którym spotykają się różne elementy systemu i używamy ich do tworzenia podłączanych lub wymiennych części naszego oprogramowania.

Węzeł - jest szcześcianem z nazwą. Najczęściej są używane w diagramach wdrożenia. Oznacza coś fizycznego np. serwer bazy danych.

# Wiadomości i akcje

Zwykła wiadomość -  ciągła linia wskazująca dzwoniączego na wywoływacza, która ma wypełniony grot strzałki wskazującą na wywoływacza.

Wiadomość zwrotna - przerywana linia, która ma grot otwarty wskazujący wstecz od wywoływacza do osoby dzwoniącej, reprezentuje zwracane dane lub zwracaną kontrolę.

Wiadomość asynchroniczna - linia ciągła, która ma grot otwarty, wywołujący nawiązuje połączenie z uczestnikiem i może kontynuować przetwarzanie.

Stany - prostokąt z zaokrąglonymi rogami i nazwą, reprezentuje stan w którym może się obiekt znaleźć.

Akcje - niestety akcje są bardzo podobne do stanów - także mają zaokrąglone rogi i nazwę. Rozróżnić je można po nazwie i/lub kontekście.

![]({{ "/assets/images/uml-basics-message-arrows.jpg" | absolute_url }})

# Relacje

Stowarzyszenie (association) - linia ciągła pomiędzy dwoma bytami, często są to klasy, ale niekoniecznie. Dodatkowo na początku i końcu linii występują liczby który oznaczać będą wielość, czyli określają ile z jednej lub drugiej strony relacji jest dozwolone. Może się też pojawić gwiazdka która oznacza od 0 do wielu.

Generalizacja (generalization) - linia ciąła ze strzałką której grot jest zamknięty, ale pusty, tworzy specjalizację klasy w której dziedziczymy z jednej klasy do drugiej i tworzymy jej wyspecjalizowaną wersję. Ta zamknięta grot wskazuje na rodzica lub klasę z której czerpiemy drugą.

Realizacja, absorbcja (implementation) - linia przerywana ze strzałką której grot jest zamknięty, ale pusty, symbolizuje implementowanie interfejsu lub wdrażanie niektórych funkcji. Strzałka wskazuje na definicję, a strona, która nie jest strzałką, łączy się z miejscem realizacji.

Zależność (dependency) - linia przerywana z otwartym grotem, oznacza pewien związek w którym jeden obiekt jest zależny od drugiego. Obiekt który jest wskazywany grotem jest tym, na którym drugi jest zależny.

![]({{ "/assets/images/uml-basics-relation-arrows.jpeg" | absolute_url }})

# Rozszerzenia

Notatki - wyglądają jak małe notki z rozłożonymi krawędziami. Służy do zapisu dodatkowych informacji na diagramie.
 
Stereotyp - zapisywany jako np. <<Interface>> i wówczas identyfikuje on element na diagramie jako interfejs, ale istnieje wiele różnych stereotypów.


Więcej informacji można znaleźć [tutaj][uml-basics].


[uml-basics]: https://www.p-programowanie.pl/uml/diagramy-klas-uml
