# SOLID

## S - Single Responsibility Principle
A class should be have single responsibility. To detect if a class is violating this principle, just ask yourself what this class is doing? If you encounter "and", it's an indication that it handling multiple responsibilities.

Example:
```java
    class Account {
        private int id;
        private int accHolderName;
        private Double balance;

        // getters

        // setters
    }

    class AccountOperations {
        void addAccount() {

        }

        void updateAccount() {

        }

        void deposit(int amount) {

        }
    }
```
It's violating the Single Responsibility Principle by including transaction related operation which belongs to TransactionOperations class. So, we have create a new class TransactionOperations to handle transaction resposibility.

```java
    class AccountOperations {
        void addAccount() {

        }

        void updateAccount() {

        }
    }

    class TransactionOperations {
        void deposit(int amount) {

        }
    }
```
With this, we made each class to handle single resposibility.

## O - Open Closed Principle
A class or interface should be open for extension and closed for modification. Suppose, we have an existing functionality for which the code has be setup already. If you need to add any new enahancement to that functionality, we should check whether we are modifying the existing code or extending it and adding the enahancement related code. If modification is done, we are violating the principle.

Example:
```java
public class ReportProcessor {

  public void printReport(String reportType, String data) {
    if (reportType.equals("Sales")) {
      System.out.println("Sales Report:");
      // Logic for processing and printing sales data
    } else if (reportType.equals("Inventory")) {
      System.out.println("Inventory Report:");
      // Logic for processing and printing inventory data
    } else {
      System.out.println("Unsupported report type");
    }
  }
}
```

By looking at above code, if we have to add code for new report type, we have to add a case in above class method which is violating the principle. The design which adhers to this principle looks like below.

```java
    interface ReportOperation {
        void print(String data);
    }

    class SalesReport implements ReportOperation {

        @Override
        public void print(String data) {
            // Some Sales Report specific code
            System.out.println(data);
        }
    }

    class InventoryReport implements ReportOperation {

        @Override
        public void print(String data) {
            // Some Inventory Report specific code
            System.out.println(data);
        }
    }

    public class ReportProcessor {
        public void printReport(ReportOperation report, String data) {
            report.print(data);
        }
    }
```

With above design, we just need to crete a new Report Type class by implementing Operation interface when needed.

## L - Liskov Substitution Principle
It states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. In other words, if a class B is a subtype of class A, then we should be able to use an object of type B wherever we use an object of type A.

Example:
```java
    class Vehicle {
        void startEngine() {
            System.out.println("Engine started!");
        }
    }

    class Car extends Vehicle {
        void honkHorn() {
            System.out.println("Car horn honking!");
        }
    }

    class Bus extends Vehicle {
        // No implementation of honkHorn() here
    }

    public class Main {
        public static void main(String[] args) {
            Vehicle vehicle1 = new Car();
            Vehicle vehicle2 = new Bus();

            vehicle1.startEngine(); // Output: "Engine started!"
            vehicle1.honkHorn();   // Output: "Car horn honking!"

            // The following line would cause a runtime error:
            // vehicle2.honkHorn();
        }
    }
```
In this example, the Car class correctly implements the honkHorn() method, but the Bus class does not. When we try to call honkHorn() on a Bus object, it would result in a runtime error because the method is not supported by the Bus class. This violates LSP because the behavior of the derived class (Bus) is not substitutable for the behavior of the base class (Vehicle).

Remember, adhering to LSP ensures that derived classes can be used interchangeably with base classes without altering program correctness. In this case, the Bus class fails to meet that requirement.

Example:
```java
    class Vehicle {
        void startEngine() {
            System.out.println("Engine started!");
        }

        void honkHorn() {
            System.out.println("Bus horn honking!");
        }
    }

    class Car extends Vehicle {
        @Override
        public void honkHorn() {
            System.out.println("Car horn honking!");
        }
    }

    class Bus extends Vehicle {
        public void honkHorn() {
            System.out.println("Bus horn honking!");
        }
    }

    public class Main {
        public static void main(String[] args) {
            Vehicle vehicle1 = new Car();
            Vehicle vehicle2 = new Bus();

            vehicle1.startEngine(); // Output: "Engine started!"
            vehicle1.honkHorn();   // Output: "Car horn honking!"
            vehicle2.honkHorn();   // Output: "Bus horn honking!"
        }
    }
```

## I - Interface Seggregation Principle
We should create interface in such a way it should consists of proper methods which needs to be implemented by all classes which implements it. It should not be the case where it consists of methods which all clients might not require all of them. For example, client A needs all of them and client B needs only half of them. It violates the principle. We should seggregate the interface in such a way that all the methods that needs to be implemented by client should be relavent to that client. ISP focuses on keeping interfaces small and specific.

Example:

```java
    interface DaoInterface {
        void createRecord();
        void updateRecord();
        void openDbConnection();
        void openFile();
    }

    class DaoOperations implements DaoInterface {

        @Override
        public void createRecord() {

        }

        @Override
        public void updateRecord() {
            
        }

        @Override
        public void openDbConnection() {
            
        }

        @Override
        public void openFile() {
            
        }
    }
```
In above, we want to open a db connection and not file. But still DaoOperations must implement openFile() method which is unnecessary. To fix this, seggregate the DaoInterface into DbInterface and FileInterface by declaring connection related methods alone. Retain operations related methods in DaoInterface itself. After that, implement DaoInterface &DbInterface in DaoOperations class which eliminates the implementation of openFile() method. This makes code look cleaner, flexible and promotes loose coupling as well.

```java
    interface DaoInterface {
        void createRecord();
        void updateRecord();
    }

    interface DbInterface {
        void openDbConnection();
    }

    interface FileInterface {
        void openFile();
    }

    class DaoOperations implements DaoInterface, DbInterface {

        @Override
        public void createRecord() {

        }

        @Override
        public void updateRecord() {
            
        }

        @Override
        public void openDbConnection() {
            
        }
    }
```
## D - Dependency Inversion Principle
It states that high level modules should not depend on any specific implementations of low level module. Instead both high level & low level modules should depend on abstractions and interfaces. This promotes loose coupling. With this, if you need to change the implementations of low level module, you don't need to change the high level module that depends on it.

Example:

Tightly coupled code
```java
    class AddOperation {
        int add(int n1, int n2) {
            return n1 + n2;
        }
    }

    class SubtractOperation {
        int subtract(int n1, int n2) {
            return n1 - n2;
        }
    }

    class CalculateOperation {
        int calculate(int n1, int n2, String type) {
            switch(type) {
                case "add":
                    AddOperation operation = new AddOperation();
                    return operation.add(n1, n2);
                case "subtract":
                    SubtractOperation operation = new SubtractOperation();
                    return operation.subtract(n1, n2);
                default:
                    return -1;
            }
        }
    }
```
High level module (CalculateOperation) is dependent on the specific implementations (AddOperation, CalculateOperation) of low level module.

Loosely Couple Code
```java
    interface Operation {
        int calculate(int n1, int n2);
    }

    class AddOperation implements Operation {
        
        @Override
        public int calculate(int n1, int n2) {
            return n1 + n2;
        }
    }

    class SubtractOperation implements Operation {
        
        @Override
        public int calculate(int n1, int n2) {
            return n1 - n2;
        }
    }

    class CalculateOperation {
        int calculate(int n1, int n2, Operation op) {
            return op.calculate(n1, n2);
        }
    }
```
Above both modules are made to depend on the interface.