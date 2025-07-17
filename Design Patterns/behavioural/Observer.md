Behavioural pattern which allows objects (subscribers or observers) to register thier interest in other object's (subject or observable) state changes. When the state changes, all the registered objects will get automatically notified with the changes. It resembles one-to-many relationship. This pattern is helpful when we want changes to one part refelect in other parts of the system automatically.

#### Key Points:
* Problem: 
    * You want multiple objects to be updated whenever another object’s state changes. For example, in a news agency, whenever a new news is published, all channels (or subscribers) should be notified.
    * News agency can directly refer to all the channels for which it has to communicate. 
    
        ```java
        public class NewsAgency {
            private Channel ani = new ANI();
            private Channel tv9 = new TV9();

            public void updateNews(String news) {
                ani.setNews(news);
                tv9.setNews(news);
            }
        }
        ```
* Problem Analysis:
    * Above approach results in below difficulties:
        * Tight coupling occurs if the subject has direct references to all dependent objects.
        * Adding/removing dependent objects requires changes in the subject.
        * Code becomes hard to maintain.
* Solution:
    * Use Observer pattern.
        * The Subject maintains a list of subscribers and provides methods to add/remove them.
        * When the subject’s state changes, it notifies all observers, who handle the update logic themselves.
        * It decouples subject and observers. 

#### Example:
Observers or Subscribers
```java
public interface Channel {
    void setNews(String news);
    String getNews();
}
```

```java
public class TV9 implements Channel {
    private String news;

    @Override
    public void setNews(String news) {
        this.news = news;
    }

    @Override
    public String getNews() {
        return this.news;
    }
}
```

```java
public class ANI implements Channel {
    private String news;

    @Override
    public void setNews(String news) {
        this.news = news;
    }

    @Override
    public String getNews() {
        return this.news;
    }
}
```
Subject or Observable
```java
public class NewsAgency {
    private List<Channel> channels = new ArrayList<>();

    public void addChannel(Channel channel) {
        channels.add(channel);
    }

    public void updateNews(String news) {
        for (Channel channel: channels) {
            channel.setNews(news);
        }
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        NewsAgency newsAgency = new NewsAgency();

        Channel ani = new ANI();
        Channel tv9 = new TV9();

        newsAgency.addChannel(ani);
        newsAgency.addChannel(tv9);

        newsAgency.updateNews("Hi");

        System.out.println(ani.getNews());
        System.out.println(tv9.getNews());
    }
}
```

#### Real-time example:
* Frontend (UI) — DOM Event Listeners
    ```js
    button.addEventListener('click', () => {
        console.log('Button clicked!');
    });
    ```
    * Subject: DOM Element
    * Observer: Callback function (listener)
    * Browser notifies when event happens
* Spring Boot - ApplicationEventPublisher
    ```java
    @Component
    public class OrderService {
        @Autowired
        private ApplicationEventPublisher eventPublisher;

        public void placeOrder(Order order) {
            // Business logic
            eventPublisher.publishEvent(new OrderPlacedEvent(order));
        }
    }
    ```
    ```java
    @Component
    public class InventoryService {
        @EventListener
        public void onOrderPlaced(OrderPlacedEvent event) {
            System.out.println("Updating inventory for order: " + event.getOrder());
        }
    }
    ```
    * Subject: ApplicationEventPublisher
    * Observer: @EventListener annotated method
    * Observer automatically reacts to published events