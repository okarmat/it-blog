---
layout: post
title:  "Web Security - SQL Injection"
date:   2021-05-30 13:00:00 +0100
category: web security sql injection
---

SQL Injection - metoda ataku komputerowego wykorzystująca lukę w zabezpieczeniach aplikacji polegającą na nieodpowiednim filtrowaniu lub niedostatecznym typowaniu danych użytkownika, które to dane są później wykorzystywane przy wykonaniu zapytań (SQL) do bazy danych. Podatne są na nią wszystkie systemy przyjmujące dane od użytkownika i dynamicznie generujące zapytania do bazy danych.

Aby zabezpieczyć się przed tym atakiem możemy:
- stosować parametry zamiast sklejać zmienne jak zwykłe typy string,
- stosować procedury składowane,
- ograniczać uprawnienia użytkowników do odczytu wąskiej grupy tablic i widoków