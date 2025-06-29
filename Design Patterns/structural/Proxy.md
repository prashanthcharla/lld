Its a structural design pattern where a proxy is a substitute for another object. It controls access to the real object by sitting in front of it and intercepting requests.

#### Key Points:
* Problem:
    * Sometimes, direct access to an object is:
        * Not safe (e.g., sensitive operations),
        * Not efficient (e.g., creating heavy objects unnecessarily),
        * Or needs additional behavior (e.g., logging, access control, caching).
    * For example:
        * You don't want every user to access restricted websites.
        * Or, you don't want to create an expensive database connection unless it's actually needed.
* Solution — Proxy Design Pattern:
    * Create a Proxy class that:
        * Implements the same interface as the real object.
        * Controls access to the real object.
        * Can add extra behavior like logging, security, lazy initialization, caching, etc.
    * The client interacts with the Proxy, which then decides whether and how to pass the request to the Real Object.
* Proxy = Middleman / Gatekeeper.
* When to Use Proxy:
    * Access Control: Restrict who can access the real object (e.g., security).
    * Lazy Initialization: Delay creating expensive objects until needed.
    * Logging / Monitoring: Log actions without changing the real object.
    * Caching: Store results and avoid repeated expensive operations.

#### Example:
```java
public interface Internet {
    void connectTo(String url) throws Exception;
}
```

```java
public class RealInternet implements Internet {
    @Override
    public void connectTo(String url) {
        System.out.println("Connected to "+url);
    }
}
```

```java
public class ProxyInternet implements Internet {
    private Internet internet = new RealInternet();
    private static List<String> blockedSites;

    static {
        blockedSites = new ArrayList<>();
        blockedSites.add("example.com");
        blockedSites.add("test.com");
    }


    @Override
    public void connectTo(String url) throws Exception {
        if (blockedSites.contains(url)) {
            throw new Exception(url+" : Site blocked");
        }

        internet.connectTo(url);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Internet internet = new ProxyInternet();

        try {
            internet.connectTo("mysite.com");
            internet.connectTo("example.com");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```
