---
layout: post
title:  "Jekyll - Udostępnianie witryny przez Github Pages"
date:   2020-12-02 22:10:00 +0100
categories: jekyll github pages
---
# Instalacja Jekyll
W pierwszej kolejności instalujemy Bundlera.
{% highlight shell %}
$ gem install bundler
{% endhighlight %}

Następnie instalujemy Jekylla.
{% highlight shell %}
$ gem install jekyll
{% endhighlight %}

Aby, założyć katalog jekyll o nazwie `myrepo` używamy `jekyll new`.
{% highlight shell %}
$ jekyll new myrepo
$ cd myrepo
$ git init
{% endhighlight %}

Aby uruchomić serwer aplikacji który udostępni stronę zbudowaną przez Jekyll wywołujemy `jekyll serve`. Jeśli chcemy zbudować stronę dla produkcji.
{% highlight shell %}
$ jekyll build
$ jekyll serve
{% endhighlight %}

[Szczegóły konfiguracji środowiska i tworzenia projektu Jekyll][jekyll-start].

[Szczegóły testowania][jekyll-test].

# Udostępnianie strony za pośrednictwem Github Pages
Github Pages zapewnia wsparcie dla stron napisanych przy pomocy Jekyll. Jeśli chcemy opublikować naszą stronę to w repozytorium tworzymy obok głównego brancha `master` jeden dodatkowy np. `gh-pages`. Na branchu pomocniczym w pliku `Gemfile` zakomentowujemy wiersz:
{% highlight ruby %}
gem "jekyll", "~> 4.1.1"
{% endhighlight %}
a następnie kasujemy znak `#` z liniki
{% highlight ruby %}
# gem "github-pages", group: :jekyll_plugins
{% endhighlight %}

Tak swoją drogą to w komentarzu wewnątrz pliku `Gemfile` jest ten tok postępowania zapisany, ale łatwo go przeoczyć.

[jekyll-start]: http://www.stephaniehicks.com/githubPages_tutorial/pages/githubpages-jekyll.html
[jekyll-test]: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll
