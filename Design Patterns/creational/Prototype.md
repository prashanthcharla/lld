It creates new objects by cloning an existing object (the prototype) instead of creating from scratch. It’s useful when object creation is costly or complex.

#### Key Points:
* Copy an existing object to create a new one with the same properties, then modify as needed.
* Use this pattern only when object creation is heavy or complex.
* When you need a centralized repository of prototypes, use a Registry (singleton class) with a HashMap to store prototype instances and a static method to retrieve clones.

#### Example:

```java
public abstract class Vehicle implements Cloneable {
    private String brand;
    private String color;
    private int speed;

    public Vehicle(String brand, String color, int speed) {
        this.brand = brand;
        this.color = color;
        this.speed = speed;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }

    public String getBrand() {
        return brand;
    }

    public String getColor() {
        return color;
    }

    public int getSpeed() {
        return speed;
    }

    @Override
    public Vehicle clone() throws CloneNotSupportedException {
        return (Vehicle) super.clone();
    }

    @Override
    public String toString() {
        return "Vehicle{" +
                "brand='" + brand + '\'' +
                ", color='" + color + '\'' +
                ", speed=" + speed +
                '}';
    }
}
```

```java
public class TwoWheeler extends Vehicle {
    private boolean hasSideStand;
    private String handlebarType;

    public TwoWheeler(String brand, String color, int speed, boolean hasSideStand, String handlebarType) {
        super(brand, color, speed);
        this.hasSideStand = hasSideStand;
        this.handlebarType = handlebarType;
    }

    public void setHandlebarType(String handlebarType) {
        this.handlebarType = handlebarType;
    }

    public void setHasSideStand(boolean hasSideStand) {
        this.hasSideStand = hasSideStand;
    }

    public boolean isHasSideStand() {
        return hasSideStand;
    }

    public String getHandlebarType() {
        return handlebarType;
    }

    @Override
    public TwoWheeler clone() throws CloneNotSupportedException {
        return (TwoWheeler) super.clone();
    }

    @Override
    public String toString() {
        return "TwoWheeler{" +
                "hasSideStand=" + hasSideStand +
                ", handlebarType='" + handlebarType + '\'' +
                '}';
    }
}
```

```java
public class FourWheeler extends Vehicle {
    private int numberOfDoors;
    private boolean isAutomatic;

    public FourWheeler(String brand, String color, int speed, int numberOfDoors, boolean isAutomatic) {
        super(brand, color, speed);
        this.numberOfDoors = numberOfDoors;
        this.isAutomatic = isAutomatic;
    }

    public void setAutomatic(boolean automatic) {
        isAutomatic = automatic;
    }

    public void setNumberOfDoors(int numberOfDoors) {
        this.numberOfDoors = numberOfDoors;
    }

    public int getNumberOfDoors() {
        return numberOfDoors;
    }

    public boolean isAutomatic() {
        return isAutomatic;
    }

    @Override
    public FourWheeler clone() throws CloneNotSupportedException {
        return (FourWheeler) super.clone();
    }
}
```

```java
public class VehicleRegistry {
    private static volatile Map<String, Vehicle> vehicleMap;

    private VehicleRegistry() {}

    public static void register(String type, Vehicle vehicle) {
        if(vehicleMap == null) {
            synchronized (VehicleRegistry.class) {
                if(vehicleMap == null) {
                    vehicleMap = new HashMap<>();
                }
            }
        }

        if(type != null && vehicle != null) {
            vehicleMap.put(type, vehicle);
        } else {
            throw new IllegalArgumentException("One or more args are null");
        }
    }

    public static Optional<Vehicle> getVehicle(String type) throws CloneNotSupportedException {
        return vehicleMap.containsKey(type) ? Optional.of(vehicleMap.get(type).clone()) : Optional.empty();
    }
}
```

```java
public class Test {
    public static void main(String[] args) throws CloneNotSupportedException {
        // Register prototypes
        VehicleRegistry.register("Two", new TwoWheeler("Bajaj", "Red", 100, true, "Straight"));
        VehicleRegistry.register("Four", new FourWheeler("Bajaj", "Red", 100, 4, false));

        // Use prototype to create new object
        Optional<Vehicle> twoWheeler = VehicleRegistry.getVehicle("Two");
        if(twoWheeler.isPresent()) {
            TwoWheeler twoWheeler1 = (TwoWheeler) twoWheeler.get();
            // Modify as necessary
            twoWheeler1.setHandlebarType("Cross");
            System.out.println(twoWheeler);
        }
    }
}
```

#### Real-time example:
* In Spring, when you define a bean with @Scope("prototype"), Spring creates a new instance every time you request the bean.
    ```java
    @Component
    @Scope("prototype")
    public class Report {
        // Some fields and methods
    }
    ```

    ```java
    @Autowired
    private ApplicationContext context;

    public Report createReport() {
        return context.getBean(Report.class);  // New instance every time — Prototype Pattern
    }
    ```
* Whenever you use @Scope("prototype") in Spring, you're practically applying the Prototype Design Pattern without writing cloning logic yourself.