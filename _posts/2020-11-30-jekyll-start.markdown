---
layout: post
title:  "Jekyll - Start"
date:   2020-11-30 22:30:00 +0100
categories: jekyll start
---
Po zainstalowaniu Ruby i Jekyll przechodzimy do utworzenia nowego projektu oraz uruchomienie go.

* Tworzymy nowy projekt.
{% highlight bash %}
jekyll new myblog
{% endhighlight %}

* Przechodzimy do katalogu nowoutworzonego projektu.
{% highlight bash %}
cd myblog
{% endhighlight %}

* Budujemy stronę i sprawiamy, aby była widoczna na serwerze lokalnym.
{% highlight bash %}
bundle exec jekyll serve
{% endhighlight %}

* Podglądamy strone w przeglądarce pod adresem: http://localhost:4000

Poniżej zamieszczam kilka stron które ułatwiają wystartowanie z pisaniem bloga przy pomocy Jekyll.

[Szybki start z Jekyll][jekyll-quick-start]

[Formatowanie dat][jekyll-date-formats]

[Lista wspieranych języków dla taga highlights][jekyll-highlights]

[jekyll-highlights]: https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
[jekyll-quick-start]: https://jekyllrb.com/docs/
[jekyll-date-formats]: https://www.webisland.agency/blog/how-to-format-dates-in-jekyll/