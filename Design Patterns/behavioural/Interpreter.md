Behavioural pattern which provides a way to interpret expressions (can be anything which produces a value like number, addition, word etc). It defines a representation for expressions along with interpreter that uses representation to interpret expressions. It's useful when you need to evaluate expressions, commands or simple scripting languages, especially when grammer is simple & fixed.

#### Key Points:
* This pattern categories expressions into two types.
    * Terminal: Represents the basic elements that cannot be broken down further. For example, numbers.
    * Non-Terminal: Represents combinations of other expressions. For example, addition.
* Problem:
    * You want to interpret or evaluate expressions (like mathematical operations, commands, or grammar). For example, parsing simple math expressions like "2 + 3" or checking if a given string matches a specific pattern.
    * Can be achieved using if/else.
        ```java
        class ExpressionEvaluator {
            int evaluate(String expression) {
                if (expression.equals("2 + 3")) {
                    return 5;
                } else if (expression.equals("5 - 2")) {
                    return 3;
                }
                return 0;
            }
        }
        ```
* Problem Analysis:
    * Hardcoding expression evaluation logic makes the code rigid and non-extensible.
    * Adding new expressions or rules requires significant changes.
* Solution:
    * Use Interpreter pattern.
        * Define a classes where each class represents a expression.
        * Expression class implements an interpret() method to evaluate or interpret that expression.
        * Build complex expressions by composing simple expressions.
        * These expressions (terminal or non-terminal) act as way to interpret (using interpret method) the real expressions (stored in terminal expressions).

#### Example:
```java
public interface Expression {
    int interpret();
}
```
Terminal Expression
```java
public class NumberExpression implements Expression {
    private int no;

    public NumberExpression(int no) {
        this.no = no;
    }

    @Override
    public int interpret() {
        return no;
    }
}
```
Non-Terminal Expression
```java
public class NumberAdditionExpression implements Expression {
    private Expression expression1;
    private Expression expression2;

    public NumberAdditionExpression(Expression expression1, Expression expression2) {
        this.expression1 = expression1;
        this.expression2 = expression2;
    }

    @Override
    public int interpret() {
        return expression1.interpret() + expression2.interpret();
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        Expression add = new NumberAdditionExpression(
                new NumberExpression(2),
                new NumberExpression(3)
        );

        System.out.println(add.interpret());
    }
}
```
