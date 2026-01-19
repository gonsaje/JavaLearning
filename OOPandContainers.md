# OOP + Containers (Java) — Daily Notes

Date: \***\*\_\_\*\***  
Goal: Lock in core OOP concepts + container basics with clean examples.

---

## 1) Core OOP Mental Model

### Everything is an object (mostly)

In Java, most things are objects, but **primitives** exist too:

- Objects: `String`, `ArrayList`, `HashMap`, `Circle`
- Primitives: `int`, `double`, `boolean`, `char`

> OOP mindset: treat objects like **service providers** with a clear API.

---

## 2) Interfaces (General Meaning vs Java `interface`)

### “Interface” (general meaning)

The **public methods** you can call on an object are its _interface_.

Example: A `List` provides methods like:

- `add()`
- `get()`
- `size()`

### Java `interface` keyword

A real Java `interface` is a contract that classes can implement.

```java
interface Drawable {
  void draw();
}
```

---

## 3) Access Modifiers

### `public`

Accessible from anywhere.

```java
public class A {
  public void hello() {}
}
```

### `private`

Accessible only inside the **same class**.

```java
class BankAccount {
  private double balance;

  public void deposit(double amount) {
    balance += amount;
  }
}
```

### `protected`

Accessible from:

- same class
- same package
- subclasses (even in other packages)

```java
class Shape {
  protected String color;
}
```

### package-private (default)

No modifier = accessible only inside the same package.

```java
class Helper {
  void internalOnly() {}
}
```

---

## 4) is-a vs has-a (composition)

### is-a = inheritance

A `Circle` **is-a** `Shape`.

```java
class Shape {
  void draw() {
    System.out.println("Drawing shape");
  }
}

class Circle extends Shape {
  @Override
  void draw() {
    System.out.println("Drawing circle");
  }

  void radius() {
    System.out.println("Circle radius logic");
  }
}
```

### has-a = composition

A `Car` **has-a** `Engine`.

```java
class Engine {
  void start() {
    System.out.println("Engine started");
  }
}

class Car {
  private Engine engine = new Engine();

  void startCar() {
    engine.start();
  }
}
```

> If something “contains” another object, that’s usually **has-a**.

---

## 5) Early Binding vs Late Binding

### Early binding (compile-time)

The compiler decides which method to call **at compile time**.

Common cases:

- **method overloading**
- `static`, `private`, `final` methods
- fields (variables)

#### Overloading example (early binding)

```java
class Printer {
  void print(int x) {
    System.out.println("int: " + x);
  }

  void print(double x) {
    System.out.println("double: " + x);
  }
}

public class Main {
  public static void main(String[] args) {
    Printer p = new Printer();
    p.print(5);    // chooses print(int)
    p.print(5.0);  // chooses print(double)
  }
}
```

---

### Late binding (runtime)

Happens when a method is **overridden**.
Java decides which method implementation to call **at runtime** based on the real object type.

#### Overriding example (late binding)

```java
class Animal {
  void speak() {
    System.out.println("...");
  }
}

class Dog extends Animal {
  @Override
  void speak() {
    System.out.println("woof");
  }
}

public class Main {
  public static void main(String[] args) {
    Animal a = new Dog();
    a.speak(); // "woof" (runtime dispatch)
  }
}
```

---

## 6) Upcasting (Polymorphism)

### What is upcasting?

Treating a subclass object as its superclass type.

```java
Shape s = new Circle(); // upcasting (implicit)
```

### What changes?

- Reference type controls **what you can call**
- Object type controls **what runs** (overridden methods)

```java
Shape s = new Circle();
s.draw();    // Circle.draw() runs (late binding)
// s.radius(); // ERROR: Shape reference doesn't know radius()
```

---

## 7) Containers (Collections) Overview

Java containers help store and organize data.

### List

- ordered
- duplicates allowed
- index-based access

### Set

- **unique values only**
- no duplicates

### Map

- key → value association
- keys are unique

### Stack

- LIFO (last in, first out)

### Queue

- FIFO (first in, first out)

### Trees

- hierarchical structure (nodes + children)

---

## 8) ArrayList vs LinkedList (Important!)

### ArrayList (dynamic array)

**Fast**

- `get(i)` → O(1)
- `add(value)` at end → amortized O(1)

**Slow**

- insert/remove in middle → O(n) due to shifting

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();
    list.add(10);
    list.add(20);
    System.out.println(list.get(1)); // 20
  }
}
```

---

### LinkedList (node-based)

**Fast**

- add/remove at ends (if using it like a queue/deque)

**Slow**

- `get(i)` → O(n)

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    LinkedList<String> names = new LinkedList<>();
    names.add("Jae");
    names.add("Song");
    System.out.println(names.get(1)); // "Song" (but slower than ArrayList)
  }
}
```

> In real Java and interviews, **ArrayList is usually preferred**.

---

## 9) Common Containers You’ll Use Most

### HashMap (key → value)

Used for counting and fast lookups.

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    String s = "aabccc";
    Map<Character, Integer> freq = new HashMap<>();

    for (char c : s.toCharArray()) {
      freq.put(c, freq.getOrDefault(c, 0) + 1);
    }

    System.out.println(freq); // {a=2, b=1, c=3}
  }
}
```

---

### HashSet (unique values)

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    Set<Integer> seen = new HashSet<>();
    seen.add(1);
    seen.add(1);
    seen.add(2);

    System.out.println(seen); // [1, 2]
  }
}
```

---

## 10) Stack + Queue in Java (Use Deque!)

Avoid using `Stack` class. Prefer `Deque` (ArrayDeque).

### Stack (LIFO)

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    Deque<Integer> stack = new ArrayDeque<>();
    stack.push(10);
    stack.push(20);

    System.out.println(stack.pop()); // 20
    System.out.println(stack.pop()); // 10
  }
}
```

### Queue (FIFO)

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    Deque<Integer> queue = new ArrayDeque<>();
    queue.offer(10);
    queue.offer(20);

    System.out.println(queue.poll()); // 10
    System.out.println(queue.poll()); // 20
  }
}
```

---

## 11) Tiny Combined Exercise (OOP + Containers)

### Goal

Store shapes in a list and draw them polymorphically.

```java
import java.util.*;

class Shape {
  void draw() {
    System.out.println("Drawing shape");
  }
}

class Circle extends Shape {
  @Override
  void draw() {
    System.out.println("Drawing circle");
  }
}

class Square extends Shape {
  @Override
  void draw() {
    System.out.println("Drawing square");
  }
}

public class Main {
  public static void main(String[] args) {
    List<Shape> shapes = new ArrayList<>();
    shapes.add(new Circle());
    shapes.add(new Square());

    for (Shape s : shapes) {
      s.draw(); // late binding chooses correct draw()
    }
  }
}
```

---

## Key Takeaways (1-minute review)

- Overloading → **early binding**
- Overriding → **late binding**
- Upcasting enables **polymorphism**
- `ArrayList` is the default List choice
- `HashMap` is the most important interview container
- Use `Deque` for stack/queue behavior
- `Set` = uniqueness (no duplicates)

---
