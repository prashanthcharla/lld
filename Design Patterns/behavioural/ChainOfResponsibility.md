Behavioural pattern where a request is passed along chain of handlers, each handler decides whether to process the request or pass it to the next handler.

#### Key Points:
* Problem:
    * You want to perform multiple checks on a request (e.g., user login), such as:
        * Is username present?
        * Is password valid?
        * Is user active?
* Problem Analysis:
    * To perform above checks, you'd write nested or multiple if-else blocks which is inefficient & hard to understand.
* Solution:
    * Create handlers.
        * Each handler is responsible for one check.
        * Each handler either
            * Handles the request (return the final result of chain). (or)
            * Pass it to the next handler.
* Each handler has a single responsibility.
* This pattern obeys single responsibility principle, open/closed principle (you can add a new handler without modifying the existing ones).
* Easily scalable & maintainable.

#### Example:
```java
public abstract class Handler {
    protected Handler next;

    public Handler setupNext(Handler next) {
        this.next = next;
        return next;
    }

    public abstract boolean handle(String userName, String password);
}
```

```java
public class UserNameHandler extends Handler {
    @Override
    public boolean handle(String userName, String password) {
        System.out.println("In UserNameHandler");
        if(userName == null) {
            System.out.println("User name is null");
            return false;
        }

        return next == null || next.handle(userName, password);
    }
}
```

```java
public class PasswordHandler extends Handler{
    @Override
    public boolean handle(String userName, String password) {
        System.out.println("In PasswordHandler");
        if(password == null) {
            System.out.println("Password is null");
            return false;
        }

        return next == null || next.handle(userName, password);
    }
}
```

```java
public class UserActiveHandler extends Handler{
    @Override
    public boolean handle(String userName, String password) {
        System.out.println("In UserActiveHandler");
        System.out.println("User is active");
        return true;
    }
}
```
Usage:
```java
public class Main {
    public static void main(String[] args) {
        Handler handler = new UserNameHandler();
        handler.setupNext(new PasswordHandler());

        System.out.println(handler.handle("hgf", null));
    }
}
```
