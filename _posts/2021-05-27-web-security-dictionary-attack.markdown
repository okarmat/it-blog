---
layout: post
title:  "Web Security - Dictionary Attack"
date:   2021-05-27 13:33:00 +0100
category: web security dictionary attack
---

Atak słownikowy (ang. dictionary attack) – technika używana do siłowego odgadywania kluczy kryptograficznych i haseł do systemów, w swoim działaniu zbliżona do metody brute force. Główna różnica między stosowanymi podejściami polega na sposobie dobierania haseł, które podlegają sukcesywnemu testowaniu. O ile w czystym ataku brute force bada się kolejno wszystkie dopuszczalne kombinacje klucza w celu odnalezienia właściwego wzoru, o tyle ataki słownikowe bazują tylko na sprawdzeniu niewielkiego podzbioru możliwych wartości, co do którego wiadomo, że jest niewspółmiernie często stosowany przy doborze haseł – na przykład listy słów występujących w danym języku albo historycznej bazy popularnych haseł.

Aby przeciwdziałać takim atakom możemy:
- ograniczyć listę prób logowań,
- wymagać złożonych haseł podczas rejestracji lub zmiany hasła,
- zastosować captche,
- dwuskładnikowe uwierzytelnienie,
- odstęp czasu pomiędzy próbami logowania,
- wymagać, aby login był inny niż email,
- prowadzenie blacklisty haseł,
- ograniczenie możliwości logowań z podejrzanych miejsc,
- inteligentne komunikaty o błędach.
