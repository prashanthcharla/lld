Structural design pattern that converts the interface of a class into another interface that a client expects, allowing incompatible classes to work together.

#### Key Components:
* Target: Interface client expects (e.g., NewPrinter).
* Adaptee: Existing incompatible class (e.g., OldPrinter).
* Adapter: Bridges target and adaptee (e.g., PrinterAdapter).

#### Types:
* Class Adapter: Uses inheritance (extends adaptee, implements target).
* Object Adapter: Uses composition (holds adaptee instance, implements target).

#### Key Points:
* Integrates legacy systems or third-party libraries with new code (e.g., adapting old APIs to modern interfaces).
* Promotes reusability, decouples client from adaptee, and supports legacy code integration.
* Use when interfaces don’t match, or you need to reuse existing classes without modifying them.

#### Example:

```java
public interface NewPrinter {
    void printInNewFormat(String message);
}
```

```java
public class NewPrinterItem implements NewPrinter {
    @Override
    public void printInNewFormat(String message) {
        System.out.println("New Printer: "+message);
    }
}
```

```java
public interface OldPrinter {
    void printInOldFormat(String message);
}
```

```java
public class OldPrinterItem implements OldPrinter{
    @Override
    public void printInOldFormat(String message) {
        System.out.println("Old Printer: "+message);
    }
}
```
Class Type
```java
public class ClassTypePrinterAdapter extends OldPrinterItem implements NewPrinter {

    @Override
    public void printInNewFormat(String message) {
        printInOldFormat(message);
    }
}
```
Object Type
```java
public class ObjectTypePrinterAdapter implements NewPrinter {
    private final OldPrinter oldPrinter;

    public ObjectTypePrinterAdapter(OldPrinter oldPrinter) {
        this.oldPrinter = oldPrinter;
    }

    @Override
    public void printInNewFormat(String message) {
        oldPrinter.printInOldFormat(message);
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
        // Test Object Type Adapter pattern
        NewPrinter newPrinter = new ObjectTypePrinterAdapter(new OldPrinterItem());
        newPrinter.printInNewFormat("Hello, world");

        // Test Class Type Adapter pattern
        NewPrinter newPrinter1 = new ClassTypePrinterAdapter();
        newPrinter1.printInNewFormat("Hello, world");
    }
}
```

#### Real-time example:
* Spring Boot
    * HttpMessageConverter (Adapter for Request/Response Bodies)
        * Scenario: Your controller returns a Java Object:
            ```java
            @RestController
            public class UserController {
                @GetMapping("/user")
                public User getUser() {
                    return new User("John");
                }
            }
            ```

            But the client expects JSON.
    * Spring uses HttpMessageConverter to adapt your Java object to JSON.
* Angular
    * HttpInterceptor (Adapting HTTP Requests)
    * Scenario: You want to add a token to every HTTP request without changing each request manually.

        You write an Adapter (Interceptor):
        ```ts
        @Injectable()
        export class AuthInterceptor implements HttpInterceptor {
            intercept(req: HttpRequest<any>, next: HttpHandler) {
                const modifiedReq = req.clone({
                headers: req.headers.set('Authorization', 'Bearer token')
                });
                return next.handle(modifiedReq);
            }
        }
        ```
    * Here
        * Adaptee: Original HTTP Request
        * Target: Modified HTTP Request (with token)
        * Adapter: HttpInterceptor