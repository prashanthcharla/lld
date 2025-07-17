A creational pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. It’s like a factory of factories, used when you need to create multiple related objects.

#### Key Points:
* Creates families of objects with a common theme.
* Abstracts the creation process.
* Abstract factory is a factory that produces multiple related products, ensuring they’re compatible (e.g., Tesla vehicle with Tesla accessories).
* Allows switching between the families.
* Useful when you need a set of related objects, like equipping a car with brand-specific parts (e.g., Tesla’s car with Tesla’s tire that serves as backup tire).

#### Example:

Below interface & implementation classes can be used to create Car factory.
```java
public interface Car {
    void drive();
}
```

```java
public class Tesla implements Car {
    @Override
    public void drive() {
        System.out.println("Driving Tesla Car");
    }
}
```

```java
public class Toyota implements Car {
    @Override
    public void drive() {
        System.out.println("Driving Toyota Car");
    }
}
```
Below interface & implementation classes can be used to create Tire factory.
```java
public interface Tire {
    void describe();
}
```

```java
public class TeslaTire implements Tire {
    @Override
    public void describe() {
        System.out.println("Accessory: Tesla Tyre");
    }
}
```

```java
public class ToyotaTire implements Tire {
    @Override
    public void describe() {
        System.out.println("Accessory: Toyota Tyre");
    }
}
```
Below interace can be used to create an abstract factory which combines the related objects together. 
```java
public interface CarBrandFactory {
    Car createCar();
    Tire createTire();
}
```
Tesla Abstract Factory Implementation
```java
public class TeslaFactory implements CarBrandFactory {
    @Override
    public Car createCar() {
        return new Tesla();
    }

    @Override
    public Tire createTire() {
        return new TeslaTire();
    }
}
```
Toyota Abstract Factory Implementation
```java
public class ToyotaFactory implements CarBrandFactory {
    @Override
    public Car createCar() {
        return new Toyota();
    }

    @Override
    public Tire createTire() {
        return new ToyotaTire();
    }
}
```

```java
public class Test {
    public static void assemble(CarBrandFactory brand) {
        Car car = brand.createCar();
        Tire tyre = brand.createTire();

        car.drive();
        tyre.describe();
    }

    public static void main(String[] args) {
        CarProduction.assemble(new TeslaFactory());

        // Switch between the families of related objects
        CarProduction.assemble(new ToyotaFactory());
    }
}
```

#### Real-time example:
* In Spring Data JPA, when you enable JPA, Spring Boot uses:
    * EntityManagerFactory to manage entities
    * JpaTransactionManager to manage transactions
* Spring Boot autoconfiguration creates/configures both using shared config (like DataSource, JpaProperties)


