---
layout: post
title:  "Jekyll - Install"
date:   2020-11-30 22:15:00 +0100
categories: jekyll install
---
# Instalowanie Jekyll'a na Windowsie.
* Zainstalować środowisko Ruby [RubyInstaller][jekyll-ruby-installer]
* Zainstalować Jekyll wywołując w linii komend `gem install jekyll bundler`.
* Sprawdzenie czy Jekyll zainstalował się prawidłowo wpisując w linii komend `jekyll -v`.
* Z racji, że mój blog udostępniam za pośrednictwem GitHub Pages konieczne jest sprawdzenie na jakim branchu jesteśmy. Lokalnie należy pracować na masterze a zmiany wypychać na gh-pages.

# Typowe błędy które pojawiają mi się po przeinstalowaniu systemu i ponownym instalowaniu Jekylla:
* błąd:
{% highlight bash %}
cannot load such file -- webrick (LoadError)
{% endhighlight %}

- rozwiązanie:
{% highlight bash %}
bundle add webrick
{% endhighlight %}



Więcej informacji na temat instalacji Jekyll na Windowsie znajdziesz [tutaj][jekyll-install-prerequisites]. 

[jekyll-ruby-installer]: https://rubyinstaller.org/downloads/
[jekyll-install-prerequisites]: https://jekyllrb.com/docs/installation/windows/