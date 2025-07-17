Its a structural design pattern that lets you treat individual objects and groups of objects in the same way. Used when you want to build a tree-like structure (e.g., menus, files, organization hierarchy). For example, a folder (component) can contain files (leaves) or other folders (composites), but both respond to the same actions like open(), rename(), etc. 

#### Key Points:
* Problem:
    * You are building systems where entities can be either:
        * Single objects (e.g., file, employee, button)
        * Or groups of objects (e.g., folder, team, panel)
    * The problem arises when:
        * The client has to handle single objects differently from groups.
        * Code becomes cluttered with conditions like:
          ```java
            if (object is folder) {
                for (each file in folder) do something
            } else {
                do something with the file
            }
          ```
    * This violates clean coding principles and makes the system hard to extend, maintain, and scale.
* Problem Analysis:
    * The root cause is:
        * No unified way to treat individual objects and composite (group) objects.
    * Client code must check:
        * Is it a file or folder?
        * Is it an employee or a manager?
    * Every time a new type is introduced, client code must be updated with more conditions.
    * This breaks:
        * Open-Closed Principle — client code isn't closed for modification.
        * Single Responsibility Principle — client handles both logic and structure checks.
* Solution — Composite Design Pattern:
    * Introduce a common interface or abstract class (e.g., Component or Employee).
    * Both:
        * Leaf objects (e.g., File, Developer) and
        * Composite objects (e.g., Folder, Manager)
    
      implement this common interface.
    * The Composite class:
        * Holds a collection of components (both Leafs and other Composites).
        * Delegates tasks like showDetails() or operation() to its children.
    * The Client doesn’t care whether it's dealing with a single object or a group — the interaction is the same.

#### Example:
Unified Interface
```java
public abstract class Employee {
    private String name;
    private String designation;

    public Employee(String name, String designation) {
        this.name = name;
        this.designation = designation;
    }

    public String getName() {
        return name;
    }

    public String getDesignation() {
        return designation;
    }
    
    public abstract void showDetails();

    public void add(Employee employee) {
        throw new UnsupportedOperationException("Add employee operation not supported");
    }

    public void remove(Employee employee) {
        throw new UnsupportedOperationException("Remove employee operation not supported");
    }
}
```
Leaf
```java
public class Developer implements Employee {
    
    public Developer(String name, String designation) {
        super(name, designation);
    }

    @Override
    public void showDetails() {
        System.out.println("Name: " + this.name + " Designation: " + this.designation);
    }
}
```
Composite
```java
public class Manager implements Employee {
    private List<Employee> employees = new ArrayList<>();

    public Manager(String name, String designation) {
        super(name, designation);
    }

    @Override
    public void showDetails() {
        System.out.println("Name: " + this.name + " Designation: " + this.designation);
        if(!employees.isEmpty()) {
            System.out.println("Employees under " + this.name);
            for (Employee employee: employees) {
                employee.showDetails();
            }
            System.out.println();
        }
    }

    @Override
    public void add(Employee employee) {
        this.employees.add(employee);
    }

    @Override
    public void remove(Employee employee) {
        this.employees.remove(employee);
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        Employee dev1 = new Developer("Bob", "Senior Software Engineer");
        Employee dev2 = new Developer("John", "Architect");

        Employee dev3 = new Developer("Madhan", "Architect");

        Employee mgr1 = new Manager("Ram", "Development Manager");
        mgr1.add(dev1);
        mgr1.add(dev2);

        mgr1.showDetails();
        dev3.showDetails();
    }
}
```

#### Real-time example:
* Spring Boot - Composite Validator using Validator Interface
    * You can create:
        * Single Validator (leaf)
        * Composite Validator that holds multiple validators (composite)
            ```java
            public class CompositeValidator implements Validator {
                private List<Validator> validators;

                public CompositeValidator(List<Validator> validators) {
                    this.validators = validators;
                }

                @Override
                public void validate(Object target, Errors errors) {
                    validators.forEach(validator -> validator.validate(target, errors));
                }

                @Override
                public boolean supports(Class<?> clazz) {
                    return true;
                }
            }
            ```
            * You can treat single validators and composite validator the same way — by calling validate()
            * Composite contains and delegates to other validators
* Angular - Form Groups & Form Controls
    * In Angular Reactive Forms:
        * FormControl = A single field (e.g., input box)
        * FormGroup = A group of fields (a form section)
            ```ts
            const profileForm = new FormGroup({
                firstName: new FormControl(''),
                address: new FormGroup({
                    street: new FormControl(''),
                    city: new FormControl('')
                })
            });
            ```
            * You can treat a FormControl and a FormGroup the same way (setValue(), reset())
            * Both implement the AbstractControl class