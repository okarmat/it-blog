---
layout: post
title:  "Linux - Sprawdzenie czy hasło zostało zhakowane"
date:   2021-05-29 13:30:00 +0100
category: linux terminal sha1 password hack
---

Za pomocą linuxowego terminalu możemy sprawdzić czy hasło które używam zostało zhakowane.
- Wyznaczamy hash za pomocą funkcji sha1sum.

{% highlight shell %}
echo -n Abc123 | sha1sum
{% endhighlight %}

- Otrzymujemy wartość np. `bec75d2e4e2acf4f4ab038144c0d862505e52d07`. Korzystamy z API udostępnionego przez `pwnedpasswords.com`, ale nie wysyłamy całej sumy kontrolnej, tylko jej początek, aby nasze hasło nie zostało wysłane w świat.
{% highlight shell %}
curl https://api.pwnedpasswords.com/range/bec75 | grep -i bec75d2e4e2acf4f4ab038144c0d862505e52d07
{% endhighlight %}

- W wyniku możemy zobaczyć czy szukane hasło występuje w bazach haseł które wyciekły z różnych serwisów.