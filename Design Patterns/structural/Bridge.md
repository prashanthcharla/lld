Structural design pattern that separates abstraction (what it does) from its implementation so that both can change independantly.

#### Key Points:
* Problem
    * There are two axes of change:
        * Shape hierarchy: Shape → Circle, Shape → Square, etc.
        * Color hierarchy: Red, Blue, etc.
    * If we tightly combine both, we end up with an explosion of classes like:
        * RedCircle
        * BlueCircle
        * RedSquare
        * BlueSquare...and it keeps growing as you add more shapes or more colors.
* Problem Analysis:
    * If we observe carefully, there are two relationships being forced:
        * Shape "is a" Circle. ✅ (Correct — Circle is a Shape)
        * Circle "is a" RedCircle. ❌ (Incorrect — Color isn't a type of Shape)
    * This is misleading, because:
        * The shape concept is completely different from color.
        * But in the wrong design (Problem), we're bundling color inside the shape type.
* Bridge Pattern — The Right Solution:
    * The irrelevant part (Color) must be provided through a bridge, not baked into the shape hierarchy.
    * So instead of saying: `Circle is a RedCircle` We say: `Circle has a Color`.
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