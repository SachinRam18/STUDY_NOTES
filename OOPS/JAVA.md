# Java OOP — Complete Notes

---

# UNIT 1 — Java Foundations

---

## 1.1 Class and Object

A **class** is a blueprint for creating objects. An **object** is an instance of a class — it has state (fields) and behavior (methods).

```java
class Student {
    String name;   // field
    int age;

    void greet() {  // method
        System.out.println("Hi, I am " + name);
    }
}

Student s = new Student();  // object
s.name = "Alice";
s.greet();  // "Hi, I am Alice"
```

**Memory:**
- Class metadata → Metaspace
- Objects → Heap
- References → Stack

---

## 1.2 Access Modifiers

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|-----------|--------------|----------|------------|
| `private` | ✅ | ❌ | ❌ | ❌ |
| default | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

---

## 1.3 Constructors

A constructor has the same name as the class and no return type. It initializes the object when `new` is called.

```java
class Student {
    String name;
    int age;

    // No-arg constructor
    Student() {
        name = "Unknown";
        age = 0;
    }

    // Parameterized constructor
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

**Rules:**
- If you write no constructor → compiler inserts a default no-arg constructor
- If you write any constructor → compiler does NOT insert a default
- Constructors cannot be `static`, `final`, `abstract`, or `synchronized`
- Constructors cannot be inherited or overridden

---

## 1.4 `this` Keyword

`this` refers to the current object.

```java
class Student {
    String name;

    Student(String name) {
        this.name = name;  // disambiguate from parameter
    }

    void show() {
        System.out.println(this);  // calls toString()
    }
}
```

**`this()` — Constructor Chaining (same class):**

```java
class Student {
    String name;
    int age;

    Student() {
        this("Unknown", 0);  // MUST be first statement
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

---

## 1.5 `static` Keyword

- **Static variable** — shared by all instances (one copy per class)
- **Static method** — belongs to class, not instance. Cannot use `this` or `super`
- **Static block** — runs once when class is first loaded

```java
class Counter {
    static int count = 0;  // shared

    Counter() {
        count++;
    }

    static void showCount() {
        System.out.println("Count: " + count);
    }
}

Counter.showCount();  // no object needed
```

**Static block:**
```java
class Config {
    static String url;
    static {
        url = "jdbc:mysql://localhost/db";  // runs once at class load
    }
}
```

---

## 1.6 Initialization Order

For a single class:
```
1. Static variables + static blocks (textual order) — once per class load
2. Instance variables + instance blocks (textual order) — per object
3. Constructor body
```

With inheritance:
```
1. Parent static → Child static  (once)
2. Parent instance block → Parent constructor
3. Child instance block → Child constructor
```

**Example:**
```java
class A {
    static { System.out.print("A-static "); }
    { System.out.print("A-instance "); }
    A() { System.out.print("A() "); }
}
class B extends A {
    static { System.out.print("B-static "); }
    { System.out.print("B-instance "); }
    B() { System.out.print("B() "); }
}
new B();
// Output: A-static B-static A-instance A() B-instance B()
```

**Trap — static field initialized with `new`:**
```java
class Test {
    static Test t = new Test();       // instance block runs during class loading
    static { System.out.print("static "); }
    { System.out.print("instance "); }
    Test() { System.out.print("constructor "); }
}
// Output: instance constructor static
```

---

## 1.7 `Object` Class

Every class implicitly extends `java.lang.Object`. Key methods:

| Method | Purpose |
|--------|---------|
| `equals(Object o)` | Compares content (override this) |
| `hashCode()` | Returns integer hash for use in HashMaps |
| `toString()` | String representation |
| `getClass()` | Returns runtime class |
| `clone()` | Shallow copy (needs `Cloneable`) |
| `finalize()` | Called before GC (deprecated) |

Default `toString()` returns `ClassName@hashHex`. Always override it.

---

## 1.8 Wrapper Classes and Autoboxing

```java
int x = 5;
Integer a = x;        // autoboxing (int → Integer)
int b = a;            // unboxing (Integer → int)
```

**Integer Cache:** Java caches `Integer` objects for values **-128 to 127**.

```java
Integer a = 127, b = 127;
System.out.println(a == b);   // true (same cached object)

Integer c = 128, d = 128;
System.out.println(c == d);   // false (different heap objects)
System.out.println(c.equals(d)); // true
```

**Unboxing null → NullPointerException:**
```java
Integer x = null;
int y = x;  // NPE!
```

---

# UNIT 2 — OOP Pillars

---

## 2.1 Encapsulation

Encapsulation means hiding an object's data and controlling access through methods.

```java
class BankAccount {
    private double balance;  // hidden

    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }

    public double getBalance() { return balance; }
}
```

Benefits: data validation, hiding implementation details, easier maintenance.

---

## 2.2 Inheritance

A child class inherits fields and methods from a parent class using `extends`.

```java
class Animal {
    String name;
    void eat() { System.out.println(name + " eats"); }
}

class Dog extends Animal {
    void bark() { System.out.println("Woof!"); }
}

Dog d = new Dog();
d.name = "Rex";
d.eat();   // inherited
d.bark();  // own
```

**Types:** Single, Multilevel, Hierarchical. Java does NOT support multiple class inheritance.

**`super` keyword:**
```java
class Child extends Parent {
    Child() {
        super();          // call parent constructor (must be first)
        super.method();   // call parent method
    }
}
```

---

## 2.3 `super` vs `this`

| | `this` | `super` |
|---|--------|--------|
| Refers to | Current object | Parent object |
| Constructor call | `this()` — same class | `super()` — parent class |
| Must be | First statement | First statement |
| Can use both? | ❌ Only one allowed | ❌ Only one allowed |

---

## 2.4 Method Overloading (Compile-time Polymorphism)

Same method name, different parameter list in the same class.

```java
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
}
```

**Overload resolution priority:** Exact match → Widening → Autoboxing → Varargs

```java
// show(5) resolves as:
void show(int x)      // 1st: exact match (chosen)
void show(long x)     // 2nd: widening
void show(Integer x)  // 3rd: autoboxing
void show(int... x)   // 4th: varargs
```

**Key rules:**
- Return type alone cannot distinguish overloads
- Java does NOT do widening then boxing together (e.g., `int` → `Long` fails)
- `null` matches the most specific type: `show(String s)` over `show(Object o)`
- `null` with equally specific types (e.g., `String` and `Integer`) → compile error

---

## 2.5 Method Overriding (Runtime Polymorphism)

Same method signature in child class, providing a different implementation.

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    @Override
    void makeSound() { System.out.println("Woof"); }
}

Animal a = new Dog();
a.makeSound();  // "Woof" — runtime dispatch
```

**Rules:**
- Same name, same parameter list, same or covariant return type
- Access modifier can be widened (e.g., `protected` → `public`), never narrowed
- Cannot override: `private`, `static`, `final` methods
- Checked exceptions: can only throw same, narrower, or none. NOT broader.
- Unchecked exceptions: no restriction

**`@Override`** annotation: compiler confirms you're actually overriding.

---

## 2.6 Overloading vs Overriding

| Aspect | Overloading | Overriding |
|--------|------------|-----------|
| Location | Same class | Subclass |
| Parameters | Different | Same |
| Return type | Can differ | Same or covariant |
| Resolved at | Compile time | Runtime |
| Access modifier | Any | Cannot narrow |
| `static` methods | Can overload | Cannot override (hidden) |

---

## 2.7 Dynamic Method Dispatch

When a parent reference holds a child object, the overridden method of the child runs at runtime.

```java
Animal a = new Dog();  // upcasting
a.makeSound();         // Dog's version runs — runtime dispatch
```

- **Methods:** resolved by actual object type (runtime)
- **Fields:** resolved by reference type (compile time — NOT polymorphic)
- **Static methods:** resolved by reference type (method hiding, not overriding)

```java
class Parent { int x = 10; static void show() { System.out.println("Parent"); } }
class Child extends Parent { int x = 20; static void show() { System.out.println("Child"); } }

Parent p = new Child();
System.out.println(p.x);  // 10 — field, reference type wins
p.show();                  // "Parent" — static, reference type wins
```

---

## 2.8 Upcasting and Downcasting

**Upcasting** (safe, implicit): child → parent reference
```java
Animal a = new Dog();  // OK, no cast needed
```

**Downcasting** (unsafe, explicit): parent → child reference
```java
Animal a = new Dog();
Dog d = (Dog) a;      // OK — actual object is Dog
Cat c = (Cat) a;      // ClassCastException at runtime!
```

Always use `instanceof` before downcasting:
```java
if (a instanceof Dog d) {   // Java 16+ pattern matching
    d.bark();
}
```

---

## 2.9 Polymorphism

Polymorphism means the same call behaves differently depending on the actual object.

```java
Animal[] animals = { new Dog(), new Cat(), new Bird() };
for (Animal a : animals) {
    a.makeSound();  // each calls their own version
}
```

- **Compile-time:** Method overloading — resolved by compiler
- **Runtime:** Method overriding + dynamic dispatch — resolved by JVM

---

## 2.10 Static and Final Methods

**Static method hiding (not overriding):**
```java
class Parent { static void show() { System.out.println("Parent"); } }
class Child extends Parent { static void show() { System.out.println("Child"); } }

Parent p = new Child();
p.show();  // "Parent" — reference type, not object type
```

**Final method** — cannot be overridden:
```java
class A { final void show() {} }
class B extends A {
    // void show() {}  // COMPILE ERROR
}
```

---

## 2.11 Encapsulation: Getters and Setters

```java
class Person {
    private String name;
    private int age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) {
        if (age > 0) this.age = age;
    }
}
```

---

## Unit 2 — Key Output Questions

**Q1: Field hiding vs method overriding**
```java
class Parent { int x = 10; void show() { System.out.println("Parent"); } }
class Child extends Parent { int x = 20; void show() { System.out.println("Child"); } }

Parent p = new Child();
System.out.println(p.x);  // 10 (field = reference type)
p.show();                  // Child (method = object type)
```

**Q2: Constructor chaining order**
```java
class A { A() { System.out.print("A "); } }
class B extends A { B() { System.out.print("B "); } }
new B();  // A B
```

**Q3: Static method hiding**
```java
class A { static void m() { System.out.println("A"); } }
class B extends A { static void m() { System.out.println("B"); } }
A obj = new B();
obj.m();  // A — static, reference type decides
```

**Q4: Overload resolution**
```java
// Given: show(int), show(Integer), show(long), show(int...)
new Test().show(5);  // "int" — exact match wins
```
```java
// Given: show(Integer), show(long), show(int...)
new Test().show(5);  // "long" — widening before boxing
```

**Q5: Calling overridable method in constructor**
```java
class Parent { Parent() { show(); } void show() {} }
class Child extends Parent { int x = 10; void show() { System.out.println(x); } }
new Child();  // prints 0 — x not initialized yet during parent constructor
```

---

# UNIT 3 — Abstraction, Interfaces, and Design

---

## 3.1 Abstraction

Abstraction means showing what an object does, not how it does it.

- Achieved via **abstract classes** and **interfaces**

---

## 3.2 Abstract Class

```java
abstract class Shape {
    String color;

    Shape(String color) { this.color = color; }

    abstract double area();  // no body

    void describe() {        // concrete method
        System.out.println("Area = " + area());
    }
}

class Circle extends Shape {
    double radius;
    Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    double area() { return Math.PI * radius * radius; }
}
```

**Rules:**
- Cannot instantiate (`new Shape()` → error)
- Can have constructors (called via `super()`)
- Can have abstract AND concrete methods
- Can have any access modifiers, instance fields, static methods
- A subclass MUST implement all abstract methods, or itself be abstract
- Abstract + final = compile error. Abstract + static = compile error.

---

## 3.3 Interface

An interface defines a contract — a set of methods that implementing classes must provide.

```java
interface Drawable {
    void draw();                           // abstract (implicitly public abstract)
    default void reset() {                 // default method (Java 8+)
        System.out.println("Resetting");
    }
    static void info() {                   // static method (Java 8+)
        System.out.println("Interface");
    }
}

class Circle implements Drawable {
    public void draw() { System.out.println("Circle drawn"); }
}
```

**Interface fields:** all implicitly `public static final` (constants).

**Interface static methods** are NOT inherited — call them on the interface name:
```java
Drawable.info();   // OK
// Circle.info();  // COMPILE ERROR
```

**Private methods in interfaces (Java 9+):** Used to share code between default methods without exposing to implementers.

---

## 3.4 Default Method Conflict (Diamond Problem)

If two interfaces have the same default method, the implementing class MUST override it:

```java
interface A { default void show() { System.out.println("A"); } }
interface B { default void show() { System.out.println("B"); } }

class C implements A, B {
    @Override
    public void show() {
        A.super.show();  // explicitly choose A's version
    }
}
```

**Resolution rules:**
1. Class method wins over interface default
2. More specific interface wins (subtype over supertype)
3. If still ambiguous → must override in class

```java
interface A { default void show() { System.out.println("A"); } }
class B { public void show() { System.out.println("B"); } }
class C extends B implements A {}
new C().show();  // "B" — class always wins
```

---

## 3.5 Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Instantiation | ❌ | ❌ |
| Constructors | ✅ | ❌ |
| Instance fields | ✅ | ❌ (only `public static final`) |
| Concrete methods | ✅ | ✅ (default, Java 8+) |
| Static methods | ✅ (inherited) | ✅ (NOT inherited) |
| Multiple inheritance | ❌ | ✅ |
| Access modifiers | Any | `public` (methods) |

**When to use:**
- **Abstract class**: shared state (fields), IS-A relationship, common base implementation
- **Interface**: CAN-DO capabilities, multiple inheritance, define a contract

---

## 3.6 Multiple Inheritance via Interfaces

```java
interface Flyable { void fly(); }
interface Swimmable { void swim(); }

class Duck implements Flyable, Swimmable {
    public void fly() { System.out.println("Duck flies"); }
    public void swim() { System.out.println("Duck swims"); }
}
```

Why classes can't extend multiple classes: multiple state inheritance creates ambiguity. Interfaces (traditionally) have no state.

---

## 3.7 Composition vs Inheritance

**Inheritance (IS-A):**
```java
class Car extends Engine {}  // semantically wrong — Car IS-A Engine?
```

**Composition (HAS-A) — preferred:**
```java
class Car {
    private Engine engine;  // Car HAS-A Engine

    Car(Engine engine) { this.engine = engine; }

    void drive() { engine.start(); }
}
```

| Aspect | Inheritance | Composition |
|--------|------------|-------------|
| Coupling | Tight | Loose |
| Flexibility | Fixed at compile time | Can change at runtime |
| Encapsulation | Breaks (child sees protected) | Preserved |

**Rule:** "Favor composition over inheritance" — Effective Java.

---

## 3.8 Association, Aggregation, Composition

- **Association** — two independent classes use each other
- **Aggregation (Weak HAS-A)** — container holds a reference, but contained object can outlive it
  - `Department ◇──── Employee` (hollow diamond)
- **Composition (Strong HAS-A)** — contained object cannot exist without container
  - `House ◆──── Room` (filled diamond)

```java
// Aggregation
class Department {
    List<Employee> employees;  // employees exist independently
}

// Composition
class House {
    private List<Room> rooms;
    House() {
        rooms = new ArrayList<>();
        rooms.add(new Room("Bedroom"));  // rooms created inside House
    }
}
```

---

## 3.9 `final` Keyword

**`final` variable** — cannot be reassigned:
```java
final int MAX = 100;
// MAX = 200;  // COMPILE ERROR
```

**`final` reference vs immutable object:**
```java
final Student s = new Student("Alice");
s.name = "Bob";         // OK — can modify object's state
// s = new Student();   // COMPILE ERROR — cannot reassign reference
```

**`final` method** — cannot be overridden.
**`final` class** — cannot be subclassed (e.g., `String`, `Integer`, `Math`).

---

## 3.10 Nested Classes

### Static Nested Class
- Declared `static` inside another class
- Can access only outer class's **static** members
- Does NOT need outer class instance

```java
class Outer {
    static int x = 10;
    static class Inner {
        void show() { System.out.println(x); }  // OK
    }
}
Outer.Inner inner = new Outer.Inner();  // no outer instance needed
```

### Inner Class (Non-static)
- Can access ALL outer class members (including private)
- Needs an outer class instance

```java
class Outer {
    private int x = 10;
    class Inner {
        void show() { System.out.println(x); }  // can access private
    }
}
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
```

### Anonymous Class
```java
Runnable r = new Runnable() {
    @Override
    public void run() { System.out.println("Running"); }
};

// Lambda (preferred for functional interfaces):
Runnable r2 = () -> System.out.println("Running");
```

Use anonymous classes when: interface has multiple methods, or you need `this` to refer to the anonymous class itself.

---

## 3.11 Generics

Generics provide compile-time type safety — no `ClassCastException` at runtime.

```java
// Without generics — runtime error possible
List list = new ArrayList();
list.add("Hello");
String s = (String) list.get(0);  // manual cast

// With generics — compile-time safe
List<String> list = new ArrayList<>();
list.add("Hello");
String s = list.get(0);  // no cast needed
// list.add(100);  // COMPILE ERROR
```

### Generic Class
```java
class Box<T> {
    private T item;
    public void set(T item) { this.item = item; }
    public T get() { return item; }
}

Box<Integer> box = new Box<>();
box.set(42);
```

### Generic Method
```java
public <T> void print(T[] arr) {
    for (T e : arr) System.out.print(e + " ");
}
```

### Bounded Types
```java
// T must be Number or subclass
class Stats<T extends Number> {
    T[] nums;
    double avg() {
        double sum = 0;
        for (T n : nums) sum += n.doubleValue();
        return sum / nums.length;
    }
}
```

### Wildcards

| Wildcard | Syntax | Read as | Write | Use |
|----------|--------|---------|-------|-----|
| Unbounded | `List<?>` | `Object` | `null` only | read-only inspection |
| Upper bounded | `List<? extends T>` | `T` | `null` only | producer (read) |
| Lower bounded | `List<? super T>` | `Object` | `T` or subtype | consumer (write) |

**PECS Rule:** Producer → `extends`. Consumer → `super`.

```java
// dest consumes (write) → super; src produces (read) → extends
static <T> void copy(List<? super T> dest, List<? extends T> src) {
    for (int i = 0; i < src.size(); i++) dest.set(i, src.get(i));
}
```

### Generic Invariance
`List<String>` is NOT a subtype of `List<Object>` — generics are invariant.

Arrays are covariant (dangerous):
```java
Object[] arr = new String[3];
arr[0] = 100;  // compiles, throws ArrayStoreException at runtime
```

### Type Erasure
Generics exist only at compile time. The compiler erases type parameters:
- `<T>` → `Object`
- `<T extends Number>` → `Number`

**Limitations due to erasure:**
- Cannot use primitives: `List<int>` → use `List<Integer>`
- Cannot create `new T()`
- Cannot create `T[]` or `List<String>[]`
- Cannot use `instanceof List<String>` — use `instanceof List<?>`
- Cannot have static fields of type `T`
- Cannot overload methods that erase to the same signature

---

## Unit 3 — Key Output Questions

**Q1: Default method conflict**
```java
interface A { default void show() { System.out.println("A"); } }
interface B { default void show() { System.out.println("B"); } }
class C implements A, B {
    public void show() { A.super.show(); }
}
new C().show();  // A
```

**Q2: Class wins over interface default**
```java
interface A { default void show() { System.out.println("A"); } }
class B { public void show() { System.out.println("B"); } }
class C extends B implements A {}
new C().show();  // B
```

**Q3: Static interface methods are not inherited**
```java
interface I { static void show() { System.out.println("Interface"); } }
class C implements I {}
I.show();    // Interface
// C.show(); // COMPILE ERROR
```

**Q4: Constructor calls overridden method**
```java
abstract class A { A() { show(); } abstract void show(); }
class B extends A { int x = 10; void show() { System.out.println("x = " + x); } }
new B();  // x = 0 — x not initialized yet
```

**Q5: Inner class accesses private field**
```java
class Outer {
    private int x = 10;
    class Inner { void show() { System.out.println(x); } }
}
new Outer().new Inner().show();  // 10
```

---

# UNIT 4 — Advanced Object Behavior + Java Traps

---

## 4.1 Immutable Classes

An immutable object's state cannot change after creation.

**Rules:**
1. Declare class `final`
2. All fields `private final`
3. No setters
4. Initialize via constructor
5. Defensive copy for mutable fields

```java
final class Student {
    private final String name;
    private final List<String> courses;

    public Student(String name, List<String> courses) {
        this.name = name;
        this.courses = new ArrayList<>(courses);  // defensive copy
    }

    public String getName() { return name; }

    public List<String> getCourses() {
        return Collections.unmodifiableList(courses);  // no external mutation
    }
}
```

---

## 4.2 String Immutability

Once created, a `String`'s content cannot change. Any "modification" creates a new object.

```java
String s = "Hello";
s.concat(" World");     // new String created but NOT assigned
System.out.println(s);  // "Hello" — unchanged

s = s.concat(" World"); // now s points to new String
System.out.println(s);  // "Hello World"
```

### String Pool

```java
String a = "Java";           // goes to pool (or reuses)
String b = "Java";           // reuses same pool object
String c = new String("Java"); // forces new heap object

System.out.println(a == b);   // true — same pool reference
System.out.println(a == c);   // false — pool vs heap
System.out.println(a.equals(c)); // true — same content

String d = c.intern();  // returns pool reference
System.out.println(a == d);  // true
```

**Why String is immutable:** Security (file paths, DB credentials), String Pool sharing, `hashCode()` caching, thread safety.

### String Concatenation Trap
```java
String s1 = "ab" + "cd";   // compile-time constant → "abcd" → pool → true
String s2 = "abcd";
System.out.println(s1 == s2);  // true

String v = "ab";
String s3 = v + "cd";  // runtime → new heap object
System.out.println(s3 == s2);  // false

final String f = "ab";
String s4 = f + "cd";  // f is compile-time constant → pool
System.out.println(s4 == s2);  // true
```

---

## 4.3 Pass-by-Value (VERY IMPORTANT)

**Java is ALWAYS pass-by-value. Period.**

- Primitives: copy of the value is passed
- Objects: copy of the reference (address) is passed

```java
// Primitive — caller unchanged
void change(int x) { x = 100; }
int a = 10; change(a);
System.out.println(a);  // 10 — unchanged

// Object — mutation visible to caller
void modify(Student s) { s.name = "Bob"; }
Student s1 = new Student("Alice");
modify(s1);
System.out.println(s1.name);  // "Bob" — object mutated via same reference

// Object reassignment — caller unchanged
void reassign(Student s) { s = new Student("Charlie"); }
reassign(s1);
System.out.println(s1.name);  // "Bob" — caller's reference unchanged
```

```
Before reassign:  s1 ──> [Alice]   s ──> [Alice]
After s = new:    s1 ──> [Alice]   s ──> [Charlie]  (s1 unchanged)
```

**Swap test — impossible in Java:**
```java
void swap(Student a, Student b) { Student t = a; a = b; b = t; }
// Only local copies are swapped. Caller's references unchanged.
```

---

## 4.4 Shallow vs Deep Copy

**Shallow copy** — copies the object but shares references to nested objects.

```java
Person p2 = p1.shallowCopy();  // same Address object shared
p2.address.city = "LA";
System.out.println(p1.address.city);  // "LA" — p1 affected!
```

**Deep copy** — copies the object AND all nested objects.

```java
Person deepCopy() {
    return new Person(this.name, new Address(this.address.city));
}
// p1 unaffected by changes to p2
```

---

## 4.5 `clone()`

```java
class Student implements Cloneable {
    String name;
    @Override
    protected Student clone() throws CloneNotSupportedException {
        return (Student) super.clone();  // shallow copy
    }
}
```

**Limitations:** bypasses constructors, shallow by default, `CloneNotSupportedException`, considered broken.

**Preferred alternative — copy constructor:**
```java
Student(Student other) {
    this.name = other.name;
    this.age = other.age;
}
```

---

## 4.6 `equals()` Contract

`equals()` must be: Reflexive, Symmetric, Transitive, Consistent, Non-null.

**Symmetry trap with `instanceof`:**
```java
class Animal {
    @Override
    public boolean equals(Object o) {
        if (!(o instanceof Animal)) return false;
        return name.equals(((Animal)o).name);
    }
}
class Dog extends Animal { /* adds breed */ }

Animal a = new Animal("Rex");
Dog d = new Dog("Rex", "Lab");
a.equals(d)  // true — Animal sees Dog as Animal
d.equals(a)  // false — Dog doesn't see Animal as Dog
// SYMMETRY BROKEN
```

**Fix:** Use `getClass()` when subclasses add fields to `equals()`:
```java
if (o == null || getClass() != o.getClass()) return false;
```

---

## 4.7 `hashCode()` Contract

1. If `a.equals(b)` is `true` → `a.hashCode() == b.hashCode()` (MUST)
2. If `a.hashCode() != b.hashCode()` → `a.equals(b)` is `false` (MUST)
3. Same hashCode does NOT guarantee `equals()` is `true` (collision is OK)

**If you override `equals()`, you MUST override `hashCode()`.**

```java
// Missing hashCode — HashMap breaks
class Key {
    int id;
    @Override public boolean equals(Object o) { /* id-based */ }
    // NO hashCode — uses Object's memory-based default
}
Map<Key, String> map = new HashMap<>();
map.put(new Key(1), "One");
map.get(new Key(1));  // null — different hashCode!
```

**Mutable keys in HashMap:**
```java
MutableKey key = new MutableKey(1);
map.put(key, "Hello");
key.value = 2;  // mutated!
map.get(key);   // null — lost! hashCode changed, wrong bucket
```

---

## 4.8 `toString()`

Default: `ClassName@hexHashCode`. Always override:

```java
@Override
public String toString() {
    return "Student{name='" + name + "', age=" + age + "}";
}
```

`toString()` is called implicitly in `System.out.println(obj)` and `"text" + obj`.

---

## 4.9 Garbage Collection

GC automatically reclaims memory of unreachable objects.

**An object becomes eligible for GC when no live reference points to it:**

```java
Student s = new Student();
s = null;              // eligible

s = new Student("A");
s = new Student("B");  // "A" is eligible

// Island of isolation — objects reference each other but no external references
Node a = new Node(), b = new Node();
a.next = b; b.next = a;
a = null; b = null;  // both eligible
```

- `System.gc()` is a request, not a command — JVM may ignore it
- `finalize()` is deprecated (Java 9+)
- Memory leaks still possible: static collections holding references, unclosed resources

---

## 4.10 Class Loading

```
Loading → Verification → Preparation → Resolution → Initialization
```

1. **Loading** — ClassLoader reads `.class` bytecode
2. **Verification** — bytecode checked for correctness
3. **Preparation** — static variables set to defaults
4. **Resolution** — symbolic references resolved
5. **Initialization** — static blocks and static variable initializers run

**ClassLoader hierarchy:**
```
Bootstrap ClassLoader (java.lang.*, java.util.*)
  └── Platform ClassLoader (extension classes)
      └── Application ClassLoader (your classes)
```

Static initialization runs ONCE when class is first loaded.

---

## 4.11 `instanceof`

```java
Dog d = new Dog();
System.out.println(d instanceof Dog);    // true
System.out.println(d instanceof Animal); // true
System.out.println(null instanceof Object); // false — always

// Pattern matching (Java 16+)
if (obj instanceof String s) {
    System.out.println(s.length());  // s already cast
}
```

---

## 4.12 Covariant Return Type

An overriding method can return a subtype of the parent's return type.

```java
class Producer { Object produce() { return new Object(); } }
class StringProducer extends Producer {
    @Override
    String produce() { return "Hello"; }  // String is subtype of Object — valid
}
```

Only for **reference types** — primitives must match exactly.

---

## 4.13 Exception Rules in Overriding

- Checked exceptions: can throw same, narrower, or nothing. Cannot throw broader or new checked exceptions.
- Unchecked exceptions: no restrictions.

```java
class Parent { void m() throws IOException {} }
class Child extends Parent {
    void m() throws FileNotFoundException {} // OK — narrower
    // void m() throws Exception {}          // COMPILE ERROR — broader
}
```

---

## 4.14 Private Methods and Inheritance

Private methods are NOT visible to subclasses — they cannot be overridden.

```java
class Parent {
    private void helper() { System.out.println("Parent helper"); }
    public void process() { helper(); }  // always calls Parent's helper
}
class Child extends Parent {
    private void helper() { System.out.println("Child helper"); }  // new method, not override
}
new Child().process();  // "Parent helper"
```

---

## 4.15 Constructors + Inheritance — Deep Example

```java
class A {
    static { System.out.println("A static"); }
    { System.out.println("A instance"); }
    A() { System.out.println("A()"); }
    A(int x) { this(); System.out.println("A(int): " + x); }
}
class B extends A {
    static { System.out.println("B static"); }
    { System.out.println("B instance"); }
    B() { super(10); System.out.println("B()"); }
}
new B();
```
Output:
```
A static
B static
A instance
A()
A(int): 10
B instance
B()
```

---

## Unit 4 — Key Output Questions

**Q1: String pool**
```java
String a = "Hello", b = "Hello", c = new String("Hello");
System.out.println(a == b);      // true
System.out.println(a == c);      // false
System.out.println(a.equals(c)); // true
```

**Q2: Pass-by-value with array**
```java
void change(int[] arr) { arr[0] = 100; arr = new int[]{9,8,7}; }
int[] nums = {1, 2, 3};
change(nums);
System.out.println(nums[0] + " " + nums.length);  // 100 3
```

**Q3: Integer cache**
```java
Integer a = 127, b = 127; System.out.println(a == b);  // true
Integer c = 128, d = 128; System.out.println(c == d);  // false
```

**Q4: equals overloading trap**
```java
class Test { public boolean equals(Test t) { return true; } }
Object t1 = new Test(), t2 = new Test();
System.out.println(t1.equals(t2));  // false — overloaded, not overridden
```

**Q5: Shallow clone**
```java
class A implements Cloneable {
    int[] data = {1, 2, 3};
    protected A clone() throws CloneNotSupportedException { return (A) super.clone(); }
}
A a1 = new A(); A a2 = a1.clone();
a2.data[0] = 99;
System.out.println(a1.data[0]);  // 99 — shared array
```

**Q6: Static method on null reference**
```java
class A { static int x = 10; static void show() { System.out.println("A"); } }
A obj = null;
System.out.println(obj.x);  // 10 — no NPE, static resolved by type
obj.show();                  // A — no NPE
```

**Q7: Static variable shared**
```java
class A { static int x = 100; }
A obj1 = new A(), obj2 = new A();
obj1.x = 200;
System.out.println(obj2.x);  // 200 — same static field
```

**Q8: Initialization order**
```java
class A { int x = 10; { x = 20; } A() { x = 30; } }
System.out.println(new A().x);  // 30
```

---

# UNIT 5 — SOLID Principles, Patterns, and Design

---

## 5.1 SOLID Principles

### S — Single Responsibility Principle
A class should have only one reason to change.

```java
// Bad: Employee does payroll, DB, and reports
// Good: separate PayCalculator, EmployeeRepository, ReportGenerator
```

### O — Open/Closed Principle
Open for extension, closed for modification. Add new behavior via new classes, not by modifying existing ones.

```java
// Bad: if-else for each shape in AreaCalculator
// Good: each Shape implements area() — add Triangle = new class, no changes
interface Shape { double area(); }
class Circle implements Shape { public double area() { return Math.PI * r * r; } }
class Triangle implements Shape { public double area() { return 0.5 * b * h; } }
```

### L — Liskov Substitution Principle
Subclasses must be substitutable for their superclasses without breaking the program.

```java
// Violation: Penguin extends Bird but throws UnsupportedOperationException on fly()
// Fix: separate FlyingBird interface; Penguin only implements Bird (eat)
```

### I — Interface Segregation Principle
Don't force classes to implement methods they don't use. Prefer small, focused interfaces.

```java
// Bad: interface Worker { code(); test(); design(); manage(); }
// Good: interface Coder { code(); }  interface Tester { test(); }
class Developer implements Coder, Tester { /* only what's relevant */ }
```

### D — Dependency Inversion Principle
High-level modules should depend on abstractions, not concrete implementations.

```java
// Bad: UserService creates new MySQLDatabase() directly
// Good:
interface Database { void save(String data); }
class UserService {
    private Database db;
    UserService(Database db) { this.db = db; }  // injected
}
UserService svc = new UserService(new PostgresDatabase());
```

---

## 5.2 Coupling and Cohesion

- **Low coupling** — classes depend on each other through interfaces, not implementations
- **High cohesion** — each class does one thing well

**Goal: Low coupling + High cohesion**

---

## 5.3 Dependency Injection

Instead of a class creating its own dependencies, they are injected from outside.

```java
// Constructor injection (preferred)
class OrderService {
    private final PaymentGateway gateway;
    OrderService(PaymentGateway gateway) { this.gateway = gateway; }
    void placeOrder(double amount) { gateway.pay(amount); }
}
```

Benefits: testability (can mock), flexibility (swap implementations), loose coupling.

---

## 5.4 Singleton Pattern

Ensures exactly one instance of a class exists.

**Enum Singleton (recommended):**
```java
enum Singleton {
    INSTANCE;
    public void doSomething() { System.out.println("Singleton action"); }
}
Singleton.INSTANCE.doSomething();
```

**Double-checked locking:**
```java
class Singleton {
    private static volatile Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) instance = new Singleton();
            }
        }
        return instance;
    }
}
```
`volatile` prevents instruction reordering so no thread sees a partially constructed instance.

---

## 5.5 Factory Pattern

Client requests an object without knowing the concrete class.

```java
interface Notification { void send(String message); }
class EmailNotification implements Notification { public void send(String m) { /* ... */ } }
class SMSNotification implements Notification { public void send(String m) { /* ... */ } }

class NotificationFactory {
    static Notification create(String type) {
        return switch (type) {
            case "email" -> new EmailNotification();
            case "sms" -> new SMSNotification();
            default -> throw new IllegalArgumentException("Unknown: " + type);
        };
    }
}

Notification n = NotificationFactory.create("email");
n.send("Hello!");
```

---

## 5.6 Builder Pattern

For objects with many optional parameters — avoids telescoping constructors.

```java
class Pizza {
    private final int size;
    private final boolean cheese, pepperoni;

    private Pizza(Builder builder) {
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.pepperoni = builder.pepperoni;
    }

    static class Builder {
        private final int size;
        private boolean cheese, pepperoni;
        Builder(int size) { this.size = size; }
        Builder cheese(boolean v) { this.cheese = v; return this; }
        Builder pepperoni(boolean v) { this.pepperoni = v; return this; }
        Pizza build() { return new Pizza(this); }
    }
}

Pizza p = new Pizza.Builder(12).cheese(true).pepperoni(true).build();
```

---

## 5.7 Strategy Pattern

Algorithm varies at runtime — encapsulate each algorithm in a class.

```java
interface PaymentStrategy { void pay(double amount); }
class CreditCard implements PaymentStrategy { public void pay(double a) { /* ... */ } }
class UPI implements PaymentStrategy { public void pay(double a) { /* ... */ } }

class PaymentProcessor {
    private PaymentStrategy strategy;
    void setStrategy(PaymentStrategy s) { this.strategy = s; }
    void process(double amount) { strategy.pay(amount); }
}
```

---

## 5.8 Observer Pattern

When one object changes, multiple dependents are notified automatically.

```java
interface Observer { void update(String event); }

class EventManager {
    private List<Observer> observers = new ArrayList<>();
    void subscribe(Observer o) { observers.add(o); }
    void unsubscribe(Observer o) { observers.remove(o); }
    void notify(String event) { observers.forEach(o -> o.update(event)); }
}

class EmailAlert implements Observer { public void update(String e) { System.out.println("Email: " + e); } }
class SMSAlert implements Observer { public void update(String e) { System.out.println("SMS: " + e); } }
```

---

## 5.9 Records (Java 16+)

Compact immutable data containers.

```java
record Student(int id, String name) {}

Student s = new Student(1, "Alice");
System.out.println(s.id());    // 1
System.out.println(s.name());  // Alice
System.out.println(s);         // Student[id=1, name=Alice]
```

Auto-generated: `private final` fields, all-args constructor, accessors, `equals()`, `hashCode()`, `toString()`.

Records are implicitly `final`, cannot extend any class, can implement interfaces.

---

## 5.10 Sealed Classes (Java 17+)

Control which classes can extend yours.

```java
sealed class Shape permits Circle, Rectangle, Triangle {}
final class Circle extends Shape { double radius; }
final class Rectangle extends Shape { double width, height; }
non-sealed class Triangle extends Shape { double base, height; }
```

Permitted subclasses must be `final`, `sealed`, or `non-sealed`. Enables exhaustive pattern matching in `switch`.

---

## 5.11 Functional Interfaces

An interface with exactly one abstract method — can be used with lambdas.

```java
@FunctionalInterface
interface Transformer { String transform(String input); }

Transformer upper = s -> s.toUpperCase();
System.out.println(upper.transform("hello"));  // HELLO
```

**Built-in functional interfaces:**

| Interface | Method | Purpose |
|-----------|--------|---------|
| `Predicate<T>` | `boolean test(T t)` | Test condition |
| `Function<T,R>` | `R apply(T t)` | Transform |
| `Consumer<T>` | `void accept(T t)` | Consume, no return |
| `Supplier<T>` | `T get()` | Supply, no input |
| `Runnable` | `void run()` | Run, no input/output |

---

## 5.12 OOP Design Problems

### Payment System
```java
interface PaymentMethod { boolean pay(double amount); String getMethodName(); }
interface DiscountStrategy { double apply(double amount); }

class CreditCardPayment implements PaymentMethod { /* ... */ }
class UPIPayment implements PaymentMethod { /* ... */ }
class PercentageDiscount implements DiscountStrategy { /* ... */ }

record Receipt(double original, double final_, String method, boolean success) {}

class PaymentProcessor {
    private PaymentMethod method;
    private DiscountStrategy discount;
    PaymentProcessor(PaymentMethod method) { this.method = method; }
    void setDiscount(DiscountStrategy d) { this.discount = d; }
    Receipt process(double amount) {
        double finalAmt = discount != null ? discount.apply(amount) : amount;
        boolean ok = method.pay(finalAmt);
        return new Receipt(amount, finalAmt, method.getMethodName(), ok);
    }
}
```

### Parking Lot
```java
enum VehicleSize { SMALL, MEDIUM, LARGE }
abstract class Vehicle { abstract VehicleSize getSize(); }
class Car extends Vehicle { public VehicleSize getSize() { return VehicleSize.MEDIUM; } }

class ParkingSpot {
    private VehicleSize size;
    private Vehicle parked;
    boolean canFit(Vehicle v) { return parked == null && v.getSize().ordinal() <= size.ordinal(); }
    boolean park(Vehicle v) { if (!canFit(v)) return false; parked = v; return true; }
    Vehicle unpark() { Vehicle v = parked; parked = null; return v; }
}

class ParkingLot {
    private List<ParkingSpot> spots;
    boolean parkVehicle(Vehicle v) {
        return spots.stream().filter(s -> s.canFit(v)).findFirst().map(s -> s.park(v)).orElse(false);
    }
}
```

### Employee Payroll
```java
abstract class Employee {
    private String name;
    private double baseSalary;
    Employee(String name, double salary) { this.name = name; this.baseSalary = salary; }
    abstract double calculateSalary();
    String getName() { return name; }
    double getBaseSalary() { return baseSalary; }
}
class FullTimeEmployee extends Employee {
    double bonus;
    double calculateSalary() { return getBaseSalary() + bonus; }
}
class ContractEmployee extends Employee {
    int hours;
    double calculateSalary() { return getBaseSalary() * hours; }
}
class Payroll {
    void process(List<Employee> employees) {
        employees.forEach(e -> System.out.println(e.getName() + ": " + e.calculateSalary()));
    }
}
```

---

# FINAL SECTION — Interview Master Practice

---

## Key Rules — Must Know

1. Java is always **pass-by-value**. For objects, the value of the reference is copied.
2. Every class implicitly inherits from `java.lang.Object`.
3. Constructors are NOT inherited.
4. If any constructor is written, the compiler does NOT insert a default.
5. `super()` or `this()` must be the **first statement** in a constructor — only one allowed.
6. Instance blocks run after super constructor returns, before child constructor body.
7. Static blocks run once when class is first loaded, in textual order.
8. Static methods cannot use `this` or `super`.
9. Static methods are **hidden**, not overridden.
10. Private methods cannot be overridden — they bind statically.
11. `final` methods cannot be overridden. `final` classes cannot be subclassed.
12. Fields and static members are resolved by **reference type** (compile time) — not polymorphic.
13. Overriding method must have same/wider access, same/covariant return, same/narrower checked exception.
14. Interface variables are implicitly `public static final`.
15. Interface abstract methods are implicitly `public abstract`.
16. Interface static methods are NOT inherited.
17. Class method wins over interface default method.
18. If two interface defaults conflict → class must override.
19. `equals()` overridden → must override `hashCode()`.
20. Equal objects must have equal hashcodes. Same hashcode doesn't mean equal.
21. `null instanceof Type` is always `false`.
22. Downcasting requires explicit cast; may throw `ClassCastException` at runtime.
23. `null` reference accessing static field/method → no NPE (resolved by reference type).
24. Integer cache: `-128 to 127` — `==` returns true within this range.
25. `abstract` + `final` = compile error. `abstract` + `static` = compile error.
26. Records are implicitly `final`, extend `java.lang.Record`.
27. Abstract classes can have 0 abstract methods — just prevents instantiation.
28. `clone()` is shallow by default; use copy constructors instead.
29. Covariant return: applies to reference types only — primitives must match exactly.
30. Overriding cannot throw broader checked exceptions; unchecked — no restriction.
31. `int` can widen to `long` for overloading. `int` → `Long` (widen + box) is illegal.
32. `null` matches most specific type in overloading; if ambiguous → compile error.
33. `String` is immutable; pool sharing is safe. `new String()` bypasses pool.
34. Blank final fields must be initialized in every constructor.
35. `static` local variables are illegal — static must be at class level.
36. Package-private methods in another package cannot be overridden by subclasses.
37. `ArrayStoreException` — runtime, when storing wrong type in covariant array.
38. `ExceptionInInitializerError` — wraps exceptions from static initializers.

---

## Interview Questions — Quick Reference

### Basic (30)
1. What is OOP? 4 pillars?
2. Class vs Object?
3. What is a constructor? Rules?
4. Default vs no-arg constructor?
5. Can constructor be private? When?
6. `this` vs `super`?
7. `this()` vs `super()`?
8. What is a static variable? Method? Block?
9. Can static methods access instance variables?
10. What is inheritance? Why no multiple class inheritance?
11. What is method overloading? Overriding?
12. Compile-time vs runtime polymorphism?
13. What is encapsulation? Abstraction?
14. Abstract class vs interface?
15. `final` for variable/method/class?
16. What is the root class in Java?
17. What is dynamic method dispatch?
18. What are access modifiers?
19. Can abstract class have a constructor?
20. Can interface have instance variables?
21. Can interface have a constructor?
22. Can an interface extend a class?
23. Can `abstract` and `final` be combined?
24. What is method hiding?
25. What is field hiding?
26. What is upcasting? Downcasting?
27. What is `instanceof`?
28. What is covariant return?
29. What is a static block?
30. What is initialization order?

### Intermediate (40)
31. What is the `equals()`/`hashCode()` contract?
32. Why override `hashCode()` when overriding `equals()`?
33. Can we override static methods? (No — hiding)
34. Can we override private methods? (No)
35. What is checked exception rule in overriding?
36. What is an inner class vs static nested class?
37. What is an anonymous class?
38. What is a local class?
39. What is shallow vs deep copy?
40. Why is `clone()` broken?
41. What are default methods in interfaces? Why added?
42. What are static interface methods? Inherited?
43. What is the diamond problem? How resolved?
44. What is composition? When preferred?
45. What is association vs aggregation vs composition?
46. Can you make abstract class final? (No)
47. Can interface be final? (No)
48. What is a marker interface?
49. What is delegation?
50. What is pattern matching for `instanceof`?
51. What is String Pool? `intern()`?
52. Why is String immutable?
53. What is String constant folding?
54. Java pass-by-value or pass-by-reference?
55. Can you swap objects in Java? (No, not by reference)
56. What is Integer caching?
57. What is `System.gc()`? Guaranteed? (No)
58. Is `finalize()` guaranteed to run? (No, deprecated)
59. What is an island of isolation?
60. How does GC detect unreachable objects?
61. What is class loading?
62. What is static vs dynamic binding?
63. Why calling overridable methods in constructors is dangerous?
64. What is effectively final?
65. Why can't inner classes define static members (pre-Java 16)?
66. Why must local inner class variables be effectively final?
67. Why are records final by default?
68. When to use abstract class vs interface?
69. What is SOLID?
70. What is dependency injection?

### Advanced (30)
71. How does vtable (dynamic dispatch) work in JVM?
72. Explain double-checked locking and role of `volatile`.
73. What is type erasure in generics?
74. What is PECS rule?
75. What is generic invariance vs array covariance?
76. What are sealed classes? Benefit?
77. What are records? Difference from normal class?
78. LSP violation example?
79. How to implement deep copy via serialization?
80. What is defensive copying?
81. `getClass()` vs `instanceof` in `equals()`?
82. Why is `String` final?
83. How does `String.intern()` work?
84. What is the diamond problem with default methods?
85. What are generics wildcards?
86. Why can't we use primitives with generics?
87. What is heap pollution?
88. What is `@SuppressWarnings("unchecked")`?
89. When does `ExceptionInInitializerError` occur?
90. How to design a thread-safe singleton without synchronized blocks?
91. What is Strategy vs State pattern?
92. What is Observer pattern? Modern Java way?
93. What is Builder vs Factory?
94. What is Decorator pattern?
95. How to design a class safe as HashMap key?
96. What is the Flyweight pattern?
97. What is the Adapter pattern?
98. What is the Command pattern?
99. How to log method calls without modifying the class?
100. How does Constructor injection prevent circular dependencies?

---

## Output Questions (50)

### Questions 1–10

**Q1:** Field hiding vs method dispatch
```java
class Parent { int x = 10; void show() { System.out.println("Parent"); } }
class Child extends Parent { int x = 20; void show() { System.out.println("Child"); } }
Parent p = new Child();
System.out.println(p.x); p.show();
```
**Answer:** `10`, `Child` — field=reference type, method=object type.

**Q2:** Static method hiding
```java
class A { static void m() { System.out.println("A"); } }
class B extends A { static void m() { System.out.println("B"); } }
A obj = new B(); obj.m();
```
**Answer:** `A` — static bound to reference type.

**Q3:** Overload priority — exact match
```java
// show(int), show(Integer), show(long), show(int...)
new Test().show(5);
```
**Answer:** `int` — exact match first.

**Q4:** Overload priority — widening over boxing
```java
// show(Integer), show(long), show(int...)
new Test().show(5);
```
**Answer:** `long` — widening before boxing.

**Q5:** Null matching most specific type
```java
// show(String), show(Object)
new Test().show(null);
```
**Answer:** `String` — most specific type.

**Q6:** Null matching ambiguous types
```java
// show(String), show(Integer)
// new Test().show(null);
```
**Answer:** Compile error — ambiguous.

**Q7:** Boxing then upcasting
```java
// show(Object)
new Test().show(5);
```
**Answer:** `Object` — int boxed to Integer, then upcast.

**Q8:** int widening + boxing together
```java
// show(Long)
// new Test().show(5); // what happens?
```
**Answer:** Compile error — cannot widen int to long then box to Long in one step.

**Q9:** Constructor chain
```java
class A { A() { System.out.print("A "); } }
class B extends A { B() { System.out.print("B "); } }
new B();
```
**Answer:** `A B`

**Q10:** Explicit super constructor
```java
class A { A(int x) { System.out.print("A" + x + " "); } }
class B extends A { B() { super(5); System.out.print("B "); } }
new B();
```
**Answer:** `A5 B`

---

### Questions 11–20

**Q11:** Full init order with static blocks
```java
class A {
    static { System.out.print("A_static "); }
    { System.out.print("A_instance "); }
    A() { System.out.print("A_cons "); }
}
class B extends A {
    static { System.out.print("B_static "); }
    { System.out.print("B_instance "); }
    B() { System.out.print("B_cons "); }
}
new B();
```
**Answer:** `A_static B_static A_instance A_cons B_instance B_cons`

**Q12:** Static field initialized with instance
```java
class Test {
    static Test t = new Test();
    static { System.out.print("static "); }
    { System.out.print("instance "); }
    Test() { System.out.print("constructor "); }
}
// Output: instance constructor static
```

**Q13:** Pass-by-value with array
```java
static void change(int[] arr) { arr[0] = 50; arr = new int[]{100, 200}; }
int[] nums = {1, 2, 3};
change(nums);
System.out.println(nums[0]);  // 50
```

**Q14:** Integer swap — doesn't work
```java
static void swap(Integer a, Integer b) { Integer t = a; a = b; b = t; }
Integer x = 10, y = 20; swap(x, y);
System.out.println(x + " " + y);  // 10 20
```

**Q15:** String pool with intern
```java
String s1 = "Java", s2 = "Java", s3 = new String("Java"), s4 = s3.intern();
System.out.println(s1 == s2);  // true
System.out.println(s1 == s3); // false
System.out.println(s1 == s4); // true
```

**Q16:** Integer cache
```java
Integer a = 127, b = 127; System.out.println(a == b);  // true
Integer c = 128, d = 128; System.out.println(c == d);  // false
```

**Q17:** Overridable method in constructor
```java
class Parent { Parent() { show(); } void show() { System.out.println("Parent"); } }
class Child extends Parent { int x = 42; void show() { System.out.println("Child: " + x); } }
new Child();  // Child: 0
```

**Q18:** Final method inherited
```java
class A { final void show() { System.out.println("A"); } }
class B extends A { /* void show() {} // ERROR */ }
new B().show();  // A
```

**Q19:** Default method conflict resolution
```java
interface A { default void show() { System.out.println("A"); } }
interface B { default void show() { System.out.println("B"); } }
class C implements A, B { public void show() { B.super.show(); } }
new C().show();  // B
```

**Q20:** Class wins over default
```java
interface A { default void show() { System.out.println("A"); } }
class B { public void show() { System.out.println("B"); } }
class C extends B implements A {}
new C().show();  // B
```

---

### Questions 21–30

**Q21:** Private method static binding
```java
class A { private void show() { System.out.println("A"); } void test() { show(); } }
class B extends A { void show() { System.out.println("B"); } }
new B().test();  // A — private not overridden
```

**Q22:** Field access via cast
```java
class A { int x = 10; } class B extends A { int x = 20; }
A a = new B();
System.out.println(((B)a).x);  // 20 — cast to B, B's x
```

**Q23:** `this()` chain
```java
class A { A() { this(10); System.out.print("A() "); } A(int x) { System.out.print("A(int) "); } }
class B extends A { B() { System.out.print("B() "); } B(int x) { this(); System.out.print("B(int) "); } }
new B(5);  // A(int) A() B() B(int)
```

**Q24:** String immutability
```java
String s = "Java"; s.concat(" Rocks"); s.toUpperCase();
System.out.println(s);  // Java
```

**Q25:** Constant folding
```java
String s1 = "ab" + "cd"; String s2 = "abcd";
System.out.println(s1 == s2);  // true

String v = "ab"; String s3 = v + "cd";
System.out.println(s3 == s2);  // false

final String f = "ab"; String s4 = f + "cd";
System.out.println(s4 == s2);  // true
```

**Q26:** equals overloading
```java
class Test { public boolean equals(Test t) { return true; } }
Object a = new Test(), b = new Test();
System.out.println(a.equals(b));  // false — Object.equals() used
```

**Q27:** static field via null
```java
class A { static int x = 10; }
A obj = null;
System.out.println(obj.x);  // 10, no NPE
```

**Q28:** Covariant return
```java
class A { A create() { return new A(); } }
class B extends A { @Override B create() { return new B(); } }
A obj = new B();
System.out.println(obj.create().getClass().getSimpleName());  // B
```

**Q29:** Static shared field
```java
class A { static int x = 100; }
A obj1 = new A(), obj2 = new A();
obj1.x = 200;
System.out.println(obj2.x);  // 200 — same static field
```

**Q30:** String replace doesn't modify original
```java
String s = "Hello"; s.replace('H', 'W');
System.out.println(s);  // Hello
```

---

### Questions 31–40

**Q31:** Parameter shadowing
```java
class Test { int x; Test(int x) { x = x; } }  // BUG: x = x assigns param to param
new Test(5).x;  // 0 — use this.x = x
```

**Q32:** Final array
```java
final int[] arr = {1, 2, 3};
arr[0] = 99;  // OK — content modifiable
System.out.println(arr[0]);  // 99
```

**Q33:** Inner class private access
```java
class Test { private int x = 10; class Inner { void show() { System.out.println(x); } } }
new Test().new Inner().show();  // 10
```

**Q34:** Static nested class
```java
class Test { static int x = 10; static class Nested { void show() { System.out.println(x); } } }
new Test.Nested().show();  // 10
```

**Q35:** Dynamic dispatch
```java
class A { void show() { System.out.println("A"); } }
class B extends A { void show() { System.out.println("B"); } }
A obj = new B(); obj.show();  // B
```

**Q36:** Compile-time check on reference
```java
class A { void show() {} }
class B extends A { void print() {} }
A obj = new B();
// obj.print();  // COMPILE ERROR — A doesn't have print()
```

**Q37:** `instanceof` with null
```java
System.out.println(null instanceof Object);  // false
```

**Q38:** Static method hiding
```java
class Parent { static void print() { System.out.println("Parent"); } }
class Child extends Parent { static void print() { System.out.println("Child"); } }
Parent p = new Child(); p.print();  // Parent
```

**Q39:** Blank final field
```java
class A { final int x; A() { x = 10; } }
System.out.println(new A().x);  // 10
```

**Q40:** Static vs instance field
```java
class Test { static int x = 10; int y = 20; }
Test t1 = new Test(), t2 = new Test();
t1.x = 30; t1.y = 40;
System.out.println(t2.x + " " + t2.y);  // 30 20
```

---

### Questions 41–50

**Q41:** Runtime concat vs compile-time pool
```java
String s1 = "ab", s2 = "cd", s3 = "abcd", s4 = s1 + s2;
System.out.println(s3 == s4);  // false — runtime concat

final String f1 = "ab", f2 = "cd";
String s5 = f1 + f2;
System.out.println(s3 == s5);  // true — compile-time constant
```

**Q42:** equals overloading reference
```java
class Test { public boolean equals(Test t) { return true; } }
Object t1 = new Test(), t2 = new Test();
System.out.println(t1.equals(t2));  // false — overloaded not overriding
```

**Q43:** new String ==
```java
String s1 = new String("Java"), s2 = new String("Java");
System.out.println(s1 == s2);  // false — separate heap objects
```

**Q44:** Parent only has parameterized constructor
```java
class Parent { Parent(int x) {} }
class Child extends Parent { /* Child() { } */ }  // COMPILE ERROR — no super()
```

**Q45:** Override widens access
```java
class Base { void show() { System.out.println("Base"); } }
class Derived extends Base { @Override public void show() { System.out.println("Derived"); } }
Base b = new Derived(); b.show();  // Derived — widening access is valid
```

**Q46:** Override narrows access
```java
class Base { public void show() {} }
class Derived extends Base { /* void show() {} */ }  // COMPILE ERROR — narrows access
```

**Q47:** Constructor dependency
```java
class A { A(int x) {} }
class B extends A {}  // COMPILE ERROR — B's default calls super(), which doesn't exist
```

**Q48:** Cannot call static on interface via class
```java
interface I { static void show() { System.out.println("I"); } }
class C implements I {}
I.show();    // I
// C.show(); // COMPILE ERROR
```

**Q49:** Overriding throws broader checked exception
```java
class A { void show() throws IOException {} }
class B extends A { /* void show() throws Exception {} */ }  // COMPILE ERROR
```

**Q50:** abstract + final / static + abstract
```java
// abstract final class A {}         // COMPILE ERROR
// abstract class A { abstract final void m(); }  // COMPILE ERROR
// class A { static abstract void m(); }          // COMPILE ERROR
```

---

## Compile-Time Error Questions (20)

| # | Code Snippet | Error Reason |
|---|-------------|-------------|
| 1 | `abstract final void show()` | abstract must be overridden, final prevents it |
| 2 | `abstract final class Test {}` | Same contradiction at class level |
| 3 | `interface Test { private int x = 10; }` | Interface fields must be public static final |
| 4 | `final int x; void init() { x = 10; }` | Blank final must be set in constructor |
| 5 | `class A { A(int x){} } class B extends A {}` | B's default constructor calls non-existent `super()` |
| 6 | `Test() { println("X"); this(10); }` | `this()` must be first statement |
| 7 | `Test() { this(); }` | Circular constructor reference |
| 8 | `void show(){}` parent, `int show() {}` child | Return type change without covariance |
| 9 | `protected void show()` parent, `void show()` child | Narrowing access modifier |
| 10 | `void show() throws IOException` parent, `void show() throws Exception` child | Broader checked exception |
| 11 | `void method() { static int y = 20; }` | `static` illegal for local variables |
| 12 | `static void show() { System.out.println(this); }` | `this` illegal in static context |
| 13 | `class C extends B, A {}` | Multiple class inheritance not allowed |
| 14 | `Test t = new Test()` where Test is interface | Cannot instantiate interface |
| 15 | `class A { final int x; }` (no constructor init) | Uninitialized blank final field |
| 16 | `void print(String... s){}` and `void print(String[] s){}` | Varargs and array same erasure |
| 17 | `this(10); super(20);` in same constructor | Only one constructor delegation allowed |
| 18 | `final void print(){}` parent; `void print(){}` child | Cannot override final method |
| 19 | `class B extends A {}` where `A { A(int x){} }` | Implicit `super()` doesn't exist |
| 20 | `static abstract void m();` | static + abstract contradictory |

---

## Runtime Exception Questions (20)

| # | Code | Exception |
|---|------|-----------|
| 1 | `(String) new Integer(100)` | `ClassCastException` |
| 2 | `null.length()` | `NullPointerException` |
| 3 | `arr[3]` (size 3) | `ArrayIndexOutOfBoundsException` |
| 4 | `Object[] = new String[3]; arr[0] = new Integer(5)` | `ArrayStoreException` |
| 5 | `Integer.parseInt("abc")` | `NumberFormatException` |
| 6 | `int x = 10 / 0` | `ArithmeticException: / by zero` |
| 7 | `clone()` without `Cloneable` | `CloneNotSupportedException` |
| 8 | `Boolean b = null; if(b)` | `NullPointerException` (unboxing null) |
| 9 | `Collections.emptyList().add("x")` | `UnsupportedOperationException` |
| 10 | Modifying list during for-each | `ConcurrentModificationException` |
| 11 | Infinite recursion | `StackOverflowError` |
| 12 | `Arrays.asList(1,2,3).add(4)` | `UnsupportedOperationException` |
| 13 | `"Hello".charAt(10)` | `StringIndexOutOfBoundsException` |
| 14 | Exception in static block | `ExceptionInInitializerError` |
| 15 | `new int[-5]` | `NegativeArraySizeException` |
| 16 | `new TreeSet<>().add(new Object())` | `ClassCastException` (no Comparable) |
| 17 | `Integer.valueOf("12.34")` | `NumberFormatException` |
| 18 | `new ArrayList<>().get(0)` | `IndexOutOfBoundsException` |
| 19 | `"null".substring(5)` | `StringIndexOutOfBoundsException` |
| 20 | `10.0 / 0` | Returns `Infinity` — no exception! |

---

## Top Interview Traps

1. **Field hiding**: `p.x` uses reference type, not object type.
2. **Static hiding**: `p.staticMethod()` uses reference type.
3. **Overridable in constructor**: child field is `0` during parent constructor.
4. **Null + static**: `null.staticField` no NPE — resolved at compile time.
5. **Integer cache**: `==` works only for -128 to 127.
6. **`final` reference ≠ immutable object**: can still modify contents.
7. **Private ≠ override**: same-named method in child is a new method.
8. **Runtime string concat ≠ pool**: `var + "cd"` makes heap object.
9. **Custom constructor blocks default**: write `A(int x){}` → `new A()` fails.
10. **Downcast compiles, fails runtime**: explicit cast compiles if subtype; JVM checks heap.
11. **`null instanceof T`**: always `false`.
12. **`equals(MyClass)` = overloading**: use `equals(Object o)` to override.
13. **Primitive covariance**: `int` cannot be covariant with `long` in overriding.
14. **Checked exception in override**: cannot be broader.
15. **Unchecked in override**: freely allowed.
16. **Abstract class has constructor**: called via `super()` in subclass.
17. **Interface fields are constants**: `public static final`.
18. **Interface static methods not inherited**: must call on interface name.
19. **Array covariance**: `String[]` → `Object[]`, but store wrong type → `ArrayStoreException`.
20. **Default conflict**: must explicitly override when two interfaces have same default.
21. **Local class variables must be effectively final**.
22. **Records are immutable**: no setters, fields are `private final`.
23. **Abstract class can have 0 abstract methods**.
24. **`this()` and `super()` are mutually exclusive** in a constructor.
25. **Static instance inside static block**: instance block runs during class loading.
26. **Float division by zero**: returns `Infinity`, not `ArithmeticException`.
27. **Static nested class**: cannot access outer instance fields.
28. **Overloading null with Object+String**: String wins (more specific).
29. **Overloading null with String+Integer**: compile error (ambiguous).
30. **`ExceptionInInitializerError`**: static block throws — not caught by `Exception`.
31. **Abstract subclass**: can defer abstract implementations.
32. **Object casting ≠ primitive casting**: `(Double) new Integer(5)` is ClassCastException.
33. **package-private override in different package**: it's a new method, not override.
34. **`String.intern()`**: adds to pool if not present, returns pool reference.
35. **Varargs == array signature**: `print(String... s)` and `print(String[] s)` same erasure.

---

## One-Day Revision Cheat Sheet

- **Polymorphism rules**: Reference type = compile check, what you can call. Object type = which overridden method runs. Fields + statics = reference type.
- **Init order**: Parent static → Child static (once). Parent instance+constructor → Child instance+constructor (every `new`).
- **Overload priority**: Exact → Widen → Box → Varargs. No widen+box together.
- **Interface**: fields = `public static final`, methods = `public abstract` (or default/static/private). Static not inherited.
- **Immutability**: `final` class + `private final` fields + no setters + defensive copy.
- **Pass-by-value**: References copied. Mutate → visible. Reassign → not visible to caller.
- **equals+hashCode**: Equal → same hash. Broken hash → HashMap loses entries.
- **`null instanceof X`**: always `false`. `null.staticMember`: no NPE.
- **Constructors**: NOT inherited, NOT overridden. `super()`/`this()` must be first.
- **`final`**: variable = no reassign. Method = no override. Class = no subclass.

---

## One-Week Study Plan

| Day | Focus Area | Key Tasks |
|-----|-----------|-----------|
| Day 1 | Unit 1: Foundations | Class anatomy, this, static, init order |
| Day 2 | Unit 2: Pillars | Polymorphism, dispatch, casting, overloading |
| Day 3 | Unit 3: Abstraction | Interfaces, default conflicts, generics |
| Day 4 | Unit 4: JVM & Traps | Pass-by-value, immutability, equals/hashCode |
| Day 5 | Unit 5: SOLID & Patterns | SOLID, Singleton, Factory, Builder, Strategy |
| Day 6 | Code practice | All 50 output + compile/runtime error questions |
| Day 7 | Design problems | Parking lot, coffee machine, payment system |
