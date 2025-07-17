Structural design pattern that lets you add new behavior (features) to an object without changing its original class. It wraps the object with extra functionality.

#### Key Points:
* Problem:
    * You have a base object (like Coffee) that can have optional features such as Milk, Sugar, Cream, etc.
    * If you try to handle all combinations using inheritance, you end up with a class explosion like:
        * BaseCoffee
        * MilkCoffee
        * SugarCoffee
        * MilkAndSugarCoffee
        * MilkAndCreamCoffee...and it keeps growing with every new combination.
    * This becomes hard to maintain, extend, and understand.
* Problem Analysis:
    * The issue is that we are mixing core functionality (BaseCoffee) with optional features (Milk, Sugar) into a single class hierarchy.
    * Optional features are orthogonal (independent) to the base object.
    * Every time a new feature (like Whipped Cream) is introduced, it results in creating new classes for all possible combinations.
    * This violates the Open-Closed Principle — the classes are not open for extension but require modification or new classes for every variation.
* Solution - Decorator Design Pattern:
    * Separate the core object (BaseCoffee) from the optional features (Milk, Sugar).
    * Create an abstract Decorator class (so that it can't be instantiated) that implements the same interface as the base class.
    * Each Concrete Decorator wraps the original object and adds its own behavior (e.g., increases cost, modifies description).
    * Multiple decorators can be stacked dynamically at runtime, meaning you can mix and match features without creating new classes for each combination.
* In real time, this pattern allow developers to add features like logging, security, error handling, and request modification without touching core business logic.

#### Example:
```java
public interface Coffee {
    String prepare();
}
```

```java
public class BaseCoffee implements Coffee {
    @Override
    public String prepare() {
        return "Base Coffee";
    }
}
```

```java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String prepare() {
        return coffee.prepare();
    }
}
```

```java
public class MilkDecorator extends CoffeeDecorator {
    MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String prepare() {
        return coffee.prepare() + ", Milk";
    }
}
```

```java
public class SugarDecorator extends CoffeeDecorator{
    SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String prepare() {
        return coffee.prepare() + ", Sugar";
    }
}
```

```java
public class CreamDecorator extends CoffeeDecorator {
    CreamDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String prepare() {
        return coffee.prepare() + ", Cream";
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Coffee coffee = new MilkDecorator(new SugarDecorator(new BaseCoffee()));
        System.out.println(coffee.prepare());
    }
}
```

#### Real-time example:
* Spring Boot — HandlerInterceptor
    * You use HandlerInterceptor to add extra behavior before and after a controller executes.
        ```java
        public class LoggingInterceptor implements HandlerInterceptor {
            public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
                System.out.println("Logging request...");
                return true;
            }
        }
        ```

        The original controller is untouched. You're wrapping it with new behavior (e.g., logging, auth).
* Similatrly, in Angular also we use HttpInterceptor for modifying HTTP requests/responses by keeping core request same.