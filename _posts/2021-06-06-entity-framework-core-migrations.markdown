---
layout: post
title:  "Enitity Framework Core - Migrations"
date:   2021-06-06 21:40:00 +0100
category: enitity framework core migrations
---

# Mechanizm migracji

Na poniższym obrazku możemy zobaczyć cały mechanizm działania migraji:

![]({{ "/assets/images/enitity-framework-core-migrations-basic-workflow.jpg" | absolute_url }})

1.Zdefiniowanie modelu.

2.Wyrażenie i wykonanie zapytania. 

3.Przygotowanie i wykonanie zapytania SQL ("SELECT..."). 

4.EF transforumuje wyniki do typów modelu. 

5.Użytkownik modyfikuje dane. 

6.Zapisywanie zmian.

7.Przygotowanie i wykonanie zapytań SQL ("UPDATE...").

# Pierwsza migracja

{% highlight bash %}
add-migration init    
{% endhighlight %}

# Tworzenie modelu na podstawie bazy danych (tzw. Reverse Engineering an Existing Database)

{% highlight bash %}
scaffold-dbcontext -provider Microsoft.EntityFrameworkCore.SqlServer -connection "Data Source = (LocalDB)\MSSQLLocalDB;Initial Catalog = SamuraiAppData"  
{% endhighlight %}
