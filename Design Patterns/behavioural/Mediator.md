Behavioural pattern which simplifies the communication between the multiple objects by introducing central mediator which manages the interactions. Use this pattern when muliple objects interact (many-to-many) in a tightly coupled way. 

#### Key Points:
* Problem: 
    * In a system where multiple objects (components) interact directly with each other,
        * The communication becomes complex and tightly coupled.
        * Changes in one component may require changes in others.
        * It's hard to maintain or extend.
    * Example:
        ```java
        public class User {
            private String name;
            private List<User> contacts;

            public User(String name) {
                this.name = name;
                this.contacts = new ArrayList<>();
            }

            public void addContact(User user) {
                contacts.add(user);
            }

            public void sendMessage(String message) {
                for (User user: contacts) {
                    user.receiveMessage(message, this);
                }
            }

            public void receiveMessage(String message, User sender) {
                System.out.println("User: " + this.name + " , Message: " + message + " , Sender: " + sender.name);
            }
        }
        ```

        ```java
        public class ChatRoom {
            public static void main(String[] args) {
                User u1 = new User("Ram");
                User u2 = new User("Hari");
                User u3 = new User("Kamal");

                // Every user must add others as dependency resulting in tight coupling 
                u1.addContact(u2);
                u1.addContact(u3);

                u2.addContact(u1);
                u2.addContact(u3);

                u3.addContact(u1);
                u3.addContact(u2);

                u1.sendMessage("Hi");
            }
        }
        ```
* Prolem Analysis:
    * Direct interaction between components leads to many-to-many dependency.
    * In above chat app, if number of users are in hundreds or thousands — it gets messy.
    * We need a central authority to manage communication.    
* Solution:
    * Use Mediator design pattern. In this,
        * Components (objects) don't communicate directly.
        * Instead, they send messages/events to a Mediator.
        * Components must be registered in Mediator.
        * The Mediator handles the communication between components.
        * This reduces coupling and centralizes communication logic.

### Example:
```java
public class User {
    private String name;
    private ChatMediator chatMediator;

    public User(String name, ChatMediator mediator) {
        this.name = name;
        this.chatMediator = mediator;
    }

    public void sendMessage(String message) {
        this.chatMediator.sendMessage(message, this);
    }

    public void receiveMessage(String message, User sender) {
        System.out.println("User: " + this.name + " , Message: " + message + " , Sender: " + sender.name);
    }
}
```

```java
public class ChatMediator {
    private List<User> users = new ArrayList<>();

    public void registerUser(User user) {
        users.add(user);
    }

    public void sendMessage(String message, User sender) {
        for (User user: users) {
            user.receiveMessage(message, sender);
        }
    }
}
```

```java
public class ChatRoom {
    public static void main(String[] args) {
        ChatMediator chatMediator = new ChatMediator();

        User u1 = new User("Ram", chatMediator);
        User u2 = new User("Hari", chatMediator);
        User u3 = new User("Kamal", chatMediator);

        chatMediator.registerUser(u1);
        chatMediator.registerUser(u2);
        chatMediator.registerUser(u3);

        u3.sendMessage("Hi");
    }
}
```

#### Real-time example:
* Air Traffic Controller manages planes' communication. ATC is a Mediater here. Planes won't interact directly.
* UI Widgets Interaction
    * When a checkbox is checked, a button is enabled
    * Instead of Checkbox knowing about Button, both interact via Dialog Manager (Mediator)
        ```java
        class DialogManager {
            void notify(Component sender, String event) {
                if (sender == checkbox && event.equals("checked")) {
                    button.enable();
                }
            }
        }
        ```