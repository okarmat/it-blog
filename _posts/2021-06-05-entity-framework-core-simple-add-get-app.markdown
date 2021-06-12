---
layout: post
title:  "Enitity Framework Core - Simple Add Record Get All Records App"
date:   2021-06-05 12:20:00 +0100
category: enitity framework core get all add record
---
W tym artykule pokażę podstawowe możliwości Entity Framework Core na przykładzie prostej aplikacji dodającej rekord do bazy SqlServer oraz zwracającej wszystkie rekordy.

1.Tworzymy solucję w skład której wchodzą 3 projekty - .Data, .Domain i .UI.

- .Data - zawiera klasę Context,

- .Domain - zawiera klasy modelu,

- .UI - zawiera prezentację graficzną aplikacji np. aplikacja konsolowa.

2.Przygotowanie klasy Context.
{% highlight cs %}
    public class SamuraiContext : DbContext
    {
        public DbSet<Samurai> Samurais { get; set; }
        public DbSet<Quote> Quotes { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer("Data Source= (localdb)\\MSSQLLocalDB; Initial Catalog=SamuraiAppData");
            base.OnConfiguring(optionsBuilder);
        }
    }
{% endhighlight %}

3.Przykładowa aplikacja z dodaniem rekordów i wyświetleniem liczby istniejących rekordów.
{% highlight cs %}
    public class Program
    {
        private static SamuraiContext _context = new SamuraiContext();

        static void Main(string[] args)
        {
            _context.Database.EnsureCreated();
            GetSamurais("Before Add:");
            AddSamurai();
            GetSamurais("After Add:");
            Console.Write("Press any key...");
            Console.ReadKey();
        }

        private static void AddSamurai()
        {
            var samurai = new Samurai { Name = "Sampson" };
            _context.Samurais.Add(samurai);
            _context.SaveChanges();
        }

        private static void GetSamurais(string text)
        {
            var samurais = _context.Samurais.ToList();
            Console.WriteLine($"{ text }: Samurai count is {samurais.Count }");
            foreach (var samurai in samurais)
            {
                Console.WriteLine(samurai.Name);
            }
        }
    }
{% endhighlight %}
