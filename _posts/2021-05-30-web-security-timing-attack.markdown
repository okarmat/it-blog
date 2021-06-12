---
layout: post
title:  "Web Security - Timming Attack"
date:   2021-05-30 12:00:00 +0100
category: web security timming attack
---

Atak czasowy (ang. timing attack) – atak hakerski mający na celu wyciek informacji poprzez analizę realizacji zapytania przesłanego do serwera.

Aby przeciwdziałać temu atakowi można istotne elementy systemu wywoływać w sposób asynchroniczny. Np. podczas resetowania hasła zwrócić komunikat: `"Jeśli Twój mail istnieje w naszej bazie to został na niego wysłany link generujący nowe hasło"` a samo wysłanie wykonujemy asynchronicznie - np. wykonując kolejkę rozsyłającą maile co 10 minut.