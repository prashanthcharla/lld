Behavioural pattern which lets you wrap a request in an objects so that you can pass it around, store it & execute it later. This is useful when you want to separate the code that calls an action from the code that does an action. It helps with
* Undo/redo
* Command history
* Flexible control over actions. You execute commands later or in a controlled way.

#### Key Points:
* Problem:
    * You can perform below operations on a file using dedicated buttons in the file system UI.
        * Open
        * Save
    * The button actions directly call:
        ```java
        click on open → file.open()
        click on save → file.save()
        ```
      But with this setup, Can you undo, redo, log, or delay actions?
* Problem Analysis:
    * The UI buttons are tightly coupled with the file actions.
    * If a new action is introduced, the UI (client) must be changed — violating the Open/Closed Principle.
    * Features like undo/redo, logging, delaying would have to be coded directly into the UI, which is bad design.
* Solution:
    * Wrap each file action (open, save) inside a Command object.
    * Each Command holds a reference to the receiver (File) and knows how to call its method (present in receiver).
    * These commands are passed to an Invoker, which has an execute() method.
    * Flow
        1. Client (UI) triggers a request.
        2. The request is wrapped in a Command object.
        3. The Invoker receives the command and executes it.
        4. The Command calls the actual method on the File (Receiver).
    * Advantages
        * You can now log, undo/redo, queue, or delay actions inside the Invoker.
        * Client/UI code stays unchanged when new commands are added.
        * Follows Single Responsibility and Open/Closed principles.

#### Example:
Common interface for Command:
```java
public interface TextFileOperation {
    void execute();
}
```
Receiver:
```java
public class TextFile {
    private String name;

    public TextFile(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void open() {
        System.out.println("Opening file " + name);
    }

    public void save() {
        System.out.println("Saving file " + name);
    }
}
```
Commands:
```java
public class OpenTextFileOperation implements TextFileOperation {
    private TextFile textFile;

    public OpenTextFileOperation(TextFile textFile) {
        this.textFile = textFile;
    }

    @Override
    public void execute() {
        textFile.open();
    }
}
```

```java
public class SaveTextTextFileOperation implements TextFileOperation {
    private TextFile textFile;

    public SaveTextTextFileOperation(TextFile textFile) {
        this.textFile = textFile;
    }

    @Override
    public void execute() {
        textFile.save();
    }
}
```
Invoker:
```java
public class TextFileCommandExecutor {
    private static List<TextFileOperation> textFileOperations = new ArrayList<>();

    public static void run(TextFileOperation fileOperation) {
        textFileOperations.add(fileOperation);
        fileOperation.execute();
    }
}
```
Client:
```java
public class Main {
    public static void main(String[] args) {
        TextFileCommandExecutor.run(new OpenTextFileOperation(new TextFile("hi.txt")));
        TextFileCommandExecutor.run(new SaveTextTextFileOperation(new TextFile("hi.txt")));
    }
}
```

#### Real-time example:
* Spring Boot - @Scheduled + Runnable/Command
    * You want scheduled tasks (commands) to execute at fixed intervals
        ```java
        @Component
        public class DataBackupCommand {

            @Scheduled(cron = "0 0 * * * ?")
            public void execute() {
                System.out.println("Running data backup...");
            }
        }
        ```
        * Command: execute() method with backup logic
        * Invoker: Spring Scheduler triggers it
* Undo/Redo in GUI Apps
    * Each action (like typing, deleting) is a Command object
    * You can store them in a stack and replay/undo/redo

