# OOSE + Software Engineering — SDE Interview Revision Notes

> **How to use:** Start with the bold definition, explain the example, then add the interview point. ⭐ marks high-priority topics.

## 1. SOLID Principles ⭐

### Single Responsibility Principle (SRP) ⭐
### Definition
A class should have only one reason to change, meaning it should have only one focused responsibility.
### Simple Explanation
Don't put all your code into one big class. A class should do one specific job. If it handles logic, it shouldn't handle database saving or UI rendering.
### Example
In a Shopping Cart app, an `Invoice` class should only calculate the total amount. Creating the PDF and sending the email should be done by `InvoicePdfGenerator` and `InvoiceEmailer` classes.
### Interview Point
Mention that SRP reduces the risk of breaking existing code when changing unrelated features. "One reason to change" is the keyword.

### Open/Closed Principle (OCP) ⭐
### Definition
Software entities (classes, modules, functions) should be open for extension but closed for modification.
### Simple Explanation
You should be able to add new features without changing the existing, working code. 
### Example
If you have a `PaymentProcessor` that handles Credit Cards, don't edit its `if/else` block to add PayPal. Instead, create a `PaymentMethod` interface and add new classes like `CreditCardPayment` and `PayPalPayment`.
### Interview Point
Say that OCP is usually achieved using Interfaces or Abstract classes (Polymorphism/Strategy Pattern) to add new behaviors safely.

### Liskov Substitution Principle (LSP) ⭐
### Definition
Objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.
### Simple Explanation
A child class must be able to do everything its parent class can do, without surprises.
### Example
If a `Bird` class has a `fly()` method, an `Ostrich` class inheriting from `Bird` will crash if `fly()` is called. This violates LSP. Ostrich shouldn't inherit from a flying bird.
### Interview Point
Mention that violating LSP often leads to ugly type checks (`instanceof`) or unexpected runtime errors.

### Interface Segregation Principle (ISP)
### Definition
Clients should not be forced to depend upon interfaces that they do not use.
### Simple Explanation
Don't make big, bulky interfaces. Split them into smaller, specific ones so classes only implement what they actually need.
### Example
Instead of one `Machine` interface with `print()`, `scan()`, and `fax()`, split it into `Printer`, `Scanner`, and `FaxMachine`. A simple printer won't be forced to implement an empty `scan()` method.
### Interview Point
Explain that ISP prevents "fat interfaces" and keeps the codebase clean and modular.

### Dependency Inversion Principle (DIP) ⭐
### Definition
High-level modules should not depend on low-level modules. Both should depend on abstractions (interfaces).
### Simple Explanation
Don't hardcode specific implementations. Depend on general interfaces so you can easily swap out the underlying technology.
### Example
An `OrderService` shouldn't directly create a `MySQLDatabase` object. Instead, it should depend on a `Database` interface. This way, you can switch to `MongoDBDatabase` without touching the `OrderService`.
### Interview Point
Say that DIP is often implemented using Dependency Injection (passing dependencies via constructors) to achieve loose coupling.


## 2. Design Principles

### DRY — Don’t Repeat Yourself
### Definition
Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.
### Simple Explanation
Don't copy-paste code. Write it once as a function or class and reuse it.
### Example
If both `UserRegistration` and `PasswordReset` check if an email is valid, move the regex logic to a shared `EmailValidator` utility.
### Interview Point
Mention that DRY prevents inconsistent bugs because a bug fixed in one place is fixed everywhere.

### KISS — Keep It Simple, Stupid
### Definition
Systems work best if they are kept simple rather than made complex.
### Simple Explanation
Don't overengineer. Write simple, readable code that solves the current problem directly.
### Example
If you need to store 5 hardcoded config values, use a simple dictionary/map. Don't build a complex database-backed configuration manager.
### Interview Point
Mention that simpler code is easier to maintain, test, and explain to new team members.

### YAGNI — You Aren’t Gonna Need It
### Definition
Always implement things when you actually need them, never when you just foresee that you need them.
### Simple Explanation
Don't write code for features you "might" need in the future.
### Example
Don't add multi-language support to your Food Delivery app on day 1 if you are only launching in one city.
### Interview Point
YAGNI saves time and prevents the codebase from bloating with unused, speculative abstractions.

### Separation of Concerns
### Definition
A program should be separated into distinct sections, where each section addresses a separate concern.
### Simple Explanation
Keep UI, business logic, and database operations separate.
### Example
In MVC (Model-View-Controller), the View only shows data, the Controller handles input, and the Model manages data rules.
### Interview Point
It is the broader concept behind SRP. It ensures changes in UI don't accidentally break business logic.

### Loose Coupling
### Definition
Components should be independent and know as little as possible about the inner workings of other components.
### Simple Explanation
Changing one part of the system shouldn't break other parts.
### Example
A `UserService` should talk to a `NotificationInterface`, not a specific `SendGridEmailClient`.
### Interview Point
Loose coupling makes unit testing easier (using mocks) and allows swapping components without rewriting the system.

### High Cohesion
### Definition
The degree to which the elements inside a module belong together.
### Simple Explanation
Things that change together and do similar work should stay together in the same class/module.
### Example
A `ShoppingCart` class should only contain methods like `addItem()`, `removeItem()`, `calculateTotal()`. It shouldn't contain `processPayment()`.
### Interview Point
High cohesion keeps code focused. Good design aims for **High Cohesion and Loose Coupling**.

### Composition over Inheritance
### Definition
It is better to build complex objects by assembling smaller objects (composition) rather than by inheriting from a base class.
### Simple Explanation
Use "Has-A" relationships instead of "Is-A" relationships to avoid rigid hierarchies.
### Example
Instead of making `ElectricCar` inherit from `Car` and overriding engine behaviors, give `Car` an `Engine` property, and pass in an `ElectricEngine` (Composition).
### Interview Point
Mention that Composition provides flexibility at runtime, while Inheritance locks you in at compile time and often leads to fragile hierarchies.


## 3. Design Patterns

### Creational Patterns

### Singleton ⭐
**Definition:**  
Ensures a class has only one instance and provides a global point of access to it.
**Problem:**  
You need exactly one instance of a shared resource across the entire app.
**How it works:**  
Make the constructor private. Create a static method that returns a static instance of the class, creating it only if it doesn't exist.
**Example:**  
A Database Connection Pool or a Logger instance.
**Interview Point:**  
Mention that Singleton makes unit testing difficult (hidden global state) and can cause issues in multi-threaded environments if not implemented thread-safely.

### Factory Method ⭐
**Definition:**  
Defines an interface for creating an object, but lets subclasses decide which class to instantiate.
**Problem:**  
You need to create objects based on some input, but you want to hide the complex creation logic from the client.
**How it works:**  
A central method takes an argument and returns the correct object using a common interface.
**Example:**  
`NotificationFactory.create("SMS")` returns an `SmsNotification` object, while `"EMAIL"` returns an `EmailNotification`.
**Interview Point:**  
Factory heavily uses Polymorphism and OCP. It is perfect for handling "if-else" object creation logic cleanly.

### Abstract Factory
**Definition:**  
Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
**Problem:**  
You need to create sets of matching objects.
**How it works:**  
You have a "factory of factories". A GUI Factory creates matching Buttons and Checkboxes for either Windows or Mac.
**Example:**  
`MacFactory` creates `MacButton` and `MacCheckbox`. `WindowsFactory` creates `WindowsButton` and `WindowsCheckbox`.
**Interview Point:**  
Mainly used when your system has to support multiple families of products.

### Builder ⭐
**Definition:**  
Separates the construction of a complex object from its representation.
**Problem:**  
A class requires a huge constructor with many optional parameters, making it unreadable (Telescoping Constructor Anti-pattern).
**How it works:**  
You create a separate Builder class with methods to set values step-by-step, ending with a `build()` method.
**Example:**  
`Pizza.builder().crust("Thin").cheese("Mozzarella").pepperoni(true).build();`
**Interview Point:**  
Builder is great for objects with many optional fields and for creating immutable objects cleanly.


### Structural Patterns

### Adapter ⭐
**Definition:**  
Converts the interface of a class into another interface clients expect.
**Problem:**  
Two incompatible interfaces need to work together.
**How it works:**  
You write a wrapper (Adapter) that translates the calls from the client to the format the third-party or legacy class expects.
**Example:**  
Your app expects `PaymentProcessor.pay(amount)`, but the new Stripe SDK uses `StripeAPI.makePayment(value)`. An adapter bridges this gap.
**Interview Point:**  
Adapter makes things work *after* they are designed; it acts like a real-world travel plug adapter.

### Decorator ⭐
**Definition:**  
Attaches additional responsibilities to an object dynamically.
**Problem:**  
You want to add features to an object without subclassing, avoiding a massive combination of classes.
**How it works:**  
You wrap the original object in a Decorator class that has the same interface, adding new behavior before or after delegating to the wrapped object.
**Example:**  
A base `Coffee` object. You wrap it in `MilkDecorator`, then `CaramelDecorator`. Each calculates its own price and adds it to the base.
**Interview Point:**  
Decorator is an alternative to subclassing. It follows the Open/Closed Principle perfectly.

### Facade
**Definition:**  
Provides a unified, simplified interface to a set of interfaces in a subsystem.
**Problem:**  
A subsystem is too complex with too many moving parts for a simple client to use.
**How it works:**  
Create a single class that handles the complex coordination of multiple underlying classes.
**Example:**  
A `HomeTheaterFacade` with a `watchMovie()` method that internally turns on the TV, dims lights, turns on the sound system, and starts the DVD player.
**Interview Point:**  
Facade doesn't hide the subsystem completely; it just provides a simpler default path for common tasks.


### Behavioral Patterns

### Observer ⭐
**Definition:**  
Defines a one-to-many dependency so that when one object changes state, all its dependents are notified.
**Problem:**  
Multiple objects need to react when another object changes, without tightly coupling them.
**How it works:**  
The "Subject" keeps a list of "Observers". When its state changes, it calls an `update()` method on all registered observers.
**Example:**  
YouTube subscriptions. When a channel (Subject) uploads a video, all subscribers (Observers) get a notification.
**Interview Point:**  
Heavily used in Event-Driven systems, UI frameworks (React/Vue), and Publish-Subscribe architectures.

### Strategy ⭐
**Definition:**  
Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
**Problem:**  
You have multiple ways to do something, and you want to switch between them dynamically at runtime.
**How it works:**  
Put the algorithms into separate classes implementing a common interface. Pass the desired strategy into the context object.
**Example:**  
A Navigation app uses a `RouteStrategy`. You can swap between `CarRouteStrategy`, `BikeRouteStrategy`, and `WalkingRouteStrategy` at runtime.
**Interview Point:**  
Strategy replaces complex `if-else` or `switch` statements and is the primary way to achieve the Open/Closed Principle.

### State
**Definition:**  
Allows an object to alter its behavior when its internal state changes.
**Problem:**  
An object has complex behavior depending on its current state.
**How it works:**  
Represent states as objects. The context object delegates its behavior to the current state object.
**Example:**  
A Vending Machine acts differently when `Idle`, `MoneyInserted`, or `Dispensing`.
**Interview Point:**  
Very similar to Strategy in structure, but in State, the states themselves often handle the transition to the next state.


### Design Pattern Comparisons

### Factory vs Abstract Factory

| Point | Factory | Abstract Factory |
|---|---|---|
| Definition | Creates one specific product using a method. | Creates families of related products. |
| Purpose | Hide object creation logic of a single type. | Ensure compatibility among a suite of products. |
| Example | `ButtonFactory` creates `Button`. | `UIFactory` creates matching `Button` and `Checkbox`. |

### Easy Way to Remember
> Factory makes a single item; Abstract Factory makes a matching set of items.

### Strategy vs State

| Point | Strategy | State |
|---|---|---|
| Definition | Encapsulates interchangeable algorithms. | Encapsulates state-specific behavior. |
| Purpose | Client explicitly chooses the algorithm. | Object changes its own state implicitly. |
| Example | Choosing a sorting algorithm (Quick vs Merge). | A Document moving from Draft to Published. |

### Easy Way to Remember
> In Strategy, the user decides how to do it. In State, the object decides what to do based on its condition.

### Adapter vs Decorator

| Point | Adapter | Decorator |
|---|---|---|
| Definition | Converts an interface to another. | Adds responsibilities dynamically. |
| Purpose | Make incompatible things work together. | Extend behavior without subclassing. |
| Example | Wrapping a legacy XML API to act like JSON. | Adding logging or caching to a repository. |

### Easy Way to Remember
> Adapter changes the interface; Decorator keeps the same interface but adds new behavior.


## 4. UML & Object-Oriented Design

### UML Diagrams

**Use Case Diagram:** Shows actors (users/systems) and the actions they can perform. Great for high-level requirements.  
**Class Diagram:** Shows classes, attributes, methods, and relationships. The blueprint of OO code.  
**Sequence Diagram:** Shows how objects interact over time in a specific scenario. Time flows top-to-bottom.  
**Activity Diagram:** A flowchart showing workflows, decisions, and parallel tasks.  
**State Diagram:** Shows the lifecycle of a single object and how events trigger state changes.  
**Object Diagram:** A snapshot of specific instances at runtime.  
**Component Diagram:** Shows high-level modules, services, and their dependencies.

### UML Relationships

**Association:**  
A generic connection where one object uses or knows about another. (e.g., `Teacher` and `Course`).

**Dependency:**  
A temporary relationship where a change in one class affects another. Usually, one class is passed as a parameter to a method in another.

**Generalization:**  
Inheritance. An "Is-A" relationship. (e.g., `Dog` is an `Animal`).

### Aggregation vs Composition

| Point | Aggregation | Composition |
|---|---|---|
| Definition | A "has-a" relationship where the child can exist independently. | A strong "has-a" relationship where the child dies with the parent. |
| Purpose | Represent weak ownership. | Represent strict ownership and lifecycle control. |
| Example | A `Department` has `Teachers`. If Department closes, Teachers still exist. | A `House` has `Rooms`. If House is destroyed, Rooms are destroyed. |

### Easy Way to Remember
> Aggregation is a team and players; Composition is a human and their brain.


## 5. SDLC — Software Development Life Cycle

### Simple Flow
Requirements → Design → Development → Testing → Deployment → Maintenance

### Definition
SDLC is the structured process used by software teams to design, develop, and test high-quality software.
### Simple Explanation
It is the standard step-by-step recipe for building a software application from scratch.
### Example
For a Banking app: First, gather rules (Requirements). Plan the database (Design). Write code (Development). Ensure transactions are safe (Testing). Put it on the App Store (Deployment). Fix bugs later (Maintenance).
### Interview Point
Mention that SDLC provides a framework for quality and risk management, regardless of which specific model (Agile, Waterfall) is used.


## 6. Software Development Models

### Waterfall Model
**Definition:** A linear, sequential approach where each phase must be completed before the next begins.  
**How it works:** Requirements are frozen upfront, followed strictly by design, coding, testing, and deployment.  
**Example:** Building software for a pacemaker, where requirements will absolutely not change.  
**When it is useful:** Projects with strict, clear, and unchanging requirements (e.g., government, medical).  
**Interview Point:** Emphasize that it is inflexible. A change late in the project is extremely expensive.

### V-Model (Validation and Verification)
**Definition:** An extension of Waterfall where every development phase has a corresponding testing phase.  
**How it works:** As you write requirements, you write acceptance tests. As you write code, you write unit tests.  
**Example:** Aerospace software where rigorous testing must be planned alongside design.  
**When it is useful:** High-criticality projects requiring strict validation.  
**Interview Point:** It ensures testability is considered early, but shares Waterfall's rigidity.

### Iterative Model
**Definition:** Develop a basic version quickly, then repeatedly refine and improve it in cycles.  
**How it works:** Build the whole system broadly, evaluate it, and improve the details in the next iteration.  
**Example:** Building a website outline first, then adding colors, then adding complex animations.  
**When it is useful:** When requirements are understood but the optimal solution needs to evolve.  
**Interview Point:** Iterative means improving something that is already there.

### Incremental Model
**Definition:** Divide the product into fully functioning pieces (increments) and deliver them one by one.  
**How it works:** Build module 1 fully, deliver it. Then build module 2, deliver it.  
**Example:** Launching an app with just Login and Search. A month later, adding a Payment module.  
**When it is useful:** When early delivery of core functionality provides immediate business value.  
**Interview Point:** Incremental means adding new, complete pieces over time.

### Spiral Model
**Definition:** A risk-driven model that combines iterative development with systematic risk analysis.  
**How it works:** You loop through planning, risk analysis, engineering, and evaluation in expanding spirals.  
**Example:** Developing a completely new AI operating system where technical feasibility is uncertain.  
**When it is useful:** Large, expensive, high-risk projects.  
**Interview Point:** The unique feature of Spiral is its explicit focus on Risk Analysis before each phase.

### Prototype Model
**Definition:** Building a quick, incomplete version to understand user requirements better.  
**How it works:** Build a UI mock or dummy app, show the client, get feedback, and then build the real system.  
**Example:** Creating a clickable Figma prototype to confirm the user flow before coding.  
**When it is useful:** When UI/UX is critical and requirements are vague.  
**Interview Point:** Ensure stakeholders know it's a throwaway prototype, not production code.

### Agile vs Waterfall

| Point | Waterfall | Agile |
|---|---|---|
| Definition | Linear, sequential phases. | Iterative, incremental cycles. |
| Purpose | Predictability and strict control. | Adaptability and rapid feedback. |
| Example | Building a bridge. | Building a startup web app. |

### Easy Way to Remember
> Waterfall is like printing a book (hard to change); Agile is like writing a blog (easy to update).

### Iterative vs Incremental

| Point | Iterative | Incremental |
|---|---|---|
| Definition | Refining the whole product repeatedly. | Building the product in distinct pieces. |
| Purpose | Improve quality through feedback. | Deliver value early. |
| Example | Sketching a painting, then coloring, then detailing. | Painting the sky, then the mountains, then the trees. |

### Easy Way to Remember
> Iterative is doing it better; Incremental is doing more of it.


## 7. Agile

**Agile** is a broad approach and mindset based on the Agile Manifesto. It values adaptability, customer collaboration, and working software over rigid planning and heavy documentation.

### Agile Principles
1. Customer satisfaction through early and continuous delivery.
2. Welcome changing requirements, even late in development.
3. Deliver working software frequently.
4. Business people and developers work together daily.
5. Continuous attention to technical excellence.

### Interview Point
Agile is not a specific process; it is a philosophy. Frameworks like Scrum and Kanban implement Agile.


## 8. Scrum

**Scrum** is a specific Agile framework that helps teams structure and manage their work through a set of roles, events, and artifacts.

### Roles
- **Product Owner:** Maximizes product value. Owns and prioritizes the Product Backlog.
- **Scrum Master:** A coach who removes blockers and ensures the team follows Scrum practices.
- **Developers:** The cross-functional team actually building the product.

### Events
- **Sprint:** A fixed timebox (usually 2 weeks) where work is done.
- **Sprint Planning:** Team decides what to pull from the Product Backlog into the Sprint.
- **Daily Scrum (Standup):** A 15-minute daily sync for Developers to plan the next 24 hours.
- **Sprint Review:** A demo of the finished work to stakeholders to gather feedback.
- **Sprint Retrospective:** A team meeting to discuss what went well and how to improve the process next Sprint.

### Artifacts and Concepts
- **Product Backlog:** The master list of all desired features and fixes, ordered by priority.
- **Sprint Backlog:** The specific items selected for the current Sprint.
- **Increment:** The working, usable piece of software delivered at the end of the Sprint.
- **User Story:** A requirement written from a user's perspective. ("As a user, I want to filter by price, so I can find cheap items.")
- **Epic:** A large chunk of work that is broken down into smaller User Stories.
- **Story Points:** A relative measure of effort/complexity used to estimate tasks (not hours).
- **Definition of Done (DoD):** A checklist of quality standards a feature must meet before it is considered finished (e.g., code reviewed, tests pass).
- **Burndown Chart:** A visual graph showing the remaining work versus time in a Sprint.

### Agile vs Scrum

| Point | Agile | Scrum |
|---|---|---|
| Definition | A broad philosophy for software development. | A specific framework to implement Agile. |
| Purpose | Value adaptability and collaboration. | Provide structure (roles, events) for Agile teams. |
| Example | "Let's be Agile and adapt to feedback." | "Let's use 2-week Sprints and Daily Standups." |

### Easy Way to Remember
> Agile is the diet (the philosophy); Scrum is the meal plan (the specific rules).


## 9. Software Testing

### Unit Testing
**Definition:** Testing individual, smallest testable parts (functions/methods) in isolation.  
**Simple Explanation:** Checking if one specific function does exactly what it's supposed to do.  
**Example:** Testing if `calculateDiscount(100, 10)` returns `90`.  
**Interview Point:** Unit tests should be fast, automated, and rely heavily on mocking external dependencies.

### Integration Testing
**Definition:** Testing combined units/modules to verify they work together correctly.  
**Simple Explanation:** Checking if two components talk to each other without crashing.  
**Example:** Testing if the `UserService` correctly saves user data to the actual `MySQLDatabase`.  
**Interview Point:** Finds issues in data flow and contracts between modules.

### System Testing
**Definition:** Testing the complete, integrated system as a whole.  
**Simple Explanation:** Testing the entire application from end-to-end to ensure it meets requirements.  
**Example:** Opening the app, logging in, adding items to a cart, paying, and checking the receipt.  
**Interview Point:** Usually performed in an environment that closely mimics production.

### Acceptance Testing
**Definition:** Formal testing with respect to user needs and business processes.  
**Simple Explanation:** The client/user uses the system to verify it solves their real-world problem.  
**Example:** Beta testers verifying they can successfully order food on the new app.  
**Interview Point:** It answers "Did we build the right product?" (Validation).

### Smoke Testing vs Regression Testing

| Point | Smoke Testing | Regression Testing |
|---|---|---|
| Definition | A quick check to see if critical functionalities work. | Re-testing to ensure a recent code change didn't break existing features. |
| Purpose | Verify the build is stable enough for deeper testing. | Prevent old bugs from reappearing. |
| Example | Does the app launch and can I log in? | I fixed the tax logic; does the checkout still work? |

### Easy Way to Remember
> Smoke test checks if it catches on fire immediately; Regression test checks if you accidentally broke a window while fixing the door.

### Functional vs Non-functional Testing

| Point | Functional Testing | Non-functional Testing |
|---|---|---|
| Definition | Tests *what* the system does against requirements. | Tests *how well* the system performs under conditions. |
| Purpose | Ensure features work correctly. | Ensure quality, speed, and security. |
| Example | Can the user upload a profile picture? | Does the upload finish in under 2 seconds? |

### Easy Way to Remember
> Functional is "Does it work?"; Non-functional is "Is it fast/secure/scalable?"

### Verification vs Validation

| Point | Verification | Validation |
|---|---|---|
| Definition | Are we building the product right? | Are we building the right product? |
| Purpose | Ensure code meets technical specifications. | Ensure the product meets the user's actual needs. |
| Example | Unit tests and code reviews. | User acceptance testing. |


## 10. Software Maintenance

### Corrective Maintenance
**Definition:** Fixing errors and bugs observed after the software is released.  
**Simple Explanation:** Patching things that are broken in production.  
**Example:** Fixing a crash that happens when users enter a negative quantity.  

### Adaptive Maintenance
**Definition:** Modifying software to keep it usable in a changing environment.  
**Simple Explanation:** Updating the app because the outside world changed.  
**Example:** Updating your iOS app because Apple released iOS 18.  

### Perfective Maintenance
**Definition:** Improving performance, maintainability, or adding new enhancements.  
**Simple Explanation:** Making the app better based on user feedback.  
**Example:** Making the search algorithm 20% faster.  

### Preventive Maintenance
**Definition:** Making changes to prevent future faults from occurring.  
**Simple Explanation:** Cleaning up code to avoid future disasters.  
**Example:** Refactoring messy code or updating an aging third-party library before it drops support.


## 11. Software Quality Attributes

### Reliability vs Availability
| Point | Reliability | Availability |
|---|---|---|
| Definition | The ability of a system to function correctly over time. | The proportion of time a system is up and usable. |
| Purpose | Ensure no wrong results or crashes. | Ensure users can access it when needed. |
| Example | Payment always processes exactly the right amount. | The servers have 99.99% uptime. |

### Easy Way to Remember
> Reliability is "Does it work correctly?"; Availability is "Is it awake to work at all?"

### Performance vs Scalability
| Point | Performance | Scalability |
|---|---|---|
| Definition | How fast the system responds under a specific load. | How well the system handles an increasing load by adding resources. |
| Purpose | Ensure low latency and high throughput. | Ensure the system doesn't crash when user count 10x's. |
| Example | The page loads in 200ms for 100 users. | Adding 3 more servers keeps the page fast for 10,000 users. |

### Easy Way to Remember
> Performance is speed; Scalability is capacity for growth.

### Other Attributes
- **Maintainability:** How easily a developer can understand, fix, or enhance the code.
- **Usability:** How intuitive and easy it is for a user to learn and use the UI.
- **Security:** How well the system protects data from unauthorized access.
- **Portability:** How easily the software can be transferred from one environment to another (e.g., Windows to Linux).
- **Reusability:** The degree to which components can be reused in other applications.

---

**Interview Ready Summary Pattern Checklist:**
- Keep your answers short.
- Always provide a real-world example (E-commerce, Banking, Rideshare).
- Highlight the **"Why"** (Why do we use Factory? Why do we use Microservices?).