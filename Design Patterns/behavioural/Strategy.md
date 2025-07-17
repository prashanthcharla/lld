Behavioural pattern allows you to define family of algorithms (strategies), encapsulate each of it into a separate class, and make them interchangeable at run time.

#### Key Points:
* Problem:
    * You have a class that performs an operation (like sorting, payment, or compression), but the way it does that should be flexible and changeable at runtime.
    * Using switch statement
        ```java
        public class PaymentProcesser {
            public void pay(String type) {
                switch (type) {
                    case "card" -> System.out.println("Paying via card");
                    case "UPI" -> System.out.println("Paying via UPI");
                    case "wallet" -> System.out.println("Paying via wallet");
                }
            }
        }
        ```
* Problem Analysis:
    * Adding new payment type requires modifying the existing code. Violates open/closed principle.
* Solution:
    * Use Strategy pattern.
        * Create a Strategy interface with a common method.
        * Implement concrete strategy (algorithm) classes for each type or variation.
        * A context class (main class which uses different strategies for its functioning) will contain a strategy instance and delegates execution to it.
        * Swap strategies based on the need.
        * It provides a way to have easily testable, reusable algorithms.
    * How its different from State design pattern?
        * In State, we pass context to the State because states trigger transition (behaviour & updating context state).
        * In Strategy, we pass strategy to the context. Context will delegate the work to the chosen strategy. Client can play by plugging in different strategies.

#### Example:
```java
public interface PaymentStrategy {
    void pay();
}
```

```java
public class CardPaymentStrategy implements PaymentStrategy {
    @Override
    public void pay() {
        System.out.println("Paying via card");
    }
}
```

```java
public class UpiPaymentStrategy implements PaymentStrategy {
    @Override
    public void pay() {
        System.out.println("Paying via UPI");
    }
}
```

```java
public class WalletPaymentStrategy implements PaymentStrategy {
    @Override
    public void pay() {
        System.out.println("Paying via wallet");
    }
}
```
Context
```java
public class PaymentProcesser {
    private PaymentStrategy paymentStrategy;

    public PaymentProcesser(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void execute() {
        paymentStrategy.pay();
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        PaymentStrategy cardPaymentStrategy = new CardPaymentStrategy();
        PaymentProcesser paymentProcesser = new PaymentProcesser(cardPaymentStrategy);
        paymentProcesser.execute();

        PaymentStrategy upiPaymentStrategy = new UpiPaymentStrategy();
        paymentProcesser.setPaymentStrategy(upiPaymentStrategy); // Change the strategy dynamically
        paymentProcesser.execute();
    }
}
```

#### Real-time example:
* Spring Boot RestTemplate with Custom Error Handlers
    ```java
    RestTemplate restTemplate = new RestTemplate();
    restTemplate.setErrorHandler(new CustomResponseErrorHandler());
    ```
    * ResponseErrorHandler is a Strategy
    * You can plug different error handling strategies into RestTemplate
* Sorting Strategies in Components
    ```ts
    export interface SortStrategy {
    sort(data: any[]): any[];
    }

    export class NameSortStrategy implements SortStrategy {
    sort(data: any[]): any[] {
        return data.sort((a, b) => a.name.localeCompare(b.name));
    }
    }

    export class DateSortStrategy implements SortStrategy {
    sort(data: any[]): any[] {
        return data.sort((a, b) => a.date - b.date);
    }
    }
    ```
