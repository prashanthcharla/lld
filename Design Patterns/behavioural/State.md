Behavioural patten allows the object to change its behaviour when its internal state changes. It helps manage complex conditional logic based on the object's state by moving state-specific behavior into separate classes.

#### Key points:
* Problem:
    * You have an object whose behavior varies depending on its current state.
    * We can use if-else statements to handle state transitions like below.
        ```java
        public class Fan {
            private String state = "off";

            public void pressButton(String state) {
                if (state.equals("low")) {
                    System.out.println("Fan is running at low speed");
                    this.state = state;
                } else if (state.equals("high")) {
                    System.out.println("Fan is running at high speed");
                    this.state = state;
                } else {
                    System.out.println("Fan is turned off");
                    this.state = state;
                }
            }
        }
        ```
* Problem Analysis:
    * Its violating the open/closed principle as we need to modify the existing code upon addition/removal of a state.
    * Hard to extend and maintain.
* Solution:
    * Use State pattern.
        * Create State interface with behaviour methods.
        * Implement concrete State classes for each state with specific behaviour.
        * The main object (called Context) holds a reference to the current state and delegates the behavior to the state object.
        * State transition in the Context happen by changing the state object.
        * Eliminates complex if-else or switch logic.
        * Each state has its own class with clear responsibilities.

#### Example:
```java
public interface State {
    String getLabel();
    void doAction(Fan fan);
}
```
Context
```java
public class Fan {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public State getState() {
        return state;
    }
}
```
Concrete State Classes
```java
public class LowState implements State {
    @Override
    public String getLabel() {
        return "low";
    }

    @Override
    public void doAction(Fan fan) {
        System.out.println("Fan is running at low speed");
        fan.setState(this);
    }
}
```

```java
public class HighState implements State {
    @Override
    public String getLabel() {
        return "high";
    }

    @Override
    public void doAction(Fan fan) {
        System.out.println("Fan is running at high speed");
        fan.setState(this);
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        Fan fan = new Fan();

        State lowState = new LowState();
        lowState.doAction(fan);

        State highState = new HighState();
        highState.doAction(fan);

        System.out.println("Current state of fan: " + fan.getState().getLabel());
    }
}
```

#### Real-time example:
* Workflow Engine (e.g., Order Processing System)
    * States: New → Processing → Shipped → Delivered → Cancelled
    * Each state controls allowed transitions
* Media Player — Play/Pause/Stop States
    * States: Playing, Paused, Stopped
    * Each action depends on the current state