---
layout: post
title:  "WPF - Proces inicjalizacji aplikacji Prism"
date:   2021-03-15 21:52:00 +0100
category: wpf prism
---
Cały proces inicjalizacji aplikacji Prism możemy podzielić na 10 kroków:

1) Konfigurowanie ViewModelLocator
2) Tworzenie Container Extension
3) Rejestrowanie typów
4) Konfigurowanie ServiceLocator
5) Konfigurowanie RegionAdapter Mappings
6) Konfigurowanie Region Behaviors
7) Tworzenie okna Shell
8) Inicjalizowanie Shell
9) Inicjalizowanie modułów
10) Wykonanie metody OnInitialized

Więcej informacji można znaleźć [tutaj]:[wpf-prism].

[wpf-prism]: https://app.pluralsight.com/library/courses/prism-wpf-introduction/table-of-contents