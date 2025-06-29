This pattern helps to build object step by step. It allows flexible creation of objects with many optional property settings, avoiding long constructor calls.

#### Key Points:
* It uses method chaining for clear construction (e.g., setFirstName().setLastName())
* Eliminates need for multiple constructors with varying parameters.
* Ideal for objects with many optional fields (e.g., custom orders, configurations like computers or meals).
* Returns a new instance on calling build() method.
* Can create immutable objects by returning a new instance only though build().

#### Example:

```java
public class Meal {
    private final String mainDish;
    private final String sideDish;
    private final String drink;
    private final String dessert;
    private final String soup;

    // Private constructor to enforce builder usage
    private Meal(MealBuilder mealBuilder) {
        this.mainDish = mealBuilder.mainDish;
        this.sideDish = mealBuilder.sideDish;
        this.drink = mealBuilder.drink;
        this.dessert = mealBuilder.dessert;
        this.soup = mealBuilder.soup;
    }

    // Static because MealBuilder instance creation is independent of Meal instance
    public static class MealBuilder {
        private String mainDish;
        private String sideDish;
        private String drink;
        private String dessert;
        private String soup;

        public MealBuilder() {}

        public MealBuilder mainDish(String mainDish) {
            this.mainDish = mainDish;
            return this;
        }

        public MealBuilder sideDish(String sideDish) {
            this.sideDish = sideDish;
            return this;
        }

        public MealBuilder drink(String drink) {
            this.drink = drink;
            return this;
        }

        public MealBuilder dessert(String dessert) {
            this.dessert = dessert;
            return this;
        }

        public MealBuilder soup(String soup) {
            this.soup = soup;
            return this;
        }

        public Meal build() {
            // Add validations if needed
            return new Meal(this);
        }
    }

    public static MealBuilder builder() {
        return new MealBuilder();
    }

    public String getMainDish() {
        return mainDish;
    }

    public String getSideDish() {
        return sideDish;
    }

    public String getDrink() {
        return drink;
    }

    public String getDessert() {
        return dessert;
    }

    public String getSoup() {
        return soup;
    }

    @Override
    public String toString() {
        return "Meal{" +
                "mainDish='" + mainDish + '\'' +
                ", sideDish='" + sideDish + '\'' +
                ", drink='" + drink + '\'' +
                ", dessert='" + dessert + '\'' +
                ", soup='" + soup + '\'' +
                '}';
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
        Meal meal = Meal.builder().mainDish("Rice").sideDish("Pastha").build();
        System.out.println(meal.toString());
    }
}
```