---
layout: post
title:  "Enitity Framework Core - ORM"
date:   2021-05-22 13:40:00 +0100
category: enitity framework core orm
---

EF Core to ORM (Object Relational Mapper) czyli narzędzie do mapowania obiektowo-relacyjnego. Generuje obiekty biznesowe oraz encje zgodnie z tabelami baz danych. Obiekt biznesowy jest encją (entity) w wielowarstowej aplikacji, która działa w połączeniu z dostępem do bazy danych oraz wartwą logiki biznesowej służącą do przesyłania danych. Z kolei nasze entity odnosi się do czegoś co jest unikatowe i istnieje oddzielnie, np. tabela bazy danych.

Entity Framework Core pozwala na:
- Wykonywanie podstawowych operacji CRUD (Create, Read, Update, Delete).
- Łatwe zarządzanie relacjami 1 do 1, 1 do wielu oraz wiele do wielu.
- Tworzenie relacji dziedziczenia pomiędzy encjami.

![]({{ "/assets/images/enitity-framework-core-orm-diagram.jpg" | absolute_url }})

Więcej informacji można znaleźć [tutaj][entity-framework-core].

[entity-framework-core]: https://www.plukasiewicz.net/Artykuly/EntityFramework