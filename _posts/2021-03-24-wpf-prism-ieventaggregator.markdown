---
layout: post
title:  "WPF - IEventAggregator"
date:   2021-03-24 21:20:00 +0100
category: wpf prism ieventaggregator event events
---

# Co to jest IEventAggregator?
`IEventAggregator` to system komunikacji eventów stworzony na zasadach luźnych powiązań (loosly coupled event-based communication). Umożliwia nam na wysłanie eventów z jednego widoku, widoku modelu lub obiektu do innego lub innych widoków, widoków modelu lub obiektów. Działa to na zasadzie nadawców i odbiorców eventów. Czyli jedna część Twojej aplikacji nadaje event a druga go odbiera. Możemy mieć wielu nadawców i wielu odbiorców eventów. Eventy umożliwiają przekazywanie parametrów. IEventAggregator zabezpiecza przed wyciekiem pamięci. Nie trzeba więc pamiętać, aby usunąć eventa w momencie kiedy już nie jest potrzebny. Jednym wyjątkiem od tej reguły jest sytuacja kiedy użyjemy `keepSubscriberReferenceAlive`. Wówczas musimy ręcznie zaprzestac subskrybcji eventa.

{% highlight cs %}
public class MessageInputViewModel : BindableBase
{
    private string _message = "Message to Send";
    private readonly IEventAggregator _eventAggregator;

    public string Message
    {
        get { return _message; }
        set { SetProperty(ref _message, value); }
    }

    public DelegateCommand SendMessageCommand { get; private set; }

    public MessageInputViewModel(IEventAggregator eventAggregator)
    {
        SendMessageCommand = new DelegateCommand(SendMessage);
        _eventAggregator = eventAggregator;
    }

    private void SendMessage()
    {
        _eventAggregator.GetEvent<MessageSentEvent>().Publish(Message);
    }
}
{% endhighlight %}

{% highlight cs %}	
public class MessageListViewModel : BindableBase
{
    private ObservableCollection<string> _messages = new ObservableCollection<string>();
    public ObservableCollection<string> Messages
    {
        get { return _messages; }
        set { SetProperty(ref _messages, value); }
    }

    public MessageListViewModel(IEventAggregator eventAggregator)
    {
        eventAggregator.GetEvent<MessageSentEvent>().Subscribe(OnMessageReceived);
    }

    private void OnMessageReceived(string message)
    {
        Messages.Add(message);
    }
}
{% endhighlight %}

# Filtering Events
Może sie zdażyć sytuacja, że nie wszystkie instancje publikowanych eventów będą potrzebne. Z pomocą przychodzi nam funkcjonalność `Filtering Events`.

{% highlight cs %}	
public class MessageListViewModel : BindableBase
{
    private ObservableCollection<string> _messages = new ObservableCollection<string>();
    public ObservableCollection<string> Messages
    {
        get { return _messages; }
        set { SetProperty(ref _messages, value); }
    }

    public MessageListViewModel(IEventAggregator eventAggregator)
    {
        eventAggregator.GetEvent<MessageSentEvent>().Subscribe(OnMessageReceived, 
            ThreadOption.PublisherThread, false,
            message => message.Contains("Brian"));
    }

    private void OnMessageReceived(string message)
    {
        Messages.Add(message);
    }
}
{% endhighlight %}

# Ręczne zaprzestanie subskrybcji eventów
Domyślnie Prism sam pilnuje, aby zaprzestawać subskrybcji eventów które nie są już potrzebne aplikacji. Jednak niekiedy zachodzi potrzeba, żeby zaprzestać przesyłania wiadomości za pośrednictwem IEventAggregator w określonej chwili. Po prostu w którymś momencie życia aplikacji dochodzimy do wniosku: nie ma już potrzeby, aby informować jedną część aplikacji o zmianach zachodzących w drugiej części. Drugą sytuacją w której potrzebne jest ręczne zaprzestanie subskrybcji to scenariusz w któym ustawiamy `keepSubscriberReferenceAlive` na true. Wówczas musimy sami zadbać o skasowanie subskrybcji. Prism daje możliwośc obsługi takich specjalnych scenariuszy.

# Unsubscribe with Delegate Method

{% highlight cs %}
public class MessageListViewModel : BindableBase
{
    MessageSentEvent _event;

    private ObservableCollection<string> _messages = new ObservableCollection<string>();
    public ObservableCollection<string> Messages
    {
        get { return _messages; }
        set { SetProperty(ref _messages, value); }
    }

    private bool _isSubscribed = true;
    public bool IsSubscribed
    {
        get { return _isSubscribed; }
        set 
        { 
            SetProperty(ref _isSubscribed, value);

            HandleSubscribe(_isSubscribed);
        }
    }

    public MessageListViewModel(IEventAggregator eventAggregator)
    {
        _event = eventAggregator.GetEvent<MessageSentEvent>();

        HandleSubscribe(true);
    }

    void HandleSubscribe(bool isSubscribed)
    {
        if (isSubscribed)
            _event.Subscribe(OnMessageReceived);
        else
            _event.Unsubscribe(OnMessageReceived);
    }

    private void OnMessageReceived(string message)
    {
        Messages.Add(message);
    }
}
{% endhighlight %}

# Unsubscribe with token

{% highlight cs %}
public class MessageListViewModel : BindableBase
{
    MessageSentEvent _event;

    private ObservableCollection<string> _messages = new ObservableCollection<string>();
    public ObservableCollection<string> Messages
    {
        get { return _messages; }
        set { SetProperty(ref _messages, value); }
    }

    private bool _isSubscribed = true;
    public bool IsSubscribed
    {
        get { return _isSubscribed; }
        set 
        { 
            SetProperty(ref _isSubscribed, value);

            HandleSubscribe(_isSubscribed);
        }
    }

    public MessageListViewModel(IEventAggregator eventAggregator)
    {
        _event = eventAggregator.GetEvent<MessageSentEvent>();

        HandleSubscribe(true);
    }

    SubscriptionToken _token = null;

    void HandleSubscribe(bool isSubscribed)
    {
        if (isSubscribed)
            _token = _event.Subscribe(OnMessageReceived);
        else
            _event.Unsubscribe(_token);
    }

    private void OnMessageReceived(string message)
    {
        Messages.Add(message);
    }
}
{% endhighlight %}