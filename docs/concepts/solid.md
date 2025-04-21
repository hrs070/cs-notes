# SOLID Principles

The SOLID principles are five key design principles in object-oriented programming (OOP), introduced by Robert C. Martin (Uncle Bob). These principles help make software more maintainable, scalable, testable, and easier to understand.

---

## 🧱 What does SOLID stand for?

| Principle | Meaning                                  |
|-----------|------------------------------------------|
| S         | Single Responsibility Principle (SRP)    |
| O         | Open/Closed Principle (OCP)              |
| L         | Liskov Substitution Principle (LSP)      |
| I         | Interface Segregation Principle (ISP)    |
| D         | Dependency Inversion Principle (DIP)     |

---

## 💡 Summary

| Principle  | Goal                                     |
|------------|------------------------------------------|
| SRP        | One responsibility per class             |
| OCP        | Extend without modifying                 |
| LSP        | Safe substitutions                       |
| ISP        | Split big interfaces                     |
| DIP        | Depend on abstractions                   |

---

## 🧩 Single Responsibility Principle (SRP)

> **“A class should have only one reason to change.”**

✅ **Good**:  
Each class/module/function should do one thing only, and do it well.

❌ **Bad**:  
A class that reads a file, parses data, logs errors, and saves to a database — too many responsibilities.

✨ **Why it matters**:
- Easier to test
- Smaller classes = easier to debug and reuse
- Changes in one feature don’t accidentally break others

### Example in TypeScript:

**Bad Example**:
```typescript
class Report {
    generateReport(): void {
        console.log("Generating report...");
    }

    saveToFile(): void {
        console.log("Saving report to file...");
    }

    sendEmail(): void {
        console.log("Sending report via email...");
    }
}
```

**Good Example**:
```typescript
class ReportGenerator {
    generateReport(): string {
        return "Report content";
    }
}

class FileSaver {
    saveToFile(content: string): void {
        console.log("Saving report to file...");
    }
}

class EmailSender {
    sendEmail(content: string): void {
        console.log("Sending report via email...");
    }
}
```

---

## 🚪 Open/Closed Principle (OCP)

> **“Software entities should be open for extension, but closed for modification.”**

✅ **Good**:  
You can add new behavior without changing existing code.

❌ **Bad**:  
You keep editing an existing function/class every time requirements change.

🛠 **Example**:  
Use inheritance, interfaces, or polymorphism so you can extend behavior without touching old code.

### Example in TypeScript:

**Bad Example**:
```typescript
class PaymentProcessor {
    processPayment(paymentType: string): void {
        if (paymentType === "credit_card") {
            console.log("Processing credit card payment...");
        } else if (paymentType === "paypal") {
            console.log("Processing PayPal payment...");
        }
    }
}
```

**Good Example**:
```typescript
interface PaymentMethod {
    process(): void;
}

class CreditCardPayment implements PaymentMethod {
    process(): void {
        console.log("Processing credit card payment...");
    }
}

class PayPalPayment implements PaymentMethod {
    process(): void {
        console.log("Processing PayPal payment...");
    }
}

class PaymentProcessor {
    processPayment(paymentMethod: PaymentMethod): void {
        paymentMethod.process();
    }
}
```

---

## 🧬 Liskov Substitution Principle (LSP)

> **“Objects of a superclass should be replaceable with objects of subclasses without breaking functionality.”**

✅ **Good**:  
If `Bird` has a method `fly()`, then `Penguin` shouldn’t inherit from `Bird` unless it can logically fly.

❌ **Bad**:  
Subclass overrides behavior in a way that breaks expectations (e.g., throws `NotImplementedError`).

✨ **Why it matters**:
- Polymorphism only works if substitution is safe
- Prevents runtime surprises

### Example in TypeScript:

**Bad Example**:
```typescript
class Bird {
    fly(): void {
        console.log("Flying...");
    }
}

class Penguin extends Bird {
    fly(): void {
        throw new Error("Penguins can't fly!");
    }
}
```

**Good Example**:
```typescript
class Bird {}

class FlyingBird extends Bird {
    fly(): void {
        console.log("Flying...");
    }
}

class Penguin extends Bird {
    swim(): void {
        console.log("Swimming...");
    }
}
```

---

## 🧃 Interface Segregation Principle (ISP)

> **“Clients should not be forced to depend on interfaces they do not use.”**

✅ **Good**:  
Split large interfaces into smaller, focused ones.

❌ **Bad**:  
A class implementing an interface with 10 methods when it only needs 2.

✨ **Why it matters**:
- Prevents unnecessary implementation
- Leads to more modular, adaptable design

### Example in TypeScript:

**Bad Example**:
```typescript
interface Animal {
    fly(): void;
    swim(): void;
    run(): void;
}

class Dog implements Animal {
    fly(): void {
        throw new Error("Dogs can't fly!");
    }

    swim(): void {
        console.log("Dog swimming...");
    }

    run(): void {
        console.log("Dog running...");
    }
}
```

**Good Example**:
```typescript
interface Swimmer {
    swim(): void;
}

interface Runner {
    run(): void;
}

class Dog implements Swimmer, Runner {
    swim(): void {
        console.log("Dog swimming...");
    }

    run(): void {
        console.log("Dog running...");
    }
}
```

---

## 🔌 Dependency Inversion Principle (DIP)

> **“High-level modules should not depend on low-level modules. Both should depend on abstractions.”**

✅ **Good**:  
Use interfaces or abstract classes for dependencies.

❌ **Bad**:  
A class directly instantiates or depends on specific low-level implementations.

✨ **Tools to help**:
- Dependency Injection frameworks
- Inversion of Control (IoC) containers

### Example in TypeScript:

**Bad Example**:
```typescript
class MySQLDatabase {
    connect(): void {
        console.log("Connecting to MySQL...");
    }
}

class UserService {
    private db: MySQLDatabase;

    constructor() {
        this.db = new MySQLDatabase();
    }

    getUser(): void {
        this.db.connect();
        console.log("Fetching user...");
    }
}
```

**Good Example**:
```typescript
interface Database {
    connect(): void;
}

class MySQLDatabase implements Database {
    connect(): void {
        console.log("Connecting to MySQL...");
    }
}

class UserService {
    private db: Database;

    constructor(db: Database) {
        this.db = db;
    }

    getUser(): void {
        this.db.connect();
        console.log("Fetching user...");
    }
}

// Usage
const mySQL = new MySQLDatabase();
const userService = new UserService(mySQL);
userService.getUser();
```

---
