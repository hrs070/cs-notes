# 📘 Object-Oriented Programming (OOP)

**Object-Oriented Programming (OOP)** is a programming paradigm based on the concept of **objects**, which are instances of **classes**. It allows modeling complex software systems using **real-world metaphors**, focusing on **data + behavior**.

---

## 🔤 Key Terminologies

| Term                   | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **Class**              | A blueprint/template for creating objects.                                |
| **Object**             | An instance of a class with actual values and behavior.                   |
| **Property / Attribute** | Variables inside an object (state).                                       |
| **Method**             | Functions defined inside a class (behavior).                              |
| **Superclass / Base class** | A class whose features are inherited.                                  |
| **Subclass / Derived class** | A class that inherits features from another.                          |
| **Constructor**        | A special method used to initialize objects.                              |
| **this**               | Refers to the current instance of the class.                              |
| **super**              | Refers to the parent class and is used to call its methods or constructor. |
| **static**             | A keyword to define methods or properties that belong to the class itself, not instances. |

---

## 🧱 Pillars of OOP

The four main pillars of OOP are **Encapsulation**, **Abstraction**, **Inheritance**, and **Polymorphism**. Let’s explore each concept with explanations and TypeScript examples.

---

### 1. **Encapsulation**

> **Definition**: Wrapping data and methods that operate on the data into a single unit and restricting direct access to some components.

**Terminologies**:  

- `private`, `protected`, and `public` access modifiers control visibility.

**Explanation**:

- **`private`**: Accessible only within the class.
- **`protected`**: Accessible within the class and its subclasses.
- **`public`**: Accessible from anywhere.

**TypeScript Example**:
```typescript
class User {
    private password: string;

    constructor(private username: string, password: string) {
        this.password = password;
    }

    public authenticate(input: string): boolean {
        return this.password === input;
    }

    public getUsername(): string {
        return this.username;
    }
}

const user = new User("john_doe", "secret123");
console.log(user.authenticate("wrong")); // false
console.log(user.getUsername());         // "john_doe"
// user.password = "hack"; ❌ Not allowed – password is private
```

**Benefits**:

- Protects internal state.
- Encourages using methods to interact with data.

---

### 2. **Abstraction**

> **Definition**: Hiding internal implementation details and showing only the necessary parts to the user.

**Terminologies**: 

- `abstract class`, `interface`, `public methods`

**Explanation**:

- **Abstract Class**: A class that cannot be instantiated and is meant to be extended by other classes.
- **Interface**: Defines a contract that classes must follow.

**TypeScript Example**:
```typescript
abstract class PaymentProcessor {
    abstract process(amount: number): void;
}

class CreditCardProcessor extends PaymentProcessor {
    process(amount: number): void {
        console.log(`Processing ₹${amount} using Credit Card`);
    }
}

function payBill(processor: PaymentProcessor) {
    processor.process(1000);
}

payBill(new CreditCardProcessor());
```

**Benefits**:

- Users don’t need to understand internals.
- Provides a **clear interface**.

---

### 3. **Inheritance**

> **Definition**: Mechanism by which one class (subclass) can inherit properties and methods from another class (superclass).

**Terminologies**: 

- `extends`: Used to inherit from a parent class.
- `super`: Used to call the parent class’s constructor or methods.

**Explanation**:

- **`super` in Constructor**: Used to initialize the parent class.
- **`super` in Methods**: Used to call the parent class’s methods.

**TypeScript Example**:
```typescript
class Animal {
    constructor(public name: string) {}

    speak(): void {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    constructor(name: string, public breed: string) {
        super(name); // Call the parent class constructor
    }

    speak(): void {
        super.speak(); // Call the parent class method
        console.log(`${this.name} barks`);
    }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.speak();
// Output:
// Buddy makes a sound
// Buddy barks
```

**Benefits**:

- Code reuse.
- Logical hierarchy of types.

---

### 4. **Polymorphism**

> **Definition**: Ability of a single function or method to work in different ways based on the object type.

**Terminologies**: 

- `method overriding`: Subclass provides a specific implementation of a method defined in the parent class.
- `method overloading`: Multiple methods with the same name but different parameters (not natively supported in TypeScript).

**TypeScript Example**:
```typescript
class Shape {
    draw(): void {
        console.log("Drawing a shape");
    }
}

class Circle extends Shape {
    draw(): void {
        console.log("Drawing a circle");
    }
}

class Square extends Shape {
    draw(): void {
        console.log("Drawing a square");
    }
}

function render(shape: Shape) {
    shape.draw();
}

render(new Circle()); // "Drawing a circle"
render(new Square()); // "Drawing a square"
```

**Benefits**:

- Extensibility.
- Reusability of common interfaces with different implementations.

---

### 5. **Static Methods and Properties**

> **Definition**: `static` methods and properties belong to the class itself, not to any instance of the class.

**Explanation**:

- Static members are accessed using the class name, not an instance.
- Useful for utility functions or shared data.

**TypeScript Example**:
```typescript
class MathUtils {
    static PI: number = 3.14;

    static calculateArea(radius: number): number {
        return this.PI * radius * radius;
    }
}

console.log(MathUtils.PI); // 3.14
console.log(MathUtils.calculateArea(5)); // 78.5
```

**Benefits**:

- No need to create an instance for common functionality.
- Shared across all instances.

---

### 6. **`this` Keyword**

> **Definition**: Refers to the current instance of the class.

**Explanation**:

- Used to access instance properties and methods.
- Its value depends on the context in which it is called.

**TypeScript Example**:
```typescript
class Counter {
    private count: number = 0;

    increment(): void {
        this.count++;
        console.log(`Count: ${this.count}`);
    }
}

const counter = new Counter();
counter.increment(); // Count: 1
counter.increment(); // Count: 2
```

---

### 7. **`super` Keyword**

> **Definition**: Refers to the parent class and is used to call its methods or constructor.

**TypeScript Example**:
```typescript
class Parent {
    greet(): void {
        console.log("Hello from Parent");
    }
}

class Child extends Parent {
    greet(): void {
        super.greet(); // Call parent method
        console.log("Hello from Child");
    }
}

const child = new Child();
child.greet();
// Output:
// Hello from Parent
// Hello from Child
```

---

## 📊 Pillars Comparison

| Principle     | Purpose                             | Key Concept                        | Real-world Analogy |
|---------------|-------------------------------------|------------------------------------|---------------------|
| Encapsulation | Protect data and expose behavior    | `private`, `public`                | ATM machine – you use interface but don’t access internals |
| Abstraction   | Hide complexity, show essentials    | `abstract class`, `interface`      | Car dashboard – you press "start", not knowing how it works |
| Inheritance   | Reuse behavior                      | `extends`, `super`                 | Dog is a kind of Animal |
| Polymorphism  | Same function, different behavior   | `overriding`, method substitution  | A person behaves differently as a student, friend, employee |

---

## 🔄 Difference Between `implements` and `extends`

| Feature              | `extends`                          | `implements`                          |
|----------------------|-------------------------------------|---------------------------------------|
| **Purpose**          | Class inheritance                  | Interface implementation              |
| **Relationship**     | "is-a" (e.g., a Car is a Vehicle)  | "can-do" (e.g., a class can Fly)      |
| **Multiple Inheritances** | Only one class can be extended.     | Multiple interfaces can be implemented. |
| **Implementation**   | Inherits and potentially overrides methods. | Must provide implementations for all interface methods. |
| **Example**          | `class Dog extends Animal { ... }` | `class Bird implements Flyable { ... }` |

---