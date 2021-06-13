---
layout: post
title:  "UML - diagramy strukturalne"
date:   2021-06-13 12:00:00 +0100
category: software design uml
---

Diagramy strukturalne pomagają nam zdefiniować ogólną strukturę lub architekturę naszego systemu, podobnie jak plan definiuje wygląd domu. Modele schematów strukturalnych określają jak nasz system będzie wyglądał architektonicznie. Pomagają nam w zdefniowaniu słownictwa systemu, zapewniają, że różni interesariusze projektu rozumieją się nawzajem w rozmowach na temat rzeczy w systemie. Identyfikują różne relacje między częściami systemu i pomagają nam zdefiniować całą architekturę systemu.

# Diagramy klas

Diagramy klasy są prawdopodobnie najczęstszym diagramem UML który można zobaczyć podczas pracy nad tworzeniem oprogramowania w systemach modelowania. Jest to najczęściej rysowany diagram. Dzięki niemu możemy wprowadzić jednolite słownictwo, nazewnictwo między wszystkimi członkami zespołu.

![]({{ "/assets/images/uml-basics-class-diagram.png" | absolute_url }})

Klasa BillPayer ma dwie operacje - SchedulePayment() i ImmediatePayment(). Widzimy zarówno zależność od Account jak i od Payee. Widzimy, że Customer jest powiązany z Payee w postaci reprezentacji agregacji a Address jest powiązany z Customer w sposób kompozycyjny. Widzimy również, że Customer jest powiązany również z Account, ponieważ klient ma konto i widziemy, że jest tutaj relacja wiele do wielu. Tą relację nazwailśmy Account Holder. Account które jest oznaczone kursywą jest klasą abstrakcyjną i ma dwie implementacje - Checking Account i Saving Account. Na koniec warto zauważyć, że umieściliśmy notatkę na tym schemacie która ma stereotyp Future, czyli jest to opis który jest planem na przyszłość. To tylko notatka dla programisty i innych interesariuszy.