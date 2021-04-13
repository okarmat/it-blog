---
layout: post
title:  "GIT - fetch vs pull"
category: 
---
Jaka jest różnica pomiędzy fetch a pull? Zanim wskażę różnice warto podkreślić ich podobieństwo. Zarówno fetch jak i pull mają za zadania pobrać z repozytorium nowe dane. Odświeżanie danych jest niezbędne w codziennej pracy, ponieważ to co mamy w lokalnym repozytorium to tylko migawka ("snapshot").

Fetch pobiera tylko nowe dane ze zdalnego repozytorium, ale nie łącze je z naszymi plikami roboczymi. Fetch nadaje się idealnie do otrzymania świeżej dawki informacji o tym co dzieje się zdalnym repozytorium. Z racji, że jest nieszkodliwy - nie może "popsuć" danych możemy użyć go za dużo.

Pull ma na celu zaktualizowanie aktualnego HEAD branch poprzez wgranie ostatnich zmian ze zdalnego serwera. Oznacza to, że nie tylko pobiera dane, ale również integruje je z naszymi plikami roboczymi. Oznacza to, że podczas pullowania mogą wystąpić tzw. "merge conflict" które trzeba będzie rozwiązać. W związku z tym najlepiej korzystać z pull tylko wówczas, gdy mamy zacommitowane wszystkie nasze zmiany.

Więcej informacji można znaleźć [tutaj]:[git-fetch-vs-pull].

[git-fetch-vs-pull]: https://www.git-tower.com/learn/git/faq/difference-between-git-fetch-git-pull/