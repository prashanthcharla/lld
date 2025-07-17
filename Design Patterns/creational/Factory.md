A creational pattern that provides a way to create objects without specifying the exact class of object that will be created. It uses a factory method to return instances of different classes based on input parameters.

#### Key Points:
* Hides object creation logic.
* Returns objects of varying classes that share a common interface or superclass.
* Promotes loose coupling and flexibility.
* One factory, one product type, different variations.
* Useful when you just need one type of object, like picking a vehicle type for a delivery app (e.g., car for Uber).

#### Rules:
* Abstract class or interface must be involved which acts as the type of factory output. Actual output will be the implementation of abstract class or interface.
* Class having factory method should have private constructor. Because we don't want to create an instance of factory. We want to create an instance of the product by factory.

#### Example:

Abstract Class
```java
public abstract class Car {
    public void seats() {
        System.out.println("Regular Seats");
    }

    public void wheels() {
        System.out.println("Regular wheels");
    }

    public abstract void runMechanism();
}
```
Implementations
```java
public class EVVarientCar extends Car {
    @Override
    public void runMechanism() {
        System.out.println("Motors/Batteries");
    }
}

```

```java
public class PetrolDieselVarientCar extends Car {
    @Override
    public void runMechanism() {
        System.out.println("Petrol/Diesel Engine");
    }
}
```
Factory Class
```java
public class CarFactory {
    private CarFactory() {}

    public static Car getCar(String varient) throws IllegalArgumentException {
        return switch (varient) {
            case "EV" -> new EVVarientCar();
            case "PetrolDiesel" -> new PetrolDieselVarientCar();
            default -> throw new IllegalArgumentException("Varient not found");
        };
    }
}
```
Test Factory Method
```java
public class Test {
    public static void main(String[] args) {
        Car evCar = CarFactory.getCar("EV");
        evCar.runMechanism();

        Car petrolDieselCar = CarFactory.getCar("PetrolDiesel");
        petrolDieselCar.runMechanism();
    }
}
```

#### Real-time example:
* Angular's HttpClient
    * Example: The HttpClient is provided by HttpClientModule, and Angular’s DI system creates it using a factory under the hood when you inject it.
        ```ts
        constructor(private http: HttpClient) { }
        ```
    
        Angular decides how to create HttpClient, you just consume it.