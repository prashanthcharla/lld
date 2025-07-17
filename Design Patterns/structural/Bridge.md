Structural design pattern that separates abstraction (what it does) from its implementation so that both can change independantly.

#### Key Points:
* Problem
    * You need to apply color to a shape.
    * There are two axes of change:
        * Shape hierarchy: Shape → Circle, Shape → Square, etc. (one to many)
        * Color hierarchy: Red, Blue, etc. (one to many)
    * If we tightly combine both, we end up with an explosion of classes like:
        * RedCircle
        * BlueCircle
        * RedSquare
        * BlueSquare...and it keeps growing as you add more shapes or more colors.
* Problem Analysis:
    * This way of multiplication is bad, because this grows rapidly if you add new color types.
* Bridge Pattern — The Right Solution:
    * When you have two things that can change independently, you pick one as Abstraction (core functionality) and one as Implementation (additional feature/behavior). 
    * The Implementation should be handled using a separate interface (called Implementor).
    * The Abstraction will hold a reference to the Implementorß.
    * So instead of saying: `Circle is a RedCircle` We say: `Circle has a Red Color`.
    * This means:
        * Shape focuses only on the type of shape (Circle, Square, etc.).
        * Color is provided externally via a bridge (composition).

#### Example:
```java
public interface Color {
    String getColor();
}
```

```java
public class RedColor implements Color {
    @Override
    public String getColor() {
        return "red";
    }
}
```

```java
public class BlueColor implements Color {
    @Override
    public String getColor() {
        return "blue";
    }
}
```

```java
public abstract class Shape {
    protected Color color;

    Shape(Color color) {
        this.color = color;
    }

    public abstract void draw();
}
```

```java
public class Circle extends Shape {

    Circle(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.println("Circle with color: "+super.color.getColor());
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Shape redCircle = new Circle(new RedColor());
        redCircle.draw();
        Shape blueCircle = new Circle(new BlueColor());
        blueCircle.draw();
    }
}
```

#### Real-time example:
* Payment: Checkout, Subscription, Refund [Core Functionality]
  
  PaymentGateway: PayPal, Stripe, Razorpay [Additional Feature/Behaviour]

  You can build new types of Payments (e.g., EMI, One-Time)

  You can plug new Gateways anytime — they stay decouple
* Button: PrimaryButton, IconButton [Core Functionality]

  Theme: DarkTheme, LightTheme [Additional Feature/Behaviour]

  You design a button once and apply any theme dynamically