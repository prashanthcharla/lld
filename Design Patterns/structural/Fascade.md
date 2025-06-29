Structural design pattern that provides a simple interface to a complex system of classes, libraries, or APIs. It hides the complexity by exposing a single entry point with easy-to-use methods.

#### Key Points:
* Problem:
    * Large systems often consist of multiple classes, modules, or services working together.
    * When a client (frontend, API consumer, or UI component) needs to perform an operation, it might require:
        * Calling multiple services.
        * Managing complex sequences of steps.
        * Understanding the internal structure of the system.
* Problem Analysis:
    * The core issue is that the client is forced to interact directly with multiple complex components or services.
    * The client must know:
        * What classes to call.
        * In what order to call them.
        * How to handle their interactions.
    * This increases the complexity in the clients code and leads to harder maintenance and scaling.
* Solution — Facade Design Pattern:
    * Introduce a Facade class, which:
        * Provides a simple, unified interface to a set of complex subsystems.
        * Hides the complexity from the client.
        * Internally handles interactions between different subsystem components.
    * The client interacts only with the Facade, not with subsystems.
* Facade = Simplifier (Interface or Class)
* When to Use Facade:
    * When dealing with complex systems, libraries, APIs, or multiple services.
    * When you want to provide a clean, simple interface to external users or other parts of the system.
    * To reduce coupling between the client code and complex subsystems.

#### Example:
Subsystem Classes (Complex Classes)
```java
public class CPU {
    public void start() {
        System.out.println("CPU Starts");
    }
}
```

```java
public class RAM {
    public void load() {
        System.out.println("Memory loads");
    }
}
```

```java
public class ROM {
    public void spin() {
        System.out.println("Disk spins");
    }
}
```
Fascade
```java
public class Computer {
    private CPU cpu;
    private RAM ram;
    private ROM rom;

    public Computer() {
        this.cpu = new CPU();
        this.ram = new RAM();
        this.rom = new ROM();
    }

    public void startComputer() {
        cpu.start();
        ram.load();
        rom.spin();
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.startComputer();
    }
}
```
