Behavioural pattern that provides a way to access the elements of a collection in sequential manner without exposing its underlying representation (data structure, number of elements). 

#### Key Points:
* Problem:
    * You have a collection of objects (like a list of books), and you want to access each element one by one
        * without knowing how the collection stores data (data structure)
        * without duplicating iteration logic across client code (actual iteration code based on data structure)
        * while keeping the collection’s internal structure hidden (type of data structure)
* Probelm Analysis:
    * Different collections use different internal structures (arrays, lists, hash sets, trees).
    * Duplicating the iteration logic in client code every time when it iterates is bad for maintainability.
* Solution:
    * Use Iterator pattern.
        * It provides an Iterator object which has simple methods like hasNext (to check if next element exists) & next (to get the next element).
        * Collection will create the new iterator thought its iterator method.
        * The iterator knows how to iterate the elements, client doesn't have to.
        * Even if the collection’s internal storage changes, the client code remains unchanged as long as the iterator works.
    * It decouples the iteration logic from clients code so that both can evolve independantly.

#### Example:
```java
public class Book {
    private String name;

    public Book(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

```java
public class BookIterator {
    private List<Book> books;
    private int currentIndex;

    public BookIterator(List<Book> books) {
        this.books = books;
        this.currentIndex = 0;
    }

    public boolean hasNext() {
        return currentIndex < books.size();
    }

    public Book next() {
        return books.get(currentIndex++);
    }
}
```
Below `BookCollection` exposes iterator through a method. If needed, we have muliple iterators with different logic for book collection and pick as necessary.
```java
public class BookCollecton {
    private List<Book> books;

    public BookCollecton() {
        this.books = new ArrayList<>();
    }

    public void add(Book book) {
        books.add(book);
    }

    public BookIterator iterator() {
        return new BookIterator(books);
    }
}
```
Client
```java
public class Main {
    public static void main(String[] args) {
        BookCollecton bookCollecton = new BookCollecton();
        bookCollecton.add(new Book("Rain"));
        bookCollecton.add(new Book("Jungle"));

        BookIterator iterator = bookCollecton.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next().getName());
        }
    }
}
```

#### Real-time example:
* Java Collections - Iterator Interface
    ```java
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

    Iterator<String> iterator = names.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
    ```
    * List provides an iterator
    * You can iterate without knowing how the list stores its elements (array, linked list, etc.)
* Angular — *ngFor Directive
    ```html
    <ul>
    <li *ngFor="let item of items">{{ item }}</li>
    </ul>
    ```
    * Angular uses Iterable interface to loop over collections
    * It abstracts iteration logic from the template