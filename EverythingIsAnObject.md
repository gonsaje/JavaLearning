# Everything is an Object

Java is oriented towards OOP.
In Java, not everything is an object:

- Objects: `String`, `ArrayList`, `HashMap`, `Circle`
- Primitives: `int`, `double`, `boolean`, `char`
- Almost everything else is

## Object Manipulation

- Objects are manipulated by their references.

```java
String s;

// It is always safer to initialize a reference with a value
String s = "asdf";
```

> _Note_: Strings can be specially initialized with just quotes. Most times you must follow a more general type of initialization.

```java
String a = "hello";             // uses String pool
String b = new String("hello"); // creates a new String object (rarely needed)
String empty = "";              // empty string
```

## Where Storage Lives

1. Registers
   - Fastest storage (CPU internal)
   - Very limited
2. The stack
   - Stores method call frames + local variables
   - Very fast
   - Object references live here (the objects themselves do not)
3. The heap
   - Stores all objects created with new
   - Garbage collected
4. Constant Storage
   - Stores constants / literals (implementation detail varies)
5. Non-RAM Storage
   - Disk / network storage
   - Used for persistence (files, databases, etc.)

---

## Primitive Types

Primitives are not objects. They store values directly.

| Primitive Type |                                      Size |                         Minimum |                     Maximum | Wrapper Type | Short description                                                                             |
| -------------- | ----------------------------------------: | ------------------------------: | --------------------------: | ------------ | --------------------------------------------------------------------------------------------- |
| `boolean`      | JVM-dependent (commonly 1 byte in arrays) |                         `false` |                      `true` | `Boolean`    | Logical true/false value (not a numeric type).                                                |
| `char`         |                          16-bit (2 bytes) |                    `\u0000` (0) |           `\uFFFF` (65,535) | `Character`  | Single UTF-16 code unit (represents many common characters, but not all Unicode as one char). |
| `byte`         |                            8-bit (1 byte) |                          `-128` |                       `127` | `Byte`       | Small signed integer, often used for raw binary data.                                         |
| `short`        |                          16-bit (2 bytes) |                       `-32,768` |                    `32,767` | `Short`      | Signed integer, larger than `byte` but smaller than `int`.                                    |
| `int`          |                          32-bit (4 bytes) |                `-2,147,483,648` |             `2,147,483,647` | `Integer`    | Default integer type for most calculations.                                                   |
| `long`         |                          64-bit (8 bytes) |    `-9,223,372,036,854,775,808` | `9,223,372,036,854,775,807` | `Long`       | Large signed integer, often used for timestamps and big counts.                               |
| `float`        |                          32-bit (4 bytes) |  ~`1.4E-45` (smallest positive) |             ~`3.4028235E38` | `Float`      | Single-precision floating-point (approx. 6–7 decimal digits).                                 |
| `double`       |                          64-bit (8 bytes) | ~`4.9E-324` (smallest positive) |   ~`1.7976931348623157E308` | `Double`     | Double-precision floating-point (approx. 15–16 decimal digits).                               |
| `void`         |                                       N/A |                             N/A |                         N/A | `Void`       | No value / no return type (used for methods that return nothing).                             |

```java
public class PrimitiveExamples {
  public static void main(String[] args) {
    boolean isActive = true;

    char grade = 'A';
    char unicodeHeart = '\u2665'; // ♥ (Unicode char)

    byte smallNum = 100;
    short shortNum = 30000;
    int age = 28;
    long population = 8_000_000_000L;

    float piApprox = 3.14159f;
    double precisePi = 3.141592653589793;

    System.out.println("boolean: " + isActive);
    System.out.println("char: " + grade + " " + unicodeHeart);
    System.out.println("byte: " + smallNum);
    System.out.println("short: " + shortNum);
    System.out.println("int: " + age);
    System.out.println("long: " + population);
    System.out.println("float: " + piApprox);
    System.out.println("double: " + precisePi);

    // void example (method that returns nothing)
    printMessage("This method returns void.");
  }

  static void printMessage(String msg) {
    System.out.println("void method says: " + msg);
  }
}
```

Wrapper classes allow primitives to be treated like objects.

```java
char c = 'x';

Character ch = c; // autoboxing
```

Wrappers are useful for collections like ArrayList<Integer>.

## High Precision Numbers

Think of these as “numbers with no limits” (but slower).

Java primitives like int, long, float, double are fast because the CPU knows exactly how to store them.
BigInteger and BigDecimal are objects that can grow as big as needed, so they’re more accurate but slower.

You can do anything you do on **BigInteger** and **BigDecimal** as you do on **int** or **float**.\
However, operations will be slower as you're trading speed for accuracy.

- **BigInteger**: _whole numbers only, unlimited size_
  - No decimals. Only integers.
  - Used when `long` isn’t enough.
  - Usecases:
    - huge counters \ IDs
    - cryptography _(RSA Keys)_
    - factorials _(i.e. 1000!)_
    - anything where overflow would be deadly

```java
import java.math.BigInteger;
import java.math.BigDecimal;

BigInteger a = new BigInteger("9999999999999999999999999999");
BigInteger b = new BigInteger("2");
System.out.println(a.multiply(b));
```

- **BigDecimal**: _decimal numbers, exact precision_
  - Can store decimals wihtout floating-point rounding errors
  - Used when accuracy matters more than speed
  - Always construct BigDecimal from String, not double, to avoid floating error.
    - Good: new BigDecimal("0.1")
    - Bad: new BigDecimal(0.1)
  - Usescases:
    - money _(i.e. prices, taxes, interest)_
    - finance calculations
    - precise measurements
    - anything where something like 0.1 + 0.2 must be correct

```java
BigDecimal x = new BigDecimal("0.1");
BigDecimal y = new BigDecimal("0.2");
System.out.println(x.add(y)); // 0.3 exactly
```

## Arrays in Java

- Arrays have fixed length
- Bounds are checked (no out-of-range access)
- Arrays of objects store references
- Uninitialized object references default to null

```java
String[] names = new String[3];
System.out.println(names[0]); // null
```

---

In Java, you never need to destroy objects. Unlike some other languages, Java does all the clean up for you.

## Scoping

- Scoping is determined by the placement of curly brackets

```java
{
    int x = 12;
    // here only x is available
    {
        int q = 96;
        // both x & q are available
    }
    // Here q becomes out of scope and only x is available
}
```

In Java, unlike C or C++, you cannout "hide" outer-scoped variables

```java
{
    int x = 12;
    {
        int x = 96; // This is illegal in Java
    }
}
```

However, Java objects don't have the same lifetime as primitives. \
If an object is created using **new**, the object can persist past scope only if there is still a reference to it somewhere.

```java
{
    String s = new String("a str");
}
// s is out of scope here; object can be GC'd if nothing else references it
```

In languages like C++, this would be a point of concern, but in Java, objects that are no longer needed are automatically cleaned up.

---

## Class

Class keyword defines a new type of object

```java
class ATypeName {
    /*
        Class logic here...
    */
}

ATypeName a = new ATypeName();
```

## Fields and Methods

- Fields
  - Data members
  - an object of any type or a primitive type
- Methods
  - Member functions

```java
class ConceptCar {
    int mkVersion = 6;
    String vin = "6kvb12ak089jka";

    Engine engine = new Engine();
    Computer computer = new Computer();

    boolean checkEngineLight = false;

    void start() {
        checkEngineLight = computer.shouldShowCheckEngine(engine);

        if (checkEngineLight) {
            System.out.println("⚠️ Check Engine Light ON");
        } else {
            System.out.println("✅ Engine OK");
        }
    }
}

class Engine {
    int mileage = 1000;
    boolean needsService = false;
    int nextServiceAt = 10000;

    void addMiles(int miles) {
        mileage += miles;
        if (mileage >= nextServiceAt) {
            needsService = true;
        }
    }

    void service() {
        needsService = false;
        nextServiceAt += 10000;
    }
}

class Computer {
    boolean shouldShowCheckEngine(Engine engine) {
        return engine.needsService;
    }

    boolean isWarrantyValid(String vin) {
        return vin != null && vin.length() > 0;
    }
}
```
