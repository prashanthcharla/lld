Behavioural pattern which helps in enforcing common workflow (sequence of steps) while letting subclasses tweak the specifc steps without affecting the overall process. Subclasses can override specific behavior methods to customize parts (some steps) of the workflow.

#### Key Points:
* Problem:
    * You have multiple classes that follow the same overall workflow but differ in some specific steps.
    * For example,
        ```java
        public class TeaMaker {
            public void make() {
                boilWater();
                brew();
                pourInCup();
                addCondiments();
            }

            public void boilWater() {
                System.out.println("Boiling water");
            }

            public void brew() {
                System.out.println("Steeping tea leaves");
            }

            public void pourInCup() {
                System.out.println("Pour into cup");
            }

            public void addCondiments() {
                System.out.println("Adding lemon");
            }
        }
        ```
        ```java
        public class CoffeeMaker {
            public void make() {
                boilWater();
                brew();
                pourInCup();
                addCondiments();
            }

            public void boilWater() {
                System.out.println("Boiling water");
            }

            public void brew() {
                System.out.println("Brew coffee grounds");
            }

            public void pourInCup() {
                System.out.println("Pour into cup");
            }

            public void addCondiments() {
                System.out.println("Add sugar and milk");
            }
        }
        ```
* Problem Analysis:
    * `make()` defines the workflow in both the classes.
    * Common workflow logic is repeated in each class leading to duplication.
    * Each of above class implements its own full version (methods apart from `make()`) of algorithm, even though only some steps differ, again leading to duplication.
* Solution:
    * Use Template pattern.
        * Put the common workflow logic in base class. The method which contains the workflow logic is called template method.
        * Define abstract methods for the steps that vary.
        * Subclasses implement the abstract methods. Template method controls the workflow.
        * This pattern works best when:
            * A class defines a workflow or sequence of operations.
            * Some steps in the workflow are common for all subclasses.
            * Some steps are customizable or vary between subclasses.

#### Example:
```java
public abstract class Template {
    // Template Method
    public void make() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    // Common Step in the workflow
    public void boilWater() {
        System.out.println("Boiling water");
    }

    // Common Step in the workflow
    public void pourInCup() {
        System.out.println("Pour into cup");
    }

    public abstract void brew();
    public abstract void addCondiments();
}
```

```java
public class TeaMaker extends Template {
    @Override
    public void brew() {
        System.out.println("Steeping tea leaves");
    }

    @Override
    public void addCondiments() {
        System.out.println("Adding lemon");
    }
}
```

```java
public class CoffeeMaker extends Template {
    @Override
    public void brew() {
        System.out.println("Brew coffee grounds");
    }

    @Override
    public void addCondiments() {
        System.out.println("Add sugar and milk");
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        Template teaMaker = new TeaMaker();
        teaMaker.make();

        Template coffeeMaker = new CoffeeMaker();
        coffeeMaker.make();
    }
}
```

#### Real-time example:
* Spring Boot – JdbcTemplate
    ```java
    List<User> users = jdbcTemplate.query(
        "SELECT * FROM users",
        (rs, rowNum) -> new User(rs.getLong("id"), rs.getString("name"))
    );
    ```
    * You want to query the database. The general steps withing `query` method are always the same:
        * Open connection
        * Execute query
        * Map result
        * Close connection

    Spring provides JdbcTemplate, which uses the Template Pattern to simplify this. You just injects query & custom behavior (mapping) without changing the structure. So, `query` method above is a template method.