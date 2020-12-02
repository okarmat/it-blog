---
layout: post
title:  "Jekyll - Github Pages"
date:   2020-12-02 22:10:00 +0100
categories: jekyll github pages
---
Github Pages zapewnia wsparcie dla stron napisanych przy pomocy Jekyll. Jeśli chcemy opublikować naszą stronę to w repozytorium tworzymy obok głównego brancha `master` jeden dodatkowy np. `gh-pages`. Na branchu pomocniczym w pliku `Gemfile` zakomentowujemy wiersz:
{% highlight ruby %}
gem "jekyll", "~> 4.1.1"
{% endhighlight %}
a następnie kasujemy znak `#` z liniki
{% highlight ruby %}
# gem "github-pages", group: :jekyll_plugins
{% endhighlight %}

Tak swoją drogą to w komentarzu wewnątrz pliku `Gemfile` jest ten tok postępowania zapisany, ale łatwo go przeoczyć.
