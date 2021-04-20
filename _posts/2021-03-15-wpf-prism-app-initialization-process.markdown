---
layout: post
title:  "WPF - Proces inicjalizacji aplikacji Prism"
date:   2021-03-15 21:52:00 +0100
category: wpf prism
---
Cały proces inicjalizacji aplikacji Prism możemy podzielić na 10 kroków:

* Konfigurowanie ViewModelLocator
* Tworzenie Container Extension
* Rejestrowanie typów
* Konfigurowanie ServiceLocator
* Konfigurowanie RegionAdapter Mappings
* Konfigurowanie Region Behaviors
* Tworzenie okna Shell
* Inicjalizowanie Shell
* Inicjalizowanie modułów
* Wykonanie metody OnInitialized

Więcej informacji można znaleźć [tutaj][wpf-prism].

[wpf-prism]: https://app.pluralsight.com/library/courses/prism-wpf-introduction/table-of-contents