---
layout: post
title:  "Web Security - Cross-site request forgery"
date:   2021-06-01 22:05:00 +0100
category: web security cross site request forgery
---

Cross-site forgery request - metoda ataku na serwis internetowy, która często (m.in. na skutek jednoczesnego wykorzystania) mylona jest z cross-site scripting (XSS) bądź jest uznawana za jej podzbiór. Ofiarami CSRF stają się użytkownicy nieświadomie przesyłający do serwera żądania spreparowane przez osoby o wrogich zamiarach. W przeciwieństwie do XSS, ataki te nie są wymierzone w strony internetowe i nie muszą powodować zmiany ich treści. Celem crackera jest wykorzystanie uprawnień ofiary do wykonania operacji w przeciwnym razie wymagających jej zgody. Błąd typu CSRF dotyczy również serwerów FTP.

Aby zapobiec atakowi należy:
- powiązać formularze ze sobą: losowy token w polu hidden jest sprawdzany po stronie serwera, token musi być losowy, nieprzewidywalny,
- wymuszać weryfikację użytkownika przed dostępem do kluczowych akcji,
- można też sprawdzać nagłówek Referer, ale należy pamiętać, że łatwo go zmodyfikować.