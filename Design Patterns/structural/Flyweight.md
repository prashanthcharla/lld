Structural design pattern which minimizes memory by sharing the objects which are similar in data. It’s ideal when you have lots of similar objects — instead of creating thousands of copies, you reuse the shared part (intrinsic state).

#### Two Key Concepts:
* Intrinsic State: Shared data that doesn't change between objects (e.g., role)
* Extrinsic State: Unique data that cannot be shared (e.g., name)

#### Key Points:
* Problem:
    * You're building a user management system where each user has a role (e.g., "Admin", "Editor", "Viewer"). There are millions of users, but only a few unique roles. If you store a separate Role object for each user, you will:
        * Waste memory (e.g., millions of "Admin" objects)
        * Repeat identical data over and over
* Problem Analysis:
    * Let’s say:
        * 500,000 users are Admins
        * 300,000 are Viewers
        * 200,000 are Editors
    * If each `User` has its own `Role` object:
        ```java
        class User {
            String name;
            Role role = new Role("Admin");
        }
        ```
    * You end up with 1,000,000 Role objects, even though only 3 distinct role types exist.
        * Roles have the same data, yet you’re duplicating them.
        * This is inefficient and violates DRY (Don't Repeat Yourself).
        * There is a clear intrinsic (shared) part: Role.name
        * And an extrinsic (unique) part: name
* Solution:
    * Use a shared pool of Role objects (Flyweights), and have users reference them instead of creating new ones.

#### Example:
```java
public class Role {
    String name;

    Role(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "Role{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

```java
public class User {
    private String name;
    private Role role;

    public User(String name, Role role) {
        this.name = name;
        this.role = role;
    }

    public String getName() {
        return name;
    }

    public Role getRole() {
        return role;
    }
}
```
To store & share the intrinsic part of an object:
```java
public class RoleFactory {

    private static Map<String, Role> roles = new HashMap<>();

    private RoleFactory() {}

    public static Role getRole(String name) {
        roles.putIfAbsent(name, new Role(name));
        System.out.println("Roles: " + roles);
        return roles.get(name);
    }
}
```
Usage:
```java
public class Main {
    public static void main(String[] args) {
        User u1 = new User("Ram", RoleFactory.getRole("Admin"));
        User u2 = new User("Hari", RoleFactory.getRole("Admin"));
        User u3 = new User("Navin", RoleFactory.getRole("Normal"));
        User u4 = new User("Praveen", RoleFactory.getRole("Admin"));
    }
}
```