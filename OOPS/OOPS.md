# JAVA OOP INTERVIEW PREPARATION — MASTER HANDBOOK

---

# UNIT I: Introduction to Java & Object-Oriented Programming

---

## Topic 1: OOP Fundamentals & The Four Pillars

### 1. Definition
Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects", which contain data in the form of fields (attributes) and code in the form of procedures (methods). It organizes software design around data and objects rather than functions and logic.

### 2. Key Points
* Focuses on objects and data security rather than procedural step-by-step logic.
* Promotes modular code structure, making software easier to test, debug, and maintain.
* Enables code reusability through inheritance and class hierarchies.
* Provides high data security using encapsulation and restricted access modifiers.
* Maps real-world entities directly into software components.

### 3. Syntax
```java
// Basic OOP Structure
class Car {
    // Data (Fields)
    private String model;
    
    // Behavior (Methods)
    public void drive() {
        System.out.println("Car is moving");
    }
}
```

### 4. Simple Example
```java
class BankAccount {
    private double balance; // Encapsulation

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        account.deposit(500.0);
        System.out.println("Balance: " + account.getBalance());
    }
}
```
* `private double balance`: Restricts direct modification of data from outside the class.
* `deposit(double amount)`: Validates and updates the state securely.
* `getBalance()`: Provides controlled access to read the data.

### 5. How It Works
```text
Real-World Entity (Bank Account)
       ↓
Class Definition (Blueprint with balance & deposit method)
       ↓
Object Instantiation (`new BankAccount()`)
       ↓
Method Invocation (`account.deposit(500.0)`)
       ↓
State Updated & Encapsulated in Heap Memory
```

### 6. Important Interview Points
* Java is an Object-Oriented language, but not 100% pure OOP because it supports primitive types (`int`, `char`, etc.).
* The 4 Pillars of OOP are Encapsulation, Abstraction, Inheritance, and Polymorphism.
* OOP improves software maintainability compared to Procedural Programming (like C).
* In Java, `java.lang.Object` is the root class of all object hierarchies.
* Data security and modularity are the primary reasons OOP is used in enterprise applications.

### 7. Common Differences
| Aspect | Procedural Programming (C) | Object-Oriented Programming (Java) |
| :--- | :--- | :--- |
| **Primary Focus** | Functions and algorithms | Data and objects |
| **Security** | Low (Data moves freely) | High (Data hidden inside objects) |
| **Approach** | Top-Down approach | Bottom-Up approach |
| **Reusability** | Limited (Functions) | High (Inheritance & Polymorphism) |

### 8. Real-World Analogy
**Class** is a architectural blueprint of a house, and **Object** is the actual physical house built from that blueprint.

### 9. 5-Mark Answer
**OOP Fundamentals & The Four Pillars**

**Definition:** Object-Oriented Programming (OOP) is a programming paradigm organized around objects that encapsulate data (fields) and behavior (methods).

**Key Points:**
1. Encapsulation: Bundles data and methods together while restricting direct access.
2. Abstraction: Shows essential features while hiding internal complex implementation details.
3. Inheritance: Allows a child class to inherit fields and methods from a parent class.
4. Polymorphism: Allows one interface or method to perform different actions based on the object.
5. Reusability & Security: Reduces code duplication and secures sensitive state data.

**Example:**
```java
class Vehicle {
    void start() { System.out.println("Starting vehicle"); }
}
class Car extends Vehicle {
    void start() { System.out.println("Starting car engine"); }
}
```

### Quick Revision
1. **OOP** → Paradigm centered around objects and data.
2. **Encapsulation** → Data wrapping & hiding.
3. **Abstraction** → Hiding complexity, exposing essential features.
4. **Inheritance** → Code reuse from parent to child class.
5. **Polymorphism** → Single interface, multiple implementations.

---

## Topic 2: Class, Object, State, Behavior & Identity

### 1. Definition
A **Class** is a user-defined template or blueprint from which objects are created. An **Object** is a basic runtime instance of a class containing State (attributes), Behavior (methods), and a unique Identity (memory address).

### 2. Key Points
* A Class is a logical entity and consumes no heap memory by itself.
* An Object is a physical entity allocated on heap memory during runtime using `new`.
* **State** represents the data or properties stored in instance variables.
* **Behavior** represents the functionality exposed via instance methods.
* **Identity** is a unique identifier (conceptually memory address) distinguishing objects.

### 3. Syntax
```java
class ClassName {
    // State (Fields)
    dataType fieldName;

    // Behavior (Methods)
    returnType methodName() {
        // Implementation
    }
}

// Object Instantiation
ClassName obj = new ClassName();
```

### 4. Simple Example
```java
class Student {
    String name; // State
    int rollNo;  // State

    void displayInfo() { // Behavior
        System.out.println(name + " - Roll No: " + rollNo);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student(); // Object 1
        s1.name = "Alice";
        s1.rollNo = 101;

        Student s2 = new Student(); // Object 2
        s2.name = "Alice";
        s2.rollNo = 101;

        s1.displayInfo();
        System.out.println(s1 == s2); // false (Different Identities)
    }
}
```
* `Student s1` declares a reference variable on the stack.
* `new Student()` allocates memory for fields `name` and `rollNo` on the heap.
* `s1 == s2` evaluates to `false` because both objects have separate identities/memory addresses despite identical state.

### 5. How It Works
```text
Class Loaded into JVM Metaspace
       ↓
`new Student()` executed in Stack
       ↓
Memory allocated on Heap for Student instance
       ↓
Instance Variables initialized to default values (null, 0)
       ↓
Reference Address returned and assigned to variable `s1`
```

### 6. Important Interview Points
* Classes do not take memory for instance variables until objects are instantiated.
* Two objects with identical state values still have distinct identities in memory.
* An object reference variable resides on the stack, while the actual object resides on the heap.
* Invoking a method on a `null` reference throws a runtime `NullPointerException`.
* An anonymous object (`new Student().displayInfo();`) is created without saving its reference.

### 7. Common Differences
| Aspect | Class | Object |
| :--- | :--- | :--- |
| **Nature** | Logical template/blueprint | Physical instance of a class |
| **Memory** | Occupies no heap memory | Occupies heap memory when created |
| **Declaration** | Created using `class` keyword | Created using `new` keyword |
| **Existence** | Declared once in code | Created multiple times at runtime |

### 8. Real-World Analogy
**Class** is a blueprint drawing of a smartphone; **Object** is the actual physical phone in your hand with state (battery level, color) and behavior (call, text).

### 9. 5-Mark Answer
**Class, Object, State, Behavior & Identity**

**Definition:** A Class is a user-defined blueprint defining data and methods. An Object is a runtime instance of a class characterized by State, Behavior, and Identity.

**Key Points:**
1. **Class:** Logical blueprint that does not occupy heap memory.
2. **Object:** Physical entity instantiated on heap memory via `new`.
3. **State:** Represented by values stored in instance variables.
4. **Behavior:** Actions performed by the object via methods.
5. **Identity:** Unique memory location that distinguishes every object instance.

**Example:**
```java
class Pen {
    String color = "Blue"; // State
    void write() { System.out.println("Writing in " + color); } // Behavior
}
Pen p1 = new Pen(); // Object with unique identity
```

### Quick Revision
1. **Class** → Logical template / blueprint.
2. **Object** → Physical instance on heap memory.
3. **State** → Object attributes/variable values.
4. **Behavior** → Actions/methods of the object.
5. **Identity** → Unique memory reference distinguishing instances.

---

## Topic 3: Class Anatomy & Primitive vs Reference Types

### 1. Definition
**Class Anatomy** refers to the constituent components of a Java class file (package, imports, fields, methods, constructors, blocks). **Primitive Types** store raw values directly, whereas **Reference Types** store reference addresses pointing to objects on the heap.

### 2. Key Points
* A class can contain fields, constructors, methods, static blocks, and nested classes.
* Java has 8 **Primitive Types**: `byte`, `short`, `int`, `long`, `float`, `double`, `char`, `boolean`.
* **Reference Types** include Classes, Interfaces, Arrays, Enums, and Strings.
* Primitives store values on stack (or inline in heap object fields); References store heap object addresses.
* Primitives default to numeric zero/`false`; Reference types default to `null`.

### 3. Syntax
```java
// Primitive declaration
int count = 10;

// Reference declaration
String text = new String("Hello");
int[] numbers = new int[5];
```

### 4. Simple Example
```java
public class Demo {
    // Instance variable (Reference type)
    String title = "Java OOP";

    public static void main(String[] args) {
        int primitiveVar = 50; // Primitive (Stack)
        Demo refVar = new Demo(); // Reference (Address to Heap)

        System.out.println("Primitive: " + primitiveVar);
        System.out.println("Reference Title: " + refVar.title);
    }
}
```
* `primitiveVar` directly holds literal value `50`.
* `refVar` holds memory reference address pointing to `Demo` instance on heap.

### 5. How It Works
```text
Stack Frame Created for `main()`
       ├─ primitiveVar: Holds value [50] directly
       └─ refVar: Holds address [0x7A9B]
                               ↓
                        Heap Memory [0x7A9B]
                               └─ title: "Java OOP"
```

### 6. Important Interview Points
* Primitive types are not objects and do not inherit from `java.lang.Object`.
* Comparison (`==`) on primitives compares values; on references, it compares memory addresses.
* Passing primitives to methods passes a copy of value; passing reference variables passes a copy of the reference address.
* Wrapper classes (`Integer`, `Double`) wrap primitives to allow usage in Collections.
* Auto-boxing automatically converts primitives to their corresponding wrapper object.

### 7. Common Differences
| Aspect | Primitive Type | Reference Type |
| :--- | :--- | :--- |
| **Data Stored** | Actual value directly | Memory reference address of object |
| **Memory Location** | Stack memory (for local variables) | Reference on Stack, Object on Heap |
| **Default Value** | `0`, `0.0`, `false`, `'\u0000'` | `null` |
| **Methods** | No methods available | Methods can be called on object |

### 8. Real-World Analogy
**Primitive** is cash held directly in your pocket. **Reference Type** is a paper check containing the bank address and account number pointing to funds stored in the vault.

### 9. 5-Mark Answer
**Class Anatomy & Primitive vs Reference Types**

**Definition:** Class Anatomy comprises fields, methods, constructors, and blocks. Primitive types hold direct values while Reference types hold memory addresses of heap objects.

**Key Points:**
1. Class components include package, imports, fields, constructors, and methods.
2. Java provides 8 primitive types (`int`, `double`, `boolean`, etc.).
3. Reference types include classes, interfaces, arrays, and strings.
4. Primitives default to standard zero/false; reference types default to `null`.
5. `==` checks value equality for primitives and address equality for reference types.

**Example:**
```java
int val = 100; // Primitive variable holding value 100
String msg = new String("Hi"); // Reference variable holding heap object address
```

### Quick Revision
1. **Class Anatomy** → Package, imports, variables, constructors, methods.
2. **Primitives** → 8 basic data types storing raw values (`int`, `boolean`, etc.).
3. **Reference Types** → Store addresses pointing to objects on heap.
4. **Stack vs Heap** → Local primitives on Stack; objects on Heap.
5. **Default Value** → `0`/`false` for primitives, `null` for reference types.

---

## Topic 4: Constructors (Default, Parameterized, Overloading, Chaining)

### 1. Definition
A **Constructor** is a special block of code called automatically when an object is instantiated using `new`. Its primary purpose is to initialize instance variables of the object.

### 2. Key Points
* Constructor name must exactly match the class name and must not have any return type (not even `void`).
* **Default Constructor**: Provided automatically by Java if no explicit constructor is defined in the class.
* **Parameterized Constructor**: Takes arguments to initialize fields with user-defined values.
* **Constructor Overloading**: Defining multiple constructors in the same class with different parameter signatures.
* **Constructor Chaining**: Calling one constructor from another using `this()` (same class) or `super()` (parent class).

### 3. Syntax
```java
class Example {
    // Default / No-arg constructor
    Example() {
        this(10); // Constructor chaining using this()
    }

    // Parameterized constructor
    Example(int x) {
        // Initialization
    }
}
```

### 4. Simple Example
```java
class Book {
    String title;
    double price;

    // No-arg constructor
    Book() {
        this("Untitled", 0.0); // Chains to parameterized constructor
    }

    // Parameterized constructor
    Book(String title, double price) {
        this.title = title;
        this.price = price;
    }

    void display() {
        System.out.println(title + " : $" + price);
    }
}

public class Main {
    public static void main(String[] args) {
        Book b1 = new Book();
        Book b2 = new Book("Java Guide", 29.99);
        b1.display();
        b2.display();
    }
}
```
* `this("Untitled", 0.0)` demonstrates constructor chaining within the same class.
* `this.title = title` resolves variable shadowing between field and parameter.

### 5. How It Works
```text
`new Book()` called in main
       ↓
Control goes to No-arg `Book()` constructor
       ↓
`this("Untitled", 0.0)` executes FIRST line
       ↓
Control redirects to Parameterized `Book(String, double)`
       ↓
Fields `title` and `price` assigned values on Heap → Object Ready
```

### 6. Important Interview Points
* If you write ANY custom constructor, Java compiler will NOT generate the default no-arg constructor.
* `this()` or `super()` MUST be the first statement inside a constructor body.
* Constructors cannot be `static`, `final`, `abstract`, or `synchronized`.
* Constructors are NOT inherited by child classes.
* A constructor can be marked `private` to restrict object creation (used in Singleton design pattern).

### 7. Common Differences
| Aspect | Method | Constructor |
| :--- | :--- | :--- |
| **Purpose** | Performs specific action/behavior | Initializes object state |
| **Return Type** | Must have a return type (`void`, etc.) | No return type at all |
| **Invocation** | Invoked explicitly by name | Invoked implicitly via `new` |
| **Name** | Any valid identifier | Must match class name exactly |

### 8. Real-World Analogy
Constructor is like an automated assembly line step in a car factory that fits tires and engine into a new frame as soon as production begins.

### 9. 5-Mark Answer
**Constructors (Types & Chaining)**

**Definition:** A constructor is a special member function used to initialize objects when instantiated with `new`. It shares the class name and lacks a return type.

**Key Points:**
1. **Default Constructor:** Inserted automatically by compiler only if no constructors are explicitly defined.
2. **Parameterized Constructor:** Accepts parameters to set initial object state dynamically.
3. **Overloading:** Multiple constructors sharing class name but with different parameter lists.
4. **Constructor Chaining:** Calling another constructor using `this()` or `super()`.
5. **Rule:** `this()` or `super()` call must be the absolute first statement in the constructor.

**Example:**
```java
class Account {
    int id;
    Account() { this(100); } // Chaining
    Account(int id) { this.id = id; }
}
```

### Quick Revision
1. **Constructor** → Special method initializing objects during creation.
2. **No Return Type** → Never specify return type (not even `void`).
3. **Default Constructor** → Provided by Java compiler if 0 constructors exist.
4. **`this()`** → Calls another constructor in the same class (must be 1st line).
5. **Private Constructor** → Prevents instantiation from outside (Singletons).

---

## Topic 5: Encapsulation & Data Hiding

### 1. Definition
**Encapsulation** is the OOP mechanism of wrapping variables (data) and methods (code) together into a single unit (class) while restricting direct access to object components using private access modifiers.

### 2. Key Points
* Achieved by declaring class variables as `private` and exposing public getter/setter methods.
* Protects internal object state from unauthorized modification or corruption from outside classes.
* Implements **Data Hiding** by shielding internal data representation details.
* Makes class fields read-only or write-only by selectively providing getter or setter methods.
* Improves maintainability because internal implementation can change without breaking client code.

### 3. Syntax
```java
class EncapsulatedClass {
    // Private data fields (Hidden)
    private String data;

    // Public Getter
    public String getData() {
        return data;
    }

    // Public Setter
    public void setData(String data) {
        this.data = data;
    }
}
```

### 4. Simple Example
```java
class Employee {
    private double salary; // Private field (Data Hiding)

    // Getter
    public double getSalary() {
        return salary;
    }

    // Setter with validation logic
    public void setSalary(double salary) {
        if (salary > 0) {
            this.salary = salary;
        } else {
            System.out.println("Invalid salary amount!");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Employee emp = new Employee();
        emp.setSalary(50000); // Access via public method
        System.out.println("Salary: " + emp.getSalary());
    }
}
```
* `private double salary` prevents external code (`emp.salary = -500`) from introducing invalid state.
* `setSalary(double)` enforces validation before setting state.

### 5. How It Works
```text
External Client Code requests update (`emp.setSalary(50000)`)
       ↓
Public Setter Method executes validation check (`if salary > 0`)
       ↓
Validation passes → Internal Private state variable updated
       ↓
Direct illegal access (`emp.salary`) blocked at Compile Time
```

### 6. Important Interview Points
* Encapsulation is also known as "Data Hiding" or "Information Hiding".
* A fully encapsulated Java class has all data members declared as `private`.
* Access Modifiers (`private`, `default`, `protected`, `public`) control the level of encapsulation.
* JavaBeans standard strictly requires private properties with public getters/setters.
* Encapsulation enables fields to be declared read-only (getter only) or write-only (setter only).

### 7. Common Differences
| Aspect | Data Hiding | Encapsulation |
| :--- | :--- | :--- |
| **Concept** | Focusing on restricting data access | Wrapping data and methods into a single class unit |
| **Implementation** | Achieved using `private` access modifier | Achieved using classes, `private` fields, and getters/setters |
| **Objective** | Security of data against invalid external access | Modularity and bundling of data + behavior |

### 8. Real-World Analogy
A **Capsule Pill** encapsulates medicinal powder inside a gel shell, protecting the contents from external exposure and releasing it in a controlled manner.

### 9. 5-Mark Answer
**Encapsulation & Data Hiding**

**Definition:** Encapsulation is the bundling of data and methods into a single class while restricting direct access to fields using access modifiers.

**Key Points:**
1. Achieved by marking class variables as `private`.
2. Access is provided using `public` getter and setter methods.
3. Allows enforcement of validation logic before modifying internal state.
4. Enables creation of read-only or write-only class properties.
5. Protects code against unintended external side-effects and increases security.

**Example:**
```java
class User {
    private String password;
    public void setPassword(String pwd) {
        if (pwd.length() >= 8) this.password = pwd;
    }
}
```

### Quick Revision
1. **Encapsulation** → Wrapping data & behavior in a single class unit.
2. **Data Hiding** → Marking variables `private` to prevent direct access.
3. **Getters/Setters** → Controlled public entry points for reading/writing data.
4. **Validation** → Setters can reject invalid input values.
5. **Read-Only Class** → Class having only getter methods without setters.

---

# UNIT II: Inheritance, Polymorphism & Abstraction

---

## Topic 6: Inheritance & `extends` Keyword

### 1. Definition
**Inheritance** is an OOP mechanism where a child (subclass) acquires the properties (fields) and behaviors (methods) of a parent (superclass) using the `extends` keyword, establishing an **IS-A** relationship.

### 2. Key Points
* Promotes code reusability by allowing subclasses to reuse parent class logic.
* Java supports **Single**, **Multilevel**, and **Hierarchical** inheritance using classes.
* Java does **NOT** support **Multiple Inheritance** using classes (to avoid Diamond Problem ambiguity).
* `private` members of a superclass are inherited conceptually but cannot be accessed directly by subclasses.
* `Object` class is the implicit parent superclass of every class in Java.

### 3. Syntax
```java
class ParentClass {
    // Parent fields and methods
}

class ChildClass extends ParentClass {
    // Child inherits parent members and adds new ones
}
```

### 4. Simple Example
```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal { // IS-A relationship: Dog IS-A Animal
    void bark() {
        System.out.println("Dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.eat();  // Inherited method from Animal
        myDog.bark(); // Subclass specific method
    }
}
```
* `class Dog extends Animal` declares `Dog` as a subclass of `Animal`.
* `myDog.eat()` calls the superclass method reused by `Dog`.

### 5. How It Works
```text
Compiler processes `Dog extends Animal`
       ↓
`Dog` object allocated on Heap with fields from both `Animal` and `Dog`
       ↓
`myDog.eat()` invoked → Lookup finds `eat()` in `Animal` class
       ↓
`Animal.eat()` executed successfully
```

### 6. Important Interview Points
* Subclass constructor always invokes superclass constructor first (implicitly via `super()`).
* `final` classes cannot be extended (e.g., `java.lang.String` is `final`).
* Composition (HAS-A) is preferred over Inheritance (IS-A) in modern software design for loose coupling.
* Multiple inheritance with classes is forbidden in Java; it is supported via Interfaces.
* Constructors and static initializer blocks are NOT inherited by subclasses.

### 7. Common Differences
| Aspect | Single Inheritance | Multilevel Inheritance |
| :--- | :--- | :--- |
| **Structure** | Subclass derives from one Superclass (`B extends A`) | Subclass derives from a class that is also a subclass (`C extends B extends A`) |
| **Hierarchy Level** | 2 levels (Parent -> Child) | 3+ levels (Grandparent -> Parent -> Child) |
| **Complexity** | Simple linear inheritance | Deeper chain of inherited state/behavior |

### 8. Real-World Analogy
Child inherits genetic traits (eye color, height) from a Parent, while also acquiring unique personal skills (playing guitar).

### 9. 5-Mark Answer
**Inheritance & extends Keyword**

**Definition:** Inheritance is a mechanism where a child class acquires non-private fields and methods of a parent class using `extends`, establishing an IS-A relationship.

**Key Points:**
1. Enables code reusability and hierarchical class modeling.
2. Formulates an IS-A relationship between child and parent.
3. Java supports Single, Multilevel, and Hierarchical class inheritance.
4. Java disallows Multiple Inheritance using classes to prevent ambiguity (Diamond Problem).
5. `super()` is automatically invoked in child constructors to initialize parent members.

**Example:**
```java
class Shape { void draw() { System.out.println("Drawing shape"); } }
class Circle extends Shape { } // Circle inherits draw()
```

### Quick Revision
1. **Inheritance** → Subclass acquires parent class state and behavior.
2. **`extends`** → Keyword used to inherit from a class.
3. **IS-A Relationship** → `Dog IS-A Animal`.
4. **No Multiple Class Inheritance** → Prevents Diamond Problem ambiguity.
5. **`final` Class** → Cannot be inherited/extended.

---

## Topic 7: Polymorphism (Compile-Time / Overloading vs Runtime / Overriding)

### 1. Definition
**Polymorphism** (meaning "many forms") is the ability of a method or object to take on different behaviors based on context. It is classified into **Compile-Time Polymorphism** (Method Overloading) and **Runtime Polymorphism** (Method Overriding).

### 2. Key Points
* **Method Overloading**: Methods in the same class sharing name but differing in parameter count, type, or order.
* **Method Overriding**: Subclass providing a specific implementation of a method declared in parent class.
* Method Overloading is resolved at **Compile-Time** based on reference types.
* Method Overriding is resolved at **Runtime** based on actual object type (Dynamic Method Dispatch).
* Subclass override method must match parent method's signature exactly and cannot reduce visibility.

### 3. Syntax
```java
// Method Overloading (Compile-Time)
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }

// Method Overriding (Runtime)
class Parent { void show() {} }
class Child extends Parent { @Override void show() {} }
```

### 4. Simple Example
```java
class Calculator { // Overloading Example
    int multiply(int a, int b) { return a * b; }
    double multiply(double a, double b) { return a * b; }
}

class Parent { // Overriding Example
    void greet() { System.out.println("Hello from Parent"); }
}
class Child extends Parent {
    @Override
    void greet() { System.out.println("Hello from Child"); }
}

public class Main {
    public static void main(String[] args) {
        // Overloading (Compile Time)
        Calculator calc = new Calculator();
        System.out.println(calc.multiply(2, 3));

        // Overriding / Runtime Polymorphism
        Parent obj = new Child(); // Parent reference pointing to Child object
        obj.greet(); // Prints "Hello from Child" at runtime
    }
}
```
* `calc.multiply(2, 3)` resolves to `int` version at compile time based on parameter types.
* `Parent obj = new Child()` invokes `Child`'s `greet()` at runtime due to Dynamic Method Dispatch.

### 5. How It Works (Dynamic Method Dispatch)
```text
`Parent obj = new Child()` compiled successfully (compiler checks `greet()` in Parent)
       ↓
Runtime execution reaches `obj.greet()`
       ↓
JVM inspects actual memory object type referenced by `obj` (which is `Child`)
       ↓
JVM invokes `Child.greet()` overriding method dynamically
```

### 6. Important Interview Points
* `static`, `private`, and `final` methods CANNOT be overridden because they use static binding.
* Overloading relies on method signature (parameters); changing return type alone is NOT valid overloading.
* An overriding method can declare sub-type return types (Covariant Return Types).
* Overriding methods cannot throw broader checked exceptions than parent methods.
* `@Override` annotation ensures compiler verifies correct overriding signature.

### 7. Common Differences
| Aspect | Method Overloading | Method Overriding |
| :--- | :--- | :--- |
| **Type** | Compile-Time Polymorphism (Static) | Runtime Polymorphism (Dynamic) |
| **Location** | Occurs within the same class | Occurs between Parent & Child classes |
| **Parameters** | Must be different (count/types) | Must be identical |
| **Private/Static** | Can overload `private`/`static` methods | Cannot override `private`/`static` methods |

### 8. Real-World Analogy
A person acting as a **Teacher** at school, a **Customer** at a store, and a **Parent** at home — same entity behaving differently based on context.

### 9. 5-Mark Answer
**Polymorphism (Overloading vs Overriding)**

**Definition:** Polymorphism allows a single action or method name to exhibit different behaviors depending on parameters (Overloading) or runtime object instance (Overriding).

**Key Points:**
1. **Method Overloading:** Same class, same method name, different parameter signature (Compile-Time).
2. **Method Overriding:** Subclass redefines parent class method with identical signature (Runtime).
3. Overloading is resolved static-binding at compile time.
4. Overriding is resolved dynamic-binding at runtime using Dynamic Method Dispatch.
5. `static`, `private`, and `final` methods cannot be overridden.

**Example:**
```java
// Overriding / Dynamic Polymorphism
Parent p = new Child();
p.display(); // Calls Child display() at runtime
```

### Quick Revision
1. **Polymorphism** → One name, multiple forms/behaviors.
2. **Overloading** → Same class, different parameters (Compile Time).
3. **Overriding** → Child redefines parent method (Runtime).
4. **Dynamic Method Dispatch** → JVM picks overridden method based on Heap object.
5. **Unoverridable Methods** → `private`, `static`, and `final` methods.

---

## Topic 8: Abstraction & Abstract Classes

### 1. Definition
**Abstraction** is the process of hiding internal implementation details and displaying only essential functional features to the user. An **Abstract Class** is a class declared with `abstract` keyword that cannot be instantiated and may contain abstract methods (without body).

### 2. Key Points
* Declared using the `abstract` keyword.
* Abstract classes CANNOT be instantiated directly using `new`.
* Abstract Methods have a declaration/signature but NO method body (`{}`).
* A class extending an abstract class MUST override all abstract methods or be declared `abstract` itself.
* Abstract classes can contain concrete methods (with body), fields, and constructors.

### 3. Syntax
```java
abstract class Shape {
    String color;

    // Abstract Method (No body)
    abstract void draw();

    // Concrete Method
    void getColor() {
        System.out.println(color);
    }
}

class Rectangle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing Rectangle");
    }
}
```

### 4. Simple Example
```java
abstract class Payment {
    double amount;

    // Constructor in abstract class
    Payment(double amount) {
        this.amount = amount;
    }

    // Abstract method to be implemented by child classes
    abstract void processPayment();
}

class CreditCardPayment extends Payment {
    CreditCardPayment(double amount) {
        super(amount);
    }

    @Override
    void processPayment() {
        System.out.println("Processing credit card payment of $" + amount);
    }
}

public class Main {
    public static void main(String[] args) {
        // Payment p = new Payment(100); // Error: Cannot instantiate
        Payment p = new CreditCardPayment(150.0);
        p.processPayment();
    }
}
```
* `Payment` class enforces a common template (`processPayment()`) for all payment types.
* `CreditCardPayment` provides the mandatory concrete implementation.

### 5. How It Works
```text
Define Abstract Parent (`Payment`) with abstract method signature
       ↓
Concrete Child (`CreditCardPayment`) extends Parent and implements body
       ↓
Instantiate Concrete Child (`new CreditCardPayment(150.0)`)
       ↓
Parent Constructor initializes fields -> Child executes overridden abstract method
```

### 6. Important Interview Points
* Abstract classes CAN have constructors (called via child classes using `super()`).
* An abstract class can have 0% to 100% abstraction (can contain all concrete methods, all abstract, or a mix).
* An abstract method CANNOT be marked `private`, `static`, or `final` (because it must be overridden).
* Abstract classes are used when classes share a common state and core identity (IS-A).
* Abstract classes can contain instance variables, whereas interface fields are implicitly `public static final`.

### 7. Common Differences
| Aspect | Concrete Class | Abstract Class |
| :--- | :--- | :--- |
| **Instantiation** | Can be instantiated using `new` | CANNOT be instantiated directly |
| **Abstract Methods** | Cannot contain abstract methods | Can contain abstract methods |
| **Keyword** | Standard `class` declaration | Declared using `abstract class` |
| **Purpose** | Complete blueprint for objects | Partial template to be extended |

### 8. Real-World Analogy
An **ATM Interface** shows abstract buttons like "Withdraw", hiding complex banking database transaction processing logic underneath.

### 9. 5-Mark Answer
**Abstraction & Abstract Classes**

**Definition:** Abstraction hides complex implementation details, exposing only functionality. An Abstract Class (declared via `abstract`) serves as a template that cannot be instantiated directly.

**Key Points:**
1. Abstract classes cannot be instantiated using `new`.
2. Abstract methods have declarations but lack method bodies.
3. Subclasses must implement all inherited abstract methods.
4. Abstract classes can contain constructors, fields, and concrete methods.
5. Abstract methods cannot be `private`, `static`, or `final`.

**Example:**
```java
abstract class Animal {
    abstract void makeSound();
}
class Dog extends Animal {
    void makeSound() { System.out.println("Woof"); }
}
```

### Quick Revision
1. **Abstraction** → Exposing "what" a system does while hiding "how".
2. **`abstract` Class** → Cannot be instantiated directly.
3. **Abstract Method** → Method with signature but no body (`{}`).
4. **Constructors Allowed** → Abstract classes have constructors for super initialization.
5. **Mandatory Overriding** → Subclass must implement all inherited abstract methods.

---

## Topic 9: Java Keywords: `this`, `super`, `static`, `final`

### 1. Definition
Java provides key control keywords: **`this`** (refers to current object), **`super`** (refers to parent object), **`static`** (belongs to class memory, shared across instances), and **`final`** (makes variables, methods, or classes immutable/unmodifiable).

### 2. Key Points
* **`this`**: Accesses current object fields, methods, or invokes constructors (`this()`).
* **`super`**: Accesses parent class fields, overridden methods, or parent constructor (`super()`).
* **`static`**: Allocates memory once at class-loading time; shared by all instances of a class.
* **`final`**: Variable = constant; Method = cannot be overridden; Class = cannot be inherited.
* Neither `this` nor `super` can be used inside a `static` method context.

### 3. Syntax
```java
class Parent {
    int val = 10;
}

class Child extends Parent {
    final int MAX = 100; // Final constant
    static int count = 0; // Static variable

    Child(int val) {
        super(); // Calls parent constructor
        System.out.println(this.val);  // Child/Current context
        System.out.println(super.val); // Parent context
    }
}
```

### 4. Simple Example
```java
class Base {
    int id = 1;
    void display() { System.out.println("Base display"); }
}

class Derived extends Base {
    int id = 2;

    @Override
    void display() {
        super.display(); // Calls Parent method
        System.out.println("Derived display: " + this.id + ", Base id: " + super.id);
    }
}

public class Main {
    public static void main(String[] args) {
        Derived d = new Derived();
        d.display();
    }
}
```
* `super.display()` invokes overridden parent method implementation.
* `this.id` accesses `Derived` class instance variable; `super.id` accesses `Base` class variable.

### 5. How It Works
```text
Child object created on Heap
       ├─ Parent memory layer (super.id = 1)
       └─ Child memory layer (this.id = 2)
       ↓
`super.id` resolves to Parent layer
`this.id` resolves to Child layer
`static` fields stored once in Metaspace (shared globally)
```

### 6. Important Interview Points
* `this()` and `super()` call constructor chaining and MUST be the 1st statement in constructor.
* You CANNOT use both `this()` and `super()` in the exact same constructor body.
* `static` methods cannot access non-static instance variables directly because there is no `this` context.
* A `final` parameter in a method cannot have its value reassigned within the method.
* Blank `final` variables must be initialized inside constructor body before object creation ends.

### 7. Common Differences
| Keyword | Variable Level | Method Level | Class Level |
| :--- | :--- | :--- | :--- |
| **`static`** | Shared single copy across all instances | Can call without object reference | Inner static nested classes |
| **`final`** | Constant value (cannot reassign) | Prevents method overriding | Prevents class inheritance |

### 8. Real-World Analogy
**`static`** is like a community park bench shared by all residents; **`final`** is a permanent tattoo that cannot be changed once created.

### 9. 5-Mark Answer
**Java Keywords: this, super, static, final**

**Definition:** Core Java modifiers control access, scoping, and immutability: `this` (current instance), `super` (parent instance), `static` (class-level shared), and `final` (constant/unmodifiable).

**Key Points:**
1. **`this`**: Disambiguates fields and chains constructors in same class.
2. **`super`**: Calls parent constructors and overridden parent methods.
3. **`static`**: Memory allocated once per class in Metaspace; shared by all instances.
4. **`final`**: Prevents variable reassignment, method overriding, and class extension.
5. `this` and `super` cannot be referenced within static contexts.

**Example:**
```java
class Test {
    static int x = 10;
    final int Y = 20;
    void show() { System.out.println(this.Y); }
}
```

### Quick Revision
1. **`this`** → Reference to current object instance.
2. **`super`** → Reference to parent object instance.
3. **`static`** → Class-level member, single shared copy.
4. **`final` Variable** → Unmodifiable constant value.
5. **`final` Class/Method** → Cannot be extended / overridden.

---

# UNIT III: Packages, Interfaces, Strings & Exception Handling

---

## Topic 10: Packages & Access Modifiers

### 1. Definition
A **Package** is a namespace mechanism that organizes related classes and interfaces into groups to prevent naming collisions. **Access Modifiers** (`private`, default, `protected`, `public`) control the visibility and scope of classes, fields, constructors, and methods.

### 2. Key Points
* Packages prevent class naming conflicts (e.g., `java.util.Date` vs `java.sql.Date`).
* Declared at the top of a file using `package package_name;`.
* **`private`**: Visible ONLY within the declared class.
* **Default (no keyword)**: Visible within the SAME package only (Package-private).
* **`protected`**: Visible in the SAME package AND in child classes across different packages via inheritance.
* **`public`**: Accessible from ANY class in ANY package.

### 3. Syntax
```java
package com.company.project; // Package declaration

import java.util.ArrayList;   // Importing specific class

public class Component {
    private int hiddenVar;     // Only inside this class
    int defaultVar;            // Package-private
    protected int protectedVar;// Package + Subclasses
    public int publicVar;      // Everywhere
}
```

### 4. Simple Example
```java
// File: mypack/Parent.java
package mypack;

public class Parent {
    protected void displayProtected() {
        System.out.println("Protected method in mypack");
    }
}

// File: otherpack/Child.java
package otherpack;
import mypack.Parent;

public class Child extends Parent {
    public static void main(String[] args) {
        Child c = new Child();
        c.displayProtected(); // Accessible via inheritance across packages
    }
}
```
* `protected displayProtected()` is accessible in `otherpack` specifically because `Child` extends `Parent`.

### 5. How It Works
```text
Source File declared with `package com.app;`
       ↓
Java Compiler creates folder structure `/com/app/Class.class`
       ↓
JVM Access Control checks visibility level (`private` < `default` < `protected` < `public`)
       ↓
Allowed -> Call proceeds | Restricted -> Compile-Time Access Error
```

### 6. Important Interview Points
* Outer classes can ONLY be declared `public` or default (package-private); they CANNOT be `private` or `protected`.
* Package names follow reverse domain naming conventions (e.g., `com.company.module`).
* `java.lang` package is automatically imported by default in every Java source file.
* `protected` members can be accessed outside the package ONLY through inheritance reference.
* Star import (`import java.util.*;`) imports all public classes in that package, but NOT sub-packages.

### 7. Common Differences
| Access Modifier | Same Class | Same Package | Subclass (Diff Package) | World (Diff Package) |
| :--- | :---: | :---: | :---: | :---: |
| **`private`** | Yes | No | No | No |
| **Default** | Yes | Yes | No | No |
| **`protected`** | Yes | Yes | Yes (via inheritance) | No |
| **`public`** | Yes | Yes | Yes | Yes |

### 8. Real-World Analogy
Access modifiers are like security levels in an office building: **`private`** = your personal desk drawer, **default** = department floor, **`protected`** = company branch members, **`public`** = main lobby open to everyone.

### 9. 5-Mark Answer
**Packages & Access Modifiers**

**Definition:** Packages group related classes to prevent name clashes. Access modifiers (`private`, default, `protected`, `public`) define the visibility scope of classes and members.

**Key Points:**
1. Packages provide namespace management and access control.
2. `private`: Accessible only within the same class.
3. Default (Package-private): Accessible only within the same package.
4. `protected`: Accessible within package and child classes in different packages.
5. `public`: Accessible from any class in any package.

**Example:**
```java
package com.demo;
public class Test {
    private int secret = 1;
    public int open = 2;
}
```

### Quick Revision
1. **Package** → Organizes classes & prevents name conflicts.
2. **`private`** → Same class only.
3. **Default** → Same package only.
4. **`protected`** → Same package + subclasses across packages.
5. **`public`** → Accessible everywhere.

---

## Topic 11: Interfaces (Default & Static Methods, Multiple Interfaces)

### 1. Definition
An **Interface** is a blueprint of a class in Java that specifies what a class must do, but not how. It achieves 100% abstraction (prior to Java 8) and enables **Multiple Inheritance** using the `implements` keyword.

### 2. Key Points
* Declared using the `interface` keyword.
* All variables declared in an interface are implicitly `public static final` (constants).
* All methods (prior to Java 8) are implicitly `public abstract`.
* Java 8 added **Default Methods** (with body) and **Static Methods** in interfaces.
* Java 9 added **Private Methods** inside interfaces to encapsulate helper logic.

### 3. Syntax
```java
interface Printable {
    int MIN = 1; // public static final

    void print(); // public abstract

    // Java 8 Default Method
    default void msg() {
        System.out.println("Default message");
    }

    // Java 8 Static Method
    static void info() {
        System.out.println("Static interface info");
    }
}
```

### 4. Simple Example
```java
interface Printable {
    void print();
}

interface Showable {
    void show();
}

// Multiple Interface Implementation
class Document implements Printable, Showable {
    @Override
    public void print() {
        System.out.println("Printing document");
    }

    @Override
    public void show() {
        System.out.println("Showing document preview");
    }
}

public class Main {
    public static void main(String[] args) {
        Document doc = new Document();
        doc.print();
        doc.show();
    }
}
```
* `Document implements Printable, Showable` achieves multiple inheritance of behavior.
* Overridden methods in `Document` MUST be declared `public` (cannot reduce interface method visibility).

### 5. How It Works
```text
Interface declares abstract contracts (`print()`, `show()`)
       ↓
`Document` class implements interfaces and defines public concrete bodies
       ↓
Instantiate `new Document()`
       ↓
JVM verifies all interface abstract methods are implemented -> Executes methods
```

### 6. Important Interview Points
* A class can implement multiple interfaces (`class A implements B, C`), solving class multiple-inheritance limits.
* Default methods were introduced in Java 8 to achieve backward compatibility without breaking implementing classes.
* If two interfaces have the same default method signature, the implementing class MUST override it to resolve diamond problem collision.
* Interfaces CANNOT have constructors and CANNOT be instantiated directly.
* Interface static methods are NOT inherited by implementing classes; called via interface name (`Printable.info()`).

### 7. Common Differences
| Aspect | Abstract Class | Interface |
| :--- | :--- | :--- |
| **Inheritance** | Class can extend only ONE abstract class | Class can implement MULTIPLE interfaces |
| **Variables** | Can have final, non-final, static, instance vars | Fields are implicitly `public static final` ONLY |
| **Constructors** | Can have constructors | CANNOT have constructors |
| **Methods** | Can have abstract & concrete methods | Abstract, `default`, `static`, and `private` methods |

### 8. Real-World Analogy
A **Power Socket Interface** defines a standardized shape specification (contract). Any appliance (TV, Laptop charger) implementing the plug interface can draw power.

### 9. 5-Mark Answer
**Interfaces (Default & Static Methods)**

**Definition:** An interface is a reference type contract defining abstract behaviors that implementing classes must fulfill, supporting multiple inheritance in Java.

**Key Points:**
1. Declared using `interface`; implemented using `implements`.
2. Variables are implicitly `public static final`.
3. Java 8 introduced `default` (overridable) and `static` (utility) methods with bodies.
4. Enables multiple inheritance by allowing a class to implement multiple interfaces.
5. Implementing class methods must be declared `public`.

**Example:**
```java
interface A { void display(); }
interface B { void show(); }
class C implements A, B {
    public void display() {}
    public void show() {}
}
```

### Quick Revision
1. **Interface** → Abstract contract defining behavioral specifications.
2. **`public static final`** → All interface variables are constants.
3. **Multiple Implementation** → `class X implements A, B`.
4. **Default Methods (Java 8)** → Methods in interface with default body (`default`).
5. **No Constructors** → Interfaces cannot be instantiated or have constructors.

---

## Topic 12: String Immutability, String Pool, `StringBuilder` vs `StringBuffer`

### 1. Definition
In Java, **Strings are immutable**, meaning once a `String` object is created on the heap, its character sequence cannot be altered. Java uses the **String Constant Pool (SCP)** to optimize memory by sharing identical string literals. `StringBuilder` and `StringBuffer` represent mutable string alternatives.

### 2. Key Points
* `String` objects are stored in the **String Constant Pool (SCP)** inside heap memory.
* Modifying a `String` creates a brand new `String` object on the heap rather than altering original data.
* **`StringBuilder`**: Mutable, fast, but **NOT thread-safe** (unsynchronized).
* **`StringBuffer`**: Mutable, thread-safe (**synchronized**), but slower due to locking overhead.
* String immutability ensures security (passwords, URLs), thread-safety, and hashCode caching.

### 3. Syntax
```java
// Literal creation (SCP)
String s1 = "Java";

// Object creation via 'new' (Heap + SCP)
String s2 = new String("Java");

// Mutable alternatives
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // Modifies original object in-place
```

### 4. Simple Example
```java
public class Main {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello"; // Points to SAME object in SCP

        System.out.println(str1 == str2); // true (Same SCP Reference)

        str1.concat(" World"); // Immutability test
        System.out.println(str1); // Output: "Hello" (Original unchanged)

        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World");
        System.out.println(sb); // Output: "Hello World" (Mutable change)
    }
}
```
* `str1.concat(" World")` creates a new String, but since it's not reassigned, `str1` remains `"Hello"`.
* `str1 == str2` evaluates to `true` because string literals are pooled in SCP.

### 5. How It Works
```text
Literal `String s1 = "Hello"` declared
       ↓
JVM searches String Constant Pool (SCP) for "Hello"
       ↓
Found? Reuses existing reference | Not Found? Creates new instance in SCP
       ↓
`new String("Hello")` creates 2 objects: 1 on main Heap, 1 in SCP if missing
```

### 6. Important Interview Points
* `String` class is declared `final`, so it cannot be extended/inherited.
* `String` overrides `equals()` to compare characters; `==` compares memory addresses.
* Calling `.intern()` on a String object returns its canonical reference from the SCP.
* Immutability allows Java Strings to safely cache their `hashCode` value (crucial for `HashMap` keys).
* String concat (`+`) in a loop creates many temporary objects; use `StringBuilder` inside loops.

### 7. Common Differences
| Aspect | String | StringBuilder | StringBuffer |
| :--- | :--- | :--- | :--- |
| **Mutability** | Immutable (Unchangeable) | Mutable (Modifiable in-place) | Mutable (Modifiable in-place) |
| **Thread Safety** | Thread-safe (Immutable) | **Not Thread-Safe** | **Thread-Safe** (Synchronized) |
| **Speed/Performance** | Slow for frequent modifications | **Fastest** | Slower than StringBuilder |
| **Storage Location** | String Constant Pool / Heap | Heap memory | Heap memory |

### 8. Real-World Analogy
**`String`** is a printed book page (unmodifiable once printed). **`StringBuilder`** is a dry-erase whiteboard where text can be erased and updated continuously.

### 9. 5-Mark Answer
**String Immutability & Pool (StringBuilder vs StringBuffer)**

**Definition:** Java Strings are immutable object sequences stored in the String Constant Pool (SCP). `StringBuilder` and `StringBuffer` provide mutable string handling.

**Key Points:**
1. Immutability: String values cannot be modified once created.
2. SCP (String Constant Pool): Reuses string literal memory addresses.
3. Security & Hash Caching: Immutability enables security (URLs, DB links) and Hash map efficiency.
4. `StringBuilder`: Mutable and fast; used in single-threaded environments.
5. `StringBuffer`: Mutable and thread-safe (synchronized); used in multi-threaded code.

**Example:**
```java
String s = "Java";
s.concat(" 8"); // s remains "Java"
StringBuilder sb = new StringBuilder("Java");
sb.append(" 8"); // sb becomes "Java 8"
```

### Quick Revision
1. **String Immutability** → Content cannot be modified after creation.
2. **SCP** → String Constant Pool reuses literal references in heap.
3. **`StringBuilder`** → Mutable, fast, non-synchronized (Single-threaded).
4. **`StringBuffer`** → Mutable, thread-safe, synchronized (Multi-threaded).
5. **`==` vs `.equals()`** → `==` compares addresses; `.equals()` compares content.

---

## Topic 13: Exception Handling (Checked vs Unchecked, `try-catch-finally`, `throw`/`throws`, Custom Exceptions, Try-with-resources)

### 1. Definition
**Exception Handling** is a mechanism in Java to manage runtime errors, maintaining normal application execution flow. The hierarchy roots at `Throwable`, splitting into `Error` (irrecoverable system issues) and `Exception` (recoverable program conditions).

### 2. Key Points
* **Checked Exceptions**: Checked at compile-time (e.g., `IOException`, `SQLException`). Must be caught or declared.
* **Unchecked Exceptions**: Occur at runtime (e.g., `NullPointerException`, `ArithmeticException`). Subclasses of `RuntimeException`.
* **`try-catch-finally`**: `try` contains risky code, `catch` handles exception, `finally` executes ALWAYS (for cleanup).
* **`throw` vs `throws`**: `throw` explicitly triggers an exception instance; `throws` declares potential exceptions in method signatures.
* **Try-with-resources** (Java 7+): Automatically closes resources implementing `AutoCloseable`.

### 3. Syntax
```java
// Try-with-resources
try (FileReader fr = new FileReader("file.txt")) {
    // Risky code
} catch (IOException e) {
    // Exception Handler
} finally {
    // Always executes
}

// Method declaring exception
void readFile() throws IOException {
    throw new IOException("File missing");
}
```

### 4. Simple Example
```java
// Custom Unchecked Exception
class InvalidAgeException extends RuntimeException {
    InvalidAgeException(String msg) {
        super(msg);
    }
}

public class Main {
    static void validateAge(int age) {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or above!"); // Explicit throw
        }
        System.out.println("Welcome to voting!");
    }

    public static void main(String[] args) {
        try {
            validateAge(15);
        } catch (InvalidAgeException e) {
            System.out.println("Caught: " + e.getMessage());
        } finally {
            System.out.println("Validation Process Complete.");
        }
    }
}
```
* `throw new InvalidAgeException(...)` creates and fires custom exception object.
* `finally` block runs regardless of whether exception was thrown or caught.

### 5. How It Works
```text
Runtime Error / `throw` triggered inside `try` block
       ↓
Normal execution halts -> Exception object created on Heap
       ↓
JVM searches matching `catch` block parameter type down the call stack
       ↓
Matching catch found -> Handler executes -> `finally` block executes
```

### 6. Important Interview Points
* `finally` block will execute EVEN IF a `return` statement is present inside `try` or `catch` blocks.
* The ONLY scenario where `finally` will NOT execute is if `System.exit(0)` is called or JVM crashes.
* Multi-catch block (`catch(IOException | SQLException e)`) handles multiple exceptions concisely.
* Custom checked exceptions extend `Exception`; custom unchecked exceptions extend `RuntimeException`.
* Exception Propagation: Unchecked exceptions propagate automatically up the call stack if uncaught.

### 7. Common Differences
| Aspect | Checked Exception | Unchecked Exception |
| :--- | :--- | :--- |
| **Check Time** | Checked at Compile Time by compiler | Checked at Runtime during execution |
| **Inheritance** | Directly extends `java.lang.Exception` | Extends `java.lang.RuntimeException` |
| **Handling** | Mandatory to catch or declare via `throws` | Optional to catch or declare |
| **Examples** | `IOException`, `ClassNotFoundException` | `NullPointerException`, `ArrayIndexOutOfBoundsException` |

### 8. Real-World Analogy
**Try-Catch** is like wearing a safety helmet while riding a motorcycle; if an accident (exception) occurs, the helmet catches the impact so you can continue living safely.

### 9. 5-Mark Answer
**Exception Handling in Java**

**Definition:** Exception handling manages runtime errors using structured control keywords (`try`, `catch`, `finally`, `throw`, `throws`) to prevent application crashes.

**Key Points:**
1. **Checked Exceptions:** Compile-time validated (`IOException`); mandatory handling.
2. **Unchecked Exceptions:** Runtime occurrences (`NullPointerException`); extend `RuntimeException`.
3. **`try-catch-finally`**: `try` holds risky code, `catch` handles errors, `finally` always executes.
4. **`throw` vs `throws`**: `throw` executes an exception object; `throws` declares signatures.
5. **Try-With-Resources**: Auto-closes resources implementing `AutoCloseable`.

**Example:**
```java
try {
    int res = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
} finally {
    System.out.println("Cleaned up");
}
```

### Quick Revision
1. **Checked Exception** → Checked at compile-time (`IOException`).
2. **Unchecked Exception** → Occurs at runtime (`RuntimeException` subclasses).
3. **`finally` Block** → Always executes (except `System.exit(0)`).
4. **`throw` vs `throws`** → `throw` fires exception; `throws` declares method exceptions.
5. **Try-with-resources** → Auto-closes resources automatically.

---

# UNIT IV: Generics, Collections & Java Utility Classes

---

## Topic 14: Wrapper Classes, Memory Management (Heap, Stack, Garbage Collection)

### 1. Definition
**Wrapper Classes** wrap Java primitive data types into Object form (`int` -> `Integer`). **Memory Management** organizes JVM memory into **Stack** (method execution frames & primitive variables) and **Heap** (objects & instances), managed by the automated **Garbage Collector (GC)**.

### 2. Key Points
* 8 Wrapper classes: `Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`, `Character`, `Boolean`.
* **Autoboxing**: Automatic primitive-to-wrapper conversion (`Integer a = 10;`).
* **Unboxing**: Automatic wrapper-to-primitive conversion (`int x = a;`).
* **Stack Memory**: Stores local variables and active method stack frames (LIFO execution, fast).
* **Heap Memory**: Stores all instantiated Objects and Instance variables globally; cleaned by Garbage Collection.

### 3. Syntax
```java
// Autoboxing & Unboxing
Integer wrapperObj = 100; // Autoboxing
int primitiveVal = wrapperObj; // Unboxing

// Manual Garbage Collection request (Hint to JVM)
System.gc();
```

### 4. Simple Example
```java
public class Main {
    public static void main(String[] args) {
        // Stack stores reference 'list', Heap stores ArrayList object
        java.util.List<Integer> numbers = new java.util.ArrayList<>();
        
        numbers.add(5); // Autoboxing: primitive 5 converted to Integer.valueOf(5)
        int num = numbers.get(0); // Unboxing: Integer converted to primitive int

        System.out.println("Retrieved: " + num);
    }
}
```
* `numbers.add(5)` automatically boxes primitive `5` into `Integer` object to store in generic List.
* Stack holds local reference variable `numbers`; Heap holds the actual `ArrayList` object.

### 5. How It Works (Garbage Collection)
```text
Object created on Heap (`new Student()`)
       ↓
Reference reassigned to null (`s1 = null`) -> Object loses all reachable references
       ↓
Garbage Collector identifies unreachable object during GC Sweep phase
       ↓
GC reclaims heap memory occupied by unreachable object
```

### 6. Important Interview Points
* Wrapper object references can be `null`, which can cause `NullPointerException` during unboxing.
* Java caches Wrapper objects for small values (e.g., `Integer` values between -128 and 127 in Integer Cache).
* Objects on Heap are eligible for Garbage Collection when they have ZERO active reference paths.
* `System.gc()` is a request/hint to the JVM; it does NOT guarantee immediate GC execution.
* Stack memory overflow causes `StackOverflowError`; Heap memory depletion causes `OutOfMemoryError`.

### 7. Common Differences
| Aspect | Stack Memory | Heap Memory |
| :--- | :--- | :--- |
| **Content** | Local variables & method call frames | All instantiated Objects & instance variables |
| **Lifetime** | Short-lived (Destroyed when method finishes) | Long-lived (Persists until Garbage Collected) |
| **Access Speed** | Fast access (LIFO order) | Slower access compared to Stack |
| **Error Thrown** | `java.lang.StackOverflowError` | `java.lang.OutOfMemoryError` |

### 8. Real-World Analogy
**Stack** is your sticky note scratchpad on your desk for immediate task notes; **Heap** is the large storage warehouse down the street, maintained by a cleanup crew (Garbage Collector).

### 9. 5-Mark Answer
**Wrapper Classes & Memory Management**

**Definition:** Wrapper classes convert primitives into objects. JVM memory is split into Stack (local execution) and Heap (object storage), managed automatically by Garbage Collection.

**Key Points:**
1. Wrapper classes enable primitives to be stored in Java Collections.
2. Autoboxing/Unboxing seamlessly converts between primitives and wrappers.
3. Stack Memory manages method execution calls and local primitive variables.
4. Heap Memory stores all created objects; accessible globally.
5. Garbage Collection automatically deallocates unreachable heap memory objects.

**Example:**
```java
Integer obj = 20; // Autoboxing
int val = obj;    // Unboxing
```

### Quick Revision
1. **Wrapper Classes** → Objects representing primitives (`Integer`, `Double`).
2. **Autoboxing / Unboxing** → Automatic conversion between primitives & wrappers.
3. **Stack** → Local variables & method frames (Fast, LIFO).
4. **Heap** → Object instances & fields (GC managed).
5. **Garbage Collector** → Automatically frees unreachable heap memory.

---

## Topic 15: Generics (Classes, Methods, Bounded Types, Wildcards `?`, `? extends`, `? super`)

### 1. Definition
**Generics** allow types (classes, interfaces, and methods) to be parameterized. They provide **Compile-Time Type Safety**, eliminating manual type casting and preventing runtime `ClassCastException`.

### 2. Key Points
* Eliminates the need for explicit type casting (`(String) list.get(0)`).
* Enforces compile-time type checking, catching type mismatches early.
* **Bounded Types**: Restrict type parameters using `<T extends SuperClass>`.
* **Wildcards (`?`)**:
  * Unbounded: `List<?>` (Accepts list of any type).
  * Upper Bounded: `List<? extends Number>` (Accepts `Number` or its subclasses — Read Only / Producer).
  * Lower Bounded: `List<? super Integer>` (Accepts `Integer` or its superclasses — Write Allowed / Consumer).

### 3. Syntax
```java
// Generic Class
class Box<T> {
    private T item;
    public void set(T item) { this.item = item; }
    public T get() { return item; }
}

// Bounded Wildcard Methods
void read(List<? extends Number> list) {} // Upper Bounded
void write(List<? super Integer> list) {} // Lower Bounded
```

### 4. Simple Example
```java
class Storage<T> {
    private T item;

    public void store(T item) {
        this.item = item;
    }

    public T retrieve() {
        return item;
    }
}

public class Main {
    public static void main(String[] args) {
        Storage<String> stringStore = new Storage<>();
        stringStore.store("Generics in Java");
        // stringStore.store(100); // Compile Error: Prevents invalid type!

        String msg = stringStore.retrieve(); // No manual casting required
        System.out.println(msg);
    }
}
```
* `Storage<T>` works with any specified object type safely.
* Type safety stops invalid types (`100`) at compile time.

### 5. How It Works (Type Erasure)
```text
Generic code written: `List<String> list = new ArrayList<>()`
       ↓
Java Compiler checks type safety rules at Compile Time
       ↓
Type Erasure removes type parameters during compilation (`List list`)
       ↓
Bytecode generated with explicit casts inserted automatically for backward compatibility
```

### 6. Important Interview Points
* **Type Erasure**: Generics exist ONLY at compile time; type parameters are erased to `Object` (or bound class) in bytecode.
* You CANNOT instantiate generic types with primitives directly (`List<int>` is INVALID; use `List<Integer>`).
* You CANNOT create instances of type parameters directly (`new T()` is INVALID).
* PECS Rule: **Producer Extends, Consumer Super** (`extends` when reading data out, `super` when writing data in).
* Generic methods can be declared inside non-generic classes (`public <E> void printArray(E[] input)`).

### 7. Common Differences
| Aspect | `? extends T` (Upper Bound) | `? super T` (Lower Bound) |
| :--- | :--- | :--- |
| **Meaning** | Accepts `T` or any child class of `T` | Accepts `T` or any parent class of `T` |
| **PECS Role** | Producer (Use when READING elements) | Consumer (Use when WRITING elements) |
| **Modification** | Cannot add elements to list (except `null`) | Can add elements of type `T` safely |

### 8. Real-World Analogy
A **Shipping Container marked "GLASS ONLY"** prevents workers from loading heavy iron machinery into it at the dock (Compile-time type check).

### 9. 5-Mark Answer
**Generics in Java**

**Definition:** Generics parameterize types for classes, interfaces, and methods, enabling compile-time type safety and eliminating runtime type casting errors.

**Key Points:**
1. Ensures compile-time type safety; prevents `ClassCastException`.
2. Removes requirement for manual type casting.
3. Bounded types (`<T extends Number>`) restrict acceptable parameter types.
4. Wildcards (`?`): `? extends T` (Upper Bound / Read) and `? super T` (Lower Bound / Write).
5. Type Erasure removes generic type info during compilation for JVM backward compatibility.

**Example:**
```java
List<String> names = new ArrayList<>();
names.add("Alice");
String name = names.get(0); // Safe compile-time check
```

### Quick Revision
1. **Generics** → Parameterized types ensuring compile-time type safety.
2. **Type Erasure** → Generics compile away into raw types in bytecode.
3. **Primitives Forbidden** → Use wrapper classes (`List<Integer>`), not primitives.
4. **`? extends T`** → Upper bounded wildcard (Read-only / Producer).
5. **`? super T`** → Lower bounded wildcard (Writeable / Consumer).

---

## Topic 16: Collections Framework (Hierarchy & Implementations)

### 1. Definition
The **Java Collections Framework (JCF)** is a unified architecture storing and manipulating groups of objects. It provides interfaces (`List`, `Set`, `Queue`, `Map`) and concrete data structure implementations (`ArrayList`, `LinkedList`, `HashMap`, etc.).

### 2. Key Points
* **Hierarchy**: `Iterable` -> `Collection` -> (`List`, `Set`, `Queue`). `Map` is a separate key-value hierarchy.
* **`List`**: Ordered collection, allows duplicate elements (`ArrayList`, `LinkedList`, `Vector`).
* **`Set`**: Unordered collection, disallows duplicates (`HashSet`, `LinkedHashSet`, `TreeSet`).
* **`Queue`**: Holds elements prior to processing (FIFO / Priority) (`PriorityQueue`, `ArrayDeque`).
* **`Map`**: Key-Value pairs; keys must be unique (`HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable`).

### 3. Collections Structure Flow
```text
                  Iterable
                     ↓
                 Collection
         ┌───────────┼───────────┐
       List         Set        Queue
       ├── ArrayList ├── HashSet   └── PriorityQueue
       ├── LinkedList├── TreeSet
       └── Vector    └── LinkedHashSet
       
                 Map (Separate Hierarchy)
                 ├── HashMap
                 ├── LinkedHashMap
                 ├── TreeMap
                 └── Hashtable
```

### 4. Simple Example
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        // List Example (Ordered, Duplicates allowed)
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Apple"); // Duplicate

        // Set Example (Unique elements)
        Set<String> set = new HashSet<>(list);

        System.out.println("List: " + list); // Output: [Apple, Apple]
        System.out.println("Set: " + set);   // Output: [Apple]
    }
}
```
* `List` stores and preserves duplicate entry `"Apple"`.
* `HashSet` automatically eliminates duplicate elements.

### 5. Time Complexity Summary
```text
ArrayList:     get(i) -> O(1),      add() -> O(1) avg,   search() -> O(n)
LinkedList:    get(i) -> O(n),      add() -> O(1),       search() -> O(n)
HashSet:       add/contains/remove -> O(1) avg
TreeSet:       add/contains/remove -> O(log n)
HashMap:       get/put/remove      -> O(1) avg
TreeMap:       get/put/remove      -> O(log n)
```

### 6. Important Interview Points
* `ArrayList` is backed by a dynamic array; fast for random access `O(1)`, slow for insertions/deletions in middle `O(n)`.
* `LinkedList` is a doubly-linked list; fast for insertions/deletions `O(1)`, slow for index access `O(n)`.
* `HashSet` uses `HashMap` internally to store elements as keys with a dummy value.
* `HashMap` allows 1 `null` key and multiple `null` values; `Hashtable` and `ConcurrentHashMap` allow NO `null` keys/values.
* `TreeSet` and `TreeMap` store elements in sorted order using `Comparable` or `Comparator` (`O(log n)` operations).

### 7. Common Differences
| Implementation | Ordering | Duplicates Allowed | Null Values Allowed | Performance |
| :--- | :--- | :---: | :---: | :--- |
| **`ArrayList`** | Insertion order preserved | Yes | Yes | Fast `O(1)` random read |
| **`LinkedList`**| Insertion order preserved | Yes | Yes | Fast `O(1)` add/remove |
| **`HashSet`** | No ordering guaranteed | No | 1 Null allowed | Fast `O(1)` lookup |
| **`TreeSet`** | Sorted order (Natural/Custom) | No | **No Nulls** | `O(log n)` sorted search |
| **`HashMap`** | No ordering guaranteed | Keys: No, Values: Yes | 1 Null Key allowed | Fast `O(1)` Key lookup |

### 8. Real-World Analogy
**`List`** is a grocery checklist with repeated items allowed; **`Set`** is a raffle drum containing unique tickets; **`Map`** is a dictionary linking unique Words (Keys) to Definitions (Values).

### 9. 5-Mark Answer
**Java Collections Framework**

**Definition:** The Java Collections Framework provides architecture and interfaces (`List`, `Set`, `Queue`, `Map`) to store, process, and manipulate groups of objects efficiently.

**Key Points:**
1. **`List`**: Ordered, allows duplicate elements (`ArrayList`, `LinkedList`).
2. **`Set`**: Unordered, unique elements only (`HashSet`, `TreeSet`).
3. **`Queue`**: Processing pipeline (`PriorityQueue`, `ArrayDeque`).
4. **`Map`**: Key-Value mapping structure (`HashMap`, `TreeMap`).
5. Complexity: `ArrayList` get is `O(1)`; `HashMap` lookup is `O(1)` average.

**Example:**
```java
Map<Integer, String> map = new HashMap<>();
map.put(101, "Alice");
System.out.println(map.get(101)); // "Alice"
```

### Quick Revision
1. **`List`** → Ordered collection, permits duplicates.
2. **`Set`** → Unordered collection, strictly unique elements.
3. **`Map`** → Key-Value pairs (Unique keys).
4. **`ArrayList` vs `LinkedList`** → `ArrayList` fast read; `LinkedList` fast write/insert.
5. **`HashMap` vs `TreeMap`** → `HashMap` unordered `O(1)`; `TreeMap` sorted `O(log n)`.

---

## Topic 17: Comparable vs Comparator

### 1. Definition
**`Comparable`** and **`Comparator`** are interfaces in Java used to sort custom object elements. `Comparable` defines **Natural Ordering** inside the domain class, whereas `Comparator` defines **Custom/Multiple Ordering** in separate classes or lambda expressions.

### 2. Key Points
* **`Comparable`**: Package `java.lang`. Method: `public int compareTo(T o)`. Modifies original class.
* **`Comparator`**: Package `java.util`. Method: `public int compare(T o1, T o2)`. Does NOT modify original class.
* `compareTo` / `compare` returns:
  * Negative int (`< 0`): Current object is smaller than parameter.
  * Zero (`0`): Both objects are equal.
  * Positive int (`> 0`): Current object is larger than parameter.
* `Collections.sort(list)` uses `Comparable`; `Collections.sort(list, comparator)` uses custom `Comparator`.

### 3. Syntax
```java
// Comparable Implementation (Natural Sort)
class Student implements Comparable<Student> {
    int rollNo;
    public int compareTo(Student s) {
        return this.rollNo - s.rollNo;
    }
}

// Comparator Implementation (Custom Sort)
Comparator<Student> nameComparator = (s1, s2) -> s1.name.compareTo(s2.name);
```

### 4. Simple Example
```java
import java.util.*;

class Student implements Comparable<Student> {
    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    // Comparable: Natural sorting by ID
    @Override
    public int compareTo(Student o) {
        return this.id - o.id;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();
        list.add(new Student(103, "Charlie"));
        list.add(new Student(101, "Alice"));

        // Sort using Comparable (Natural ID Sort)
        Collections.sort(list);

        // Sort using Comparator (Custom Name Sort)
        list.sort(Comparator.comparing(s -> s.name));
    }
}
```
* `compareTo` sorts students naturally by ID integer difference.
* `Comparator.comparing` sorts students dynamically by name without altering `Student` class code.

### 5. How It Works
```text
`Collections.sort(list)` called
       ↓
Sorting algorithm (TimSort) compares elements pairwise
       ↓
Invokes `s1.compareTo(s2)` OR `comparator.compare(s1, s2)`
       ↓
Returns negative/zero/positive -> Algorithm swaps or retains element positions
```

### 6. Important Interview Points
* Use `Comparable` when there is a single logical default natural sort order for a class.
* Use `Comparator` when you need multiple sorting strategies or cannot modify source code of the class.
* Java 8 added fluent methods to `Comparator`: `Comparator.comparing(...).thenComparing(...)`.
* `TreeSet` and `TreeMap` rely on `Comparable`/`Comparator` for maintaining element order.
* Ensure consistency: `(x.compareTo(y) == 0)` should ideally match `(x.equals(y) == true)`.

### 7. Common Differences
| Aspect | `Comparable` | `Comparator` |
| :--- | :--- | :--- |
| **Package** | `java.lang` | `java.util` |
| **Method** | `compareTo(Object o)` | `compare(Object o1, Object o2)` |
| **Sequence** | Single natural sorting sequence | Multiple custom sorting sequences |
| **Class Modification**| Must modify original class source | No modification to original class needed |
| **Invocation** | `Collections.sort(list)` | `Collections.sort(list, comparator)` |

### 8. Real-World Analogy
Sorting students naturally by Roll Number (**`Comparable`** default) vs sorting students dynamically by Height, Weight, or Name for specific events (**`Comparator`** custom strategies).

### 9. 5-Mark Answer
**Comparable vs Comparator**

**Definition:** `Comparable` provides natural single sorting for objects by modifying the class, while `Comparator` provides multiple custom sorting criteria externally.

**Key Points:**
1. `Comparable` interface belongs to `java.lang`; method `compareTo(T o)`.
2. `Comparator` interface belongs to `java.util`; method `compare(T o1, T o2)`.
3. `Comparable` alters the original class definition.
4. `Comparator` defines external sorting logic without modifying the class.
5. Sort methods: `Collections.sort(list)` vs `list.sort(comparator)`.

**Example:**
```java
// Comparator Lambda
List<Integer> nums = Arrays.asList(3, 1, 2);
nums.sort((a, b) -> b - a); // Reverse sort
```

### Quick Revision
1. **`Comparable`** → `java.lang`, `compareTo(o)`, natural single sort order.
2. **`Comparator`** → `java.util`, `compare(o1, o2)`, multiple custom sort orders.
3. **Return Value** → `<0` (Smaller), `0` (Equal), `>0` (Larger).
4. **Class Modification** → `Comparable` modifies class; `Comparator` does not.
5. **Java 8 Comparator** → `Comparator.comparing(Student::getName)`.

---

## Topic 18: Object Class Methods (`equals`, `hashCode`, `toString`, `clone`) & Contract

### 1. Definition
The **`java.lang.Object`** class is the root of the class hierarchy in Java. Key methods include **`toString()`**, **`equals()`**, **`hashCode()`**, **`getClass()`**, and **`clone()`**. The **equals + hashCode contract** dictates that equal objects MUST yield identical hash codes.

### 2. Key Points
* **`toString()`**: Returns String representation of object (default: `ClassName@HexHashCode`).
* **`equals(Object obj)`**: Checks logical equality (default implementation compares memory reference addresses `==`).
* **`hashCode()`**: Returns integer hash code for hashing data structures (`HashMap`, `HashSet`).
* **`equals() + hashCode()` Contract**: If `a.equals(b)` is `true`, then `a.hashCode() == b.hashCode()` MUST be `true`.
* **`clone()`**: Creates a copy of an object (Requires `Cloneable` interface implementation).

### 3. Syntax
```java
class Person implements Cloneable {
    int id;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return id == person.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

### 4. Simple Example
```java
import java.util.Objects;

class Person {
    int id;
    String name;

    Person(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person p = (Person) o;
        return id == p.id && Objects.equals(name, p.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person(1, "Alice");
        Person p2 = new Person(1, "Alice");

        System.out.println(p1.equals(p2)); // true
        System.out.println(p1.hashCode() == p2.hashCode()); // true (Contract satisfied)
    }
}
```
* `equals` checks logical attribute values (`id` and `name`).
* `hashCode` generates matching hash integer using same fields, maintaining the contract for `HashMap` usage.

### 5. How It Works (HashMap Lookup)
```text
`map.put(person1, "Manager")` called
       ↓
Computes `person1.hashCode()` -> Locates Hash Bucket array index
       ↓
When fetching `map.get(person2)`, computes `person2.hashCode()` -> Finds same Bucket
       ↓
Bucket traversed -> Invokes `person1.equals(person2)` -> True -> Value returned!
```

### 6. Important Interview Points
* If you override `equals()`, you MUST override `hashCode()`; failing to do so breaks `HashMap` and `HashSet`.
* If two objects have the SAME `hashCode()`, they are NOT necessarily equal (Hash Collision).
* If two objects are equal via `equals()`, their `hashCode()` values MUST be identical.
* Default `clone()` performs a **Shallow Copy** (copies primitive fields and reference addresses).
* **Deep Copy** manually duplicates nested object instances so cloned copy is independent.

### 7. Common Differences
| Aspect | Shallow Copy | Deep Copy |
| :--- | :--- | :--- |
| **Nested Objects** | Shares nested object references with original | Creates brand new copies of nested objects |
| **Independence** | Changes to nested objects affect both copies | Cloned object is completely independent |
| **Implementation** | Default `Object.clone()` | Custom code duplicating nested object fields |

### 8. Real-World Analogy
**`equals()`** checks if two passports belong to the same person by comparing Passport ID numbers; **`hashCode()`** is the filing cabinet drawer number where the passport file is stored.

### 9. 5-Mark Answer
**Object Class & equals() + hashCode() Contract**

**Definition:** `java.lang.Object` is the root parent class in Java. `equals()` tests logical equality and `hashCode()` generates memory hashing integers for data collections.

**Key Points:**
1. Default `equals()` compares reference memory addresses (`==`).
2. Overridden `equals()` compares state attribute values.
3. **Contract**: If `a.equals(b)` is true, `a.hashCode() == b.hashCode()` MUST be true.
4. `toString()` returns string representation of object.
5. Default `clone()` does shallow copying; deep copy requires manual field duplication.

**Example:**
```java
@Override
public boolean equals(Object o) {
    return (o instanceof Student s) && this.id == s.id;
}
@Override
public int hashCode() { return Objects.hash(id); }
```

### Quick Revision
1. **Root Class** → `java.lang.Object` is parent of all classes.
2. **`equals()`** → Compares logical object state values.
3. **`hashCode()`** → Integer hash code used by `HashMap`/`HashSet`.
4. **Contract Rule** → Equal objects MUST have equal hash codes.
5. **Shallow vs Deep Copy** → Shallow shares nested refs; Deep creates fresh nested copies.

---

# UNIT V: Advanced Java Concepts & Modern OOP Features

---

## Topic 19: Lambda Expressions & Functional Interfaces (`Predicate`, `Function`, `Consumer`, `Supplier`)

### 1. Definition
**Lambda Expressions** (introduced in Java 8) provide concise, anonymous functional syntax `(parameters) -> body`. A **Functional Interface** is an interface containing exactly ONE abstract method, annotated with `@FunctionalInterface`.

### 2. Key Points
* Enables Functional Programming concepts in Java.
* Simplifies anonymous inner class syntax drastically.
* Core Built-in Functional Interfaces (`java.util.function`):
  * **`Predicate<T>`**: Takes input `T`, returns `boolean` (Method: `boolean test(T t)`).
  * **`Function<T, R>`**: Takes input `T`, returns result `R` (Method: `R apply(T t)`).
  * **`Consumer<T>`**: Takes input `T`, returns `void` (Method: `void accept(T t)`).
  * **`Supplier<T>`**: Takes NO input, returns result `T` (Method: `T get()`).

### 3. Syntax
```java
// Lambda Expression syntax
(parameters) -> { body }

// Functional Interface Annotations
@FunctionalInterface
interface Calculator {
    int operate(int a, int b);
}
```

### 4. Simple Example
```java
import java.util.function.*;

public class Main {
    public static void main(String[] args) {
        // Predicate: Checks if number is even
        Predicate<Integer> isEven = num -> num % 2 == 0;
        System.out.println(isEven.test(4)); // true

        // Function: Takes String, returns Length Integer
        Function<String, Integer> getLength = str -> str.length();
        System.out.println(getLength.apply("Java")); // 4

        // Consumer: Takes input, prints it
        Consumer<String> printer = msg -> System.out.println("Printing: " + msg);
        printer.accept("Lambda");

        // Supplier: Takes no input, provides value
        Supplier<Double> getRandom = () -> Math.random();
        System.out.println(getRandom.get());
    }
}
```
* `Predicate`, `Function`, `Consumer`, and `Supplier` replace verbose custom anonymous classes with clean functional logic.

### 5. How It Works
```text
Lambda Expression written `num -> num % 2 == 0`
       ↓
Compiler infers target type to `Predicate<Integer>`
       ↓
`invokedynamic` instruction executed by JVM at runtime
       ↓
Generates lightweight functional call site without creating extra `.class` files
```

### 6. Important Interview Points
* An interface annotated `@FunctionalInterface` will throw a compile error if it has more than 1 abstract method.
* Functional interfaces CAN contain multiple `default` or `static` methods; only 1 abstract method allowed.
* Lambdas can access effectively final local variables from their enclosing scope (Closure concept).
* Method References (`Class::methodName`) offer even cleaner syntax for simple lambdas.
* `Runnable` (`run()`) and `Comparator` (`compare()`) are classic examples of functional interfaces.

### 7. Common Differences
| Functional Interface | Input Argument(s) | Return Type | Primary Purpose / Use Case |
| :--- | :--- | :--- | :--- |
| **`Predicate<T>`** | 1 (`T`) | `boolean` | Filtering / Conditional Evaluation |
| **`Function<T, R>`** | 1 (`T`) | `R` (Result Type) | Transforming / Mapping Input to Output |
| **`Consumer<T>`** | 1 (`T`) | `void` (No Return)| Performing Side Effects / Printing |
| **`Supplier<T>`** | None | `T` (Result Type) | Factory / Data Generation |

### 8. Real-World Analogy
**`Predicate`** is a security guard checking IDs (True/False); **`Function`** is a currency converter converting USD to Euros; **`Consumer`** is a trash shredder processing paper; **`Supplier`** is a vending machine dispensing water.

### 9. 5-Mark Answer
**Lambda Expressions & Functional Interfaces**

**Definition:** Lambda Expressions (`->`) represent anonymous functional methods. A Functional Interface contains exactly one abstract method, annotated with `@FunctionalInterface`.

**Key Points:**
1. Reduces boilerplate code by eliminating anonymous inner classes.
2. `@FunctionalInterface` enforces exactly one abstract method rule.
3. **`Predicate<T>`**: Accepts `T`, returns `boolean` (`test()`).
4. **`Function<T,R>`**: Accepts `T`, returns transformed `R` (`apply()`).
5. **`Consumer<T>`**: Accepts `T`, returns void (`accept()`); **`Supplier<T>`**: Returns `T` (`get()`).

**Example:**
```java
Predicate<String> isEmpty = s -> s.isEmpty();
System.out.println(isEmpty.test("")); // true
```

### Quick Revision
1. **Lambda** → Anonymous function expression `(params) -> expression`.
2. **Functional Interface** → Interface with strictly 1 abstract method.
3. **`Predicate<T>`** → Input `T` -> returns `boolean`.
4. **`Function<T,R>`** → Input `T` -> returns `R`.
5. **`Consumer` & `Supplier`** → `Consumer` accepts (`void`); `Supplier` supplies (`T`).

---

## Topic 20: Stream API (Source, Intermediate Operations, Terminal Operations)

### 1. Definition
The **Stream API** (introduced in Java 8) is a sequence of elements supporting functional-style parallel and sequential processing on collections. Streams do NOT store data; they process data through a pipeline consisting of a **Source**, **Intermediate Operations**, and a **Terminal Operation**.

### 2. Key Points
* Streams do not alter the underlying source collection.
* **Source**: Collection, Array, or I/O channel providing data (`list.stream()`).
* **Intermediate Operations**: Lazy evaluation operations that return a new Stream (`filter`, `map`, `sorted`).
* **Terminal Operations**: Trigger processing and produce a result or side effect (`collect`, `forEach`, `reduce`).
* **Lazy Evaluation**: Intermediate operations execute ONLY when a terminal operation is invoked.

### 3. Stream Pipeline Flow
```text
Collection / Source Data
       ↓
    stream()            (Creates Stream)
       ↓
   filter(...)          (Intermediate Operation - Lazy)
       ↓
    map(...)             (Intermediate Operation - Lazy)
       ↓
   sorted(...)           (Intermediate Operation - Lazy)
       ↓
  collect(...)           (Terminal Operation - Triggers Execution!)
       ↓
Final Result / List / Value
```

### 4. Simple Example
```java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Alexander", "Brian");

        // Pipeline: Filter names starting with 'A' -> Transform to Uppercase -> Collect to List
        List<String> result = names.stream()
                .filter(name -> name.startsWith("A")) // Intermediate
                .map(String::toUpperCase)              // Intermediate
                .sorted()                              // Intermediate
                .collect(Collectors.toList());         // Terminal

        System.out.println(result); // Output: [ALEXANDER, ALICE]
    }
}
```
* `filter()`, `map()`, and `sorted()` set up lazy pipeline rules.
* `collect()` triggers execution flow and produces output list.

### 5. How It Works
```text
`names.stream()` creates Stream pipeline wrapper
       ↓
`filter()` and `map()` attached as chain nodes (No data processed yet!)
       ↓
`collect()` Terminal operation called -> Data flows element-by-element through chain
       ↓
Intermediate operations process elements -> Accumulated into final Result List
```

### 6. Important Interview Points
* Streams are **Single-Use**; once a terminal operation completes, the stream is consumed and cannot be reused.
* **Intermediate vs Terminal**: Intermediate operations return `Stream<T>`; Terminal operations return non-stream results (`List`, `long`, `void`, etc.).
* `parallelStream()` leverages ForkJoinPool to process collection elements concurrently across multi-core CPUs.
* `map()` transforms elements 1-to-1; `flatMap()` flattens nested streams (1-to-many).
* Streams use short-circuit operations like `findFirst()`, `anyMatch()`, and `limit()` to optimize performance.

### 7. Common Differences
| Aspect | Intermediate Operation | Terminal Operation |
| :--- | :--- | :--- |
| **Return Type** | Returns a new `Stream<T>` | Returns result (Collection, Primitive, void) |
| **Execution** | **Lazy** (Does not process until terminal call) | **Eager** (Triggers immediate stream processing) |
| **Multiplicity** | Multiple intermediate operations chained | **Exactly ONE** terminal operation per stream |
| **Examples** | `filter()`, `map()`, `sorted()`, `distinct()` | `collect()`, `forEach()`, `count()`, `reduce()` |

### 8. Real-World Analogy
An **Assembly Line Pipeline**: Raw material comes from Source -> Workers inspect (`filter`) and paint (`map`) components lazily -> Package box operator (`collect`) triggers the conveyor belt to run and deliver finished goods.

### 9. 5-Mark Answer
**Stream API (Pipeline & Operations)**

**Definition:** Stream API processes data collection elements functionally via pipelines comprising Source, Intermediate Operations (lazy), and Terminal Operations (eager execution).

**Key Points:**
1. Streams process collection data without modifying the source collection.
2. **Intermediate Operations**: Lazy transformation returning new Stream (`filter`, `map`).
3. **Terminal Operations**: Triggers pipeline execution producing final output (`collect`, `count`).
4. Lazy Evaluation: Processing occurs only upon reaching terminal method.
5. Streams are single-use and can run in parallel using `parallelStream()`.

**Example:**
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4);
List<Integer> evens = nums.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

### Quick Revision
1. **Stream API** → Functional processing pipeline for collections.
2. **Source** → Collection/Array origin (`list.stream()`).
3. **Intermediate Ops** → `filter()`, `map()`, `sorted()` (Lazy, returns Stream).
4. **Terminal Ops** → `collect()`, `forEach()`, `reduce()` (Eager, consumes Stream).
5. **Single-Use** → Streams cannot be reused after terminal operation runs.

---

## Topic 21: Multithreading (Lifecycle, `Thread` vs `Runnable`, `sleep`, `join`, `synchronized`, Race Condition)

### 1. Definition
**Multithreading** is the concurrent execution of two or more threads to maximize CPU utilization. A **Thread** is the smallest lightweight unit of process execution. A **Race Condition** occurs when multiple threads modify shared resource data simultaneously, mitigated using **Synchronization**.

### 2. Key Points
* **Thread Creation**: Extend `Thread` class OR Implement `Runnable` interface (Preferred).
* **Thread States**: New -> Runnable -> Running -> Blocked/Waiting -> Terminated.
* **`sleep(ms)`**: Pauses current thread execution for specified milliseconds without releasing locks.
* **`join()`**: Causes current thread to wait until target thread finishes execution.
* **`synchronized`**: Restricts method or code block access to ONE thread at a time using object locks.

### 3. Syntax
```java
// Method 1: Runnable Interface (Preferred)
class MyTask implements Runnable {
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

// Synchronization
synchronized void safeIncrement() {
    count++; // Thread-safe operation
}
```

### 4. Simple Example
```java
class Counter {
    private int count = 0;

    // Synchronized to prevent Race Condition
    public synchronized void increment() {
        count++;
    }

    public int getCount() { return count; }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) counter.increment();
        });
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) counter.increment();
        });

        t1.start();
        t2.start();

        t1.join(); // Main thread waits for t1
        t2.join(); // Main thread waits for t2

        System.out.println("Final Count: " + counter.getCount()); // Output: 2000
    }
}
```
* `synchronized` prevents thread race conditions on `count++`.
* `t1.join()` ensures main thread waits until `t1` completes before reading final count.

### 5. Thread Lifecycle Flow
```text
[New] state (`new Thread()`)
       ↓ `.start()`
[Runnable] state (Waiting for CPU Scheduler pick)
       ↓ CPU allocated
[Running] state (`run()` executing)
       ├─ `sleep()` / `wait()` -> [Waiting / Blocked]
       └─ Method finishes -> [Terminated / Dead]
```

### 6. Important Interview Points
* Always implement `Runnable` (or `Callable`) instead of extending `Thread` to preserve inheritance flexibility.
* Call `.start()` to start a thread (allocates new stack frame); calling `.run()` directly just executes it like a normal method on the current stack frame.
* A **Race Condition** occurs when execution results depend on unpredictable thread interleaving timing.
* `synchronized` acquires object monitor lock (intrinsic lock); released automatically upon block exit.
* `volatile` keyword ensures variable updates are written immediately to main memory (visibility guarantee).

### 7. Common Differences
| Aspect | Extending `Thread` Class | Implementing `Runnable` Interface |
| :--- | :--- | :--- |
| **Inheritance** | Cannot extend any other class (`extends Thread`) | Class can still extend another parent class |
| **Reusability** | Tight coupling between code and thread execution | Loose coupling; separates task from execution |
| **Object Sharing**| Each thread creates unique object instance | Multiple threads can share same `Runnable` instance |

### 8. Real-World Analogy
**Multithreading** is like multiple chefs working concurrently in a restaurant kitchen. **Synchronization** is locking the single microwave door while one chef heats food, preventing another chef from messing up the meal.

### 9. 5-Mark Answer
**Multithreading & Synchronization**

**Definition:** Multithreading executes multiple concurrent thread tasks. Synchronization (`synchronized`) prevents race conditions by locking shared resources for one thread at a time.

**Key Points:**
1. Threads can be created via `extends Thread` or `implements Runnable` (preferred).
2. Thread States: New -> Runnable -> Running -> Blocked/Waiting -> Terminated.
3. `.start()` creates a new call stack; `.run()` executes code on current thread.
4. `sleep()` pauses execution; `join()` waits for target thread completion.
5. `synchronized` locks object monitor to guarantee thread safety and prevent race conditions.

**Example:**
```java
Thread t = new Thread(() -> System.out.println("Running"));
t.start();
t.join();
```

### Quick Revision
1. **Multithreading** → Concurrent execution of lightweight threads.
2. **`Runnable` Interface** → Preferred thread creation method.
3. **`.start()` vs `.run()`** → `.start()` creates new stack; `.run()` runs synchronously on caller stack.
4. **`join()` & `sleep()`** → `join()` waits for thread termination; `sleep()` pauses execution.
5. **Race Condition & `synchronized`** → Race condition corrupts shared data; `synchronized` locks access.

---

## Topic 22: Java I/O & Serialization (`Serializable`, `transient`)

### 1. Definition
**Java I/O** (Input/Output) processes reading and writing data across streams. **Serialization** is the mechanism of converting an object's memory state into a byte stream for storage (file/database) or network transmission. **Deserialization** reconstructs the object back from the byte stream.

### 2. Key Points
* **Byte Streams**: Process raw binary 8-bit bytes (`InputStream`, `OutputStream`, `FileInputStream`).
* **Character Streams**: Process text 16-bit Unicode characters (`Reader`, `Writer`, `FileReader`).
* **`Serializable` Interface**: Marker interface (0 methods) required to make an object serializable.
* **`transient` Keyword**: Fields marked `transient` are skipped during serialization (default value saved).
* **`serialVersionUID`**: Unique ID verifying that sender and receiver of serialized object share compatible classes.

### 3. Syntax
```java
// Serializable Object with transient field
class User implements Serializable {
    private static final long serialVersionUID = 1L;
    String username;
    transient String password; // Excluded from serialization!
}

// Serialization Writer
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"));
out.writeObject(userInstance);
```

### 4. Simple Example
```java
import java.io.*;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    String name;
    transient int secretPin; // Will NOT be saved!

    Student(String name, int secretPin) {
        this.name = name;
        this.secretPin = secretPin;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Alice", 9999);

        // Serialization
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(s1);
        } catch (IOException e) { e.printStackTrace(); }

        // Deserialization
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student deserializedStudent = (Student) ois.readObject();
            System.out.println("Name: " + deserializedStudent.name);          // Alice
            System.out.println("Pin: " + deserializedStudent.secretPin);      // 0 (Default restored!)
        } catch (Exception e) { e.printStackTrace(); }
    }
}
```
* `secretPin` is marked `transient`, so it is ignored during serialization and restores as `0` during deserialization.

### 5. Serialization & Deserialization Flow
```text
Memory Object (`Student`)
       ↓
`ObjectOutputStream.writeObject()`
       ↓
Byte Stream generated (Transient fields skipped!)
       ↓ Storage / File (`student.ser`) / Network Transmission
Byte Stream read
       ↓
`ObjectInputStream.readObject()`
       ↓
Object reconstructed on Heap (Transient fields set to default: null/0)
```

### 6. Important Interview Points
* `Serializable` is a **Marker Interface** (has no fields or methods); it simply signals JVM permission to serialize.
* `static` variables are NOT serialized because they belong to class Metaspace, not object heap instance state.
* If a parent class implements `Serializable`, all child subclasses are automatically serializable.
* If a child class implements `Serializable` but parent does NOT, parent must have an accessible no-arg constructor during deserialization.
* Mismatched `serialVersionUID` throws `InvalidClassException` during deserialization.

### 7. Common Differences
| Aspect | Byte Streams | Character Streams |
| :--- | :--- | :--- |
| **Data Unit** | Reads/Writes 8-bit bytes | Reads/Writes 16-bit Unicode characters |
| **Target Data** | Binary files (Images, Audio, PDF, Executables) | Text files (TXT, CSV, HTML, Source code) |
| **Root Classes** | `InputStream` and `OutputStream` | `Reader` and `Writer` |

### 8. Real-World Analogy
**Serialization** is disassembling a furniture table into flat-packed wood boards for shipping in a box; **Deserialization** is assembling the flat-pack boards back into a table in your living room. **`transient`** is leaving fragile accessories out of the shipping box.

### 9. 5-Mark Answer
**Java I/O & Serialization (Serializable & transient)**

**Definition:** Serialization converts an object into a byte stream for storage/transmission; Deserialization reconstructs the object.

**Key Points:**
1. Byte Streams (8-bit) process binary data; Character Streams (16-bit) process text.
2. `Serializable` is a marker interface enabling object byte-stream conversion.
3. `transient` fields are excluded from serialization (restored to default values).
4. `static` fields belong to class memory and are not serialized.
5. `serialVersionUID` ensures compatibility between serialized byte stream and JVM class.

**Example:**
```java
class Account implements Serializable {
    String user;
    transient String pin; // Not saved
}
```

### Quick Revision
1. **Serialization** → Converting object state to byte stream.
2. **Deserialization** → Reconstructing object from byte stream.
3. **`Serializable`** → Marker interface (0 methods) enabling serialization.
4. **`transient`** → Field modifier preventing variable serialization.
5. **Byte vs Character Streams** → Byte Streams (8-bit binary); Character Streams (16-bit text).

---

# FINAL MASTER CHECKLIST SUMMARY

```text
Java Basics
   ↓
Classes & Objects
   ↓
Constructors
   ↓
Encapsulation
   ↓
Inheritance
   ↓
Polymorphism
   ↓
Abstraction
   ↓
Interfaces
   ↓
Packages
   ↓
Strings
   ↓
Exception Handling
   ↓
Wrapper Classes
   ↓
Generics
   ↓
Collections
   ↓
Comparable & Comparator
   ↓
Object Class
   ↓
Memory Management
   ↓
Lambda Expressions
   ↓
Functional Interfaces
   ↓
Stream API
   ↓
Multithreading
   ↓
I/O & Serialization
```
