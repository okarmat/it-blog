---
layout: post
title:  "Oracle - Startup and Shutdown"
date:   2021-04-23 07:50:00 +0100
category: oracle shutdown startup 
---

Baza Oracle jak każde inne oprogramowanie może czasami odmówić posłuszeństwa. Najlepiej ją jest wówczas zrestartować logując się do sqlplus jako sysdba.
Opcja `immediate` w wywołaniu `shoutdown` zamyka wszystkie otwarte sesje niezależnie od tego czy użytkownik wyrazi na to zgodę czy nie.

Więcej informacji można znaleźć [tutaj]:[oracle-restart].

{% highlight bash %}
sqlplus / as sysdba
shutdown immediate
startup
{% endhighlight %}

[oracle-restart]: https://docs.oracle.com/cd/B19306_01/server.102/b14231/start.htm