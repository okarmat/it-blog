---
layout: post
title:  "Linux - Telnet"
date:   2020-12-03 08:52:00 +0100
categories: linux telnet
---
Niekiedy podczas testowania działania aplikacji występują problemy z komunikacją pomiędzy klientem a serwerem. W systemie Linux mamy do dyspozycji narzędzie `telnet` które pozwala sprawdzać dostępność adresu oraz portu pod którym aplikacja powinna być dostępna. Więcej informacji można znaleźć [tutaj][linux-telnet].

{% highlight linux %}
telnet {host} {port}
telnet www.cyberciti.biz 80
telnet 10.10.4.208 8888
{% endhighlight %}

[linux-telnet]: https://www.cyberciti.biz/faq/ping-test-a-specific-port-of-machine-ip-address-using-linux-unix/
