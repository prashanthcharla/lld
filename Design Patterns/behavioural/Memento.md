Behavioural pattern which is used to capture & store the state of an object so that it can be restored later without exposing the internal details of the object. It's commonly used for undo (or rollbak) functionality.

#### Key Points:
* Problem:
    * You want to implement undo functionality or restore an object’s previous state, but you don’t want to expose its internal state or make your code tightly coupled.
    * First Approach
        ```java
        class TextEditor {
            public String text = "";  // Exposed state

            public void write(String newText) {
                text += newText;
            }
        }
        ```

        ```java
        class HistoryManager {
            private String previousState = "";

            public void save(TextEditor editor) {
                previousState = editor.text;  // Direct access (bad practice)
            }

            public void undo(TextEditor editor) {
                editor.text = previousState;  // Direct manipulation (violates encapsulation)
            }
        }
        ```
    * Second Approach
        ```java
        class TextEditor {
            private String text = ""; 
            private String previousText = "";

            public void write(String newText) {
                previousText = new String(text);
                text += newText;
            }

            public String getText() {
                return text;
            }

            public void restore() {
                text = previousText;
            }
        }
        ```
* Problem Analysis:
    * First approach exposes the state to other classes, violating encapsulation.
    * Second approach violates single responsibility principle by handling both the text editing & restoring functionlities.
    * Both the approaches limits the number of rollbacks to only one.
* Solution:
    * Use Memento pattern.
        * It has 3 components mainly.
            * Originator: Class whose state need to be restored. Eg: A text document that knows its content.
            * Memento: Class which acts as a snapshot of the Originator’s state. Objects of this class are created by Originator.
            * Caretaker: Stores and retrieves Mementos, without knowing what's inside. Eg: A folder that stores snapshots but doesn’t open or edit them.

#### Example:
Originator
```java
public class TextDocument {
    private String text;
    // other props. Possible to choose only some out of all properties (state) for restoration.

    public void saveText(String text) {
        this.text = text;
    }

    public String getText() {
        return this.text;
    }

    public TextDocumentSnapshot getSnapshot() {
        return new TextDocumentSnapshot(this.text);
    }
}
```
Memento
```java
public class TextDocumentSnapshot {
    private String text;

    public TextDocumentSnapshot(String text) {
        this.text = text;
    }

    public String getText() {
        return this.text;
    }
}
```
Caretaker
```java
public class TextDocumentSnapshotStorage {
    private List<TextDocumentSnapshot> history = new ArrayList<>();

    public void add(TextDocumentSnapshot textDocumentSnapshot) {
        history.add(textDocumentSnapshot);
    }

    public TextDocumentSnapshot restore() {
        return history.isEmpty() ? null : history.removeLast();
    }
}
```
Usage
```java
public class Main {
    public static void main(String[] args) {
        TextDocumentSnapshotStorage textDocumentSnapshotStorage = new TextDocumentSnapshotStorage();

        TextDocument textDocument = new TextDocument();
        textDocument.saveText("Hello India");
        textDocumentSnapshotStorage.add(textDocument.getSnapshot()); // Edited & saved

        textDocument.saveText("Hello US");
        textDocumentSnapshotStorage.add(textDocument.getSnapshot()); // Edited & saved

        textDocument.saveText("Hello Canada"); // Edited but not yet saved

        System.out.println("Current: " + textDocument.getText());

        TextDocumentSnapshot previousVersion = textDocumentSnapshotStorage.restore();
        while (previousVersion != null) {
            System.out.println("Previous: " + previousVersion.getText());
            previousVersion = textDocumentSnapshotStorage.restore();
        }
    }
}
```

#### Real-time example:
* Git — Version Control
    * Each Commit = Memento of project state
    * You can checkout/revert back to that state
* Browser — History / Back Button
    * Each visited page is stored as a Memento in browser history.
    * You can navigate back (restore previous state).
* IDE / Text Editor — Undo Functionality
    * You type text in a document.
    * Each change creates a Memento (snapshot of text state).
    * You can Undo to revert back to a previous state.