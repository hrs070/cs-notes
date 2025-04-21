# Clean Code

## Introduction

"Clean Code" by Robert C. Martin is a must-read for software developers who want to write maintainable, efficient, and readable code. This book emphasizes principles, patterns, and practices that help developers produce high-quality code.

The following notes summarize key concepts from the book, along with examples in TypeScript to illustrate each principle.

---

## 1. Meaningful Names

### Principle

Choosing meaningful and descriptive names for variables, functions, and classes is critical for writing clean code. Names should reveal intent, avoid disinformation, and be pronounceable.

### Guidelines

- Use intention-revealing names.
- Avoid disinformation.
- Make names pronounceable.
- Use searchable names.
- Avoid encodings or Hungarian notation.

### ❌ Bad Example

```typescript
function d(a: number, b: number): number {
    return a * b / 2;
}

const t = d(10, 5);
console.log(t);
```

### ✅ Good Example

```typescript
function calculateTriangleArea(base: number, height: number): number {
    return (base * height) / 2;
}

const triangleArea = calculateTriangleArea(10, 5);
console.log(triangleArea);
```

### ❌ Hard to Pronounce

```typescript
const genymdhms = "2025-04-21T10:00:00Z";
```

### ✅ Easy to Pronounce

```typescript
const generationTimestamp = "2025-04-21T10:00:00Z";
```

---

## 2. Functions

### Principle

Functions should be small, do one thing, and do it well. They should be easy to read, test, and maintain.

### Guidelines

- Keep functions small (ideally under 20 lines).
- Make them do one thing.
- Use descriptive names.
- Avoid side effects.
- Keep the number of arguments low (ideally ≤ 3).

### ❌ Bad: Function does too many things

```typescript
function processUserData(user: { name: string; age: number; email: string }): void {
    console.log(`User: ${user.name}`);
    if (user.age >= 18) {
        console.log("User is an adult.");
    } else {
        console.log("User is a minor.");
    }
    sendEmail(user.email, "Welcome!");
}
```

### ✅ Good: Break it down

```typescript
function logUserName(name: string): void {
    console.log(`User: ${name}`);
}

function checkUserAge(age: number): void {
    if (age >= 18) {
        console.log("User is an adult.");
    } else {
        console.log("User is a minor.");
    }
}

function sendWelcomeEmail(email: string): void {
    sendEmail(email, "Welcome!");
}

function sendEmail(email: string, message: string): void {
    console.log(`Sending email to ${email}: ${message}`);
}

const user = { name: "John", age: 25, email: "john@example.com" };

logUserName(user.name);
checkUserAge(user.age);
sendWelcomeEmail(user.email);
```

### ❌ Bad: Too many arguments

```typescript
function createUser(name: string, age: number, email: string, isAdmin: boolean): void {
    console.log(`Name: ${name}, Age: ${age}, Email: ${email}, Admin: ${isAdmin}`);
}
```

### ✅ Good: Use an object

```typescript
interface User {
    name: string;
    age: number;
    email: string;
    isAdmin: boolean;
}

function createUser(user: User): void {
    console.log(`Name: ${user.name}, Age: ${user.age}, Email: ${user.email}, Admin: ${user.isAdmin}`);
}

const newUser: User = {
    name: "Jane",
    age: 30,
    email: "jane@example.com",
    isAdmin: true
};

createUser(newUser);
```

---

## 3. Comments

### Principle

Code should be self-explanatory whenever possible. Use comments to explain "why", not "what". Avoid redundant, misleading, or outdated comments.

### ✅ Good: Explains the why

```typescript
// Using a Set to improve lookup time
const visited = new Set<string>();
```

### ❌ Bad: Explains the obvious

```typescript
// Increment i by 1
i = i + 1;
```

---

## 4. Formatting

### Principle

Proper formatting improves the readability and maintainability of code. Consistent formatting helps developers quickly understand the structure and flow of the code.

### Guidelines

- Use consistent indentation (e.g., 4 spaces per level).
- Group related code together.
- Use blank lines to separate unrelated sections of code.
- Limit line length (e.g., 80–120 characters).
- Align code for clarity when appropriate.

### ❌ Bad Example

```typescript
// Bad: Inconsistent indentation and poor grouping
function calculateTotal(items: number[], tax: number): number {
let total=0;for(let i=0;i<items.length;i++){total+=items[i];}total+=total*tax;return total;}

const items=[10,20,30];
const tax=0.1;const total=calculateTotal(items,tax);console.log(total);
```

### ✅ Good Example

```typescript
// Good: Consistent indentation and proper grouping
function calculateTotal(items: number[], tax: number): number {
    let total = 0;

    for (let i = 0; i < items.length; i++) {
        total += items[i];
    }

    total += total * tax;
    return total;
}

// Usage
const items = [10, 20, 30];
const tax = 0.1;
const total = calculateTotal(items, tax);
console.log(total);
```

### Use Blank Lines to Separate Concepts

```typescript
// Bad: No separation between unrelated code
const user = { name: "Alice", age: 25 };
console.log(user.name); const items = [1, 2, 3]; console.log(items.length);

// Good: Add blank lines to separate concepts
const user = { name: "Alice", age: 25 };
console.log(user.name);

const items = [1, 2, 3];
console.log(items.length);
```

### Limit Line Length

```typescript
// Bad: Line is too long
const message = "This is a very long message that exceeds the recommended line length and makes the code harder to read.";

// Good: Break long lines into multiple lines
const message = "This is a very long message that exceeds the recommended line length " +
                "and makes the code harder to read.";
```

---

## 5. Error Handling

### Principle

Error handling is an essential part of writing clean code. It should be done gracefully and in a way that does not obscure the logic of the program. Avoid cluttering the code with error-handling logic and ensure that errors are handled consistently.

### Guidelines

- Use exceptions instead of return codes to handle errors.
- Provide meaningful error messages that describe the problem clearly.
- Handle exceptions at the appropriate level where they can be meaningfully addressed.
- Avoid swallowing exceptions without taking action or rethrowing them.
- Use custom exceptions to represent specific error conditions.

### ❌ Bad Example: Using Return Codes

```typescript
// Bad: Using return codes for error handling
function divide(a: number, b: number): number | null {
    if (b === 0) {
        return null; // Error case
    }
    return a / b;
}

const result = divide(10, 0);
if (result === null) {
    console.log("Error: Division by zero");
} else {
    console.log(result);
}
```

### ✅ Good Example: Using Exceptions

```typescript
// Good: Using exceptions for error handling
function divide(a: number, b: number): number {
    if (b === 0) {
        throw new Error("Division by zero is not allowed");
    }
    return a / b;
}

try {
    const result = divide(10, 0);
    console.log(result);
} catch (error) {
    console.error(error.message);
}
```

### Provide Meaningful Error Messages

```typescript
// Bad: Generic error message
throw new Error("Something went wrong");

// Good: Specific error message
throw new Error("Invalid user ID: User ID must be a positive integer");
```

### Handle Exceptions at the Appropriate Level

```typescript
// Bad: Catching exceptions too early
try {
    const data = fetchDataFromAPI();
    processData(data);
} catch (error) {
    console.error("Error occurred"); // No meaningful handling
}

// Good: Catch exceptions where they can be handled meaningfully
function fetchDataAndProcess(): void {
    try {
        const data = fetchDataFromAPI();
        processData(data);
    } catch (error) {
        console.error("Failed to fetch or process data:", error.message);
    }
}
```

### Use Custom Exceptions

```typescript
// Good: Define a custom exception
class InvalidUserInputError extends Error {
    constructor(message: string) {
        super(message);
        this.name = "InvalidUserInputError";
    }
}

function validateUserInput(input: string): void {
    if (input.trim() === "") {
        throw new InvalidUserInputError("Input cannot be empty");
    }
}

// Usage
try {
    validateUserInput("");
} catch (error) {
    if (error instanceof InvalidUserInputError) {
        console.error("User input error:", error.message);
    } else {
        console.error("Unexpected error:", error.message);
    }
}
```

---

## 6. Classes

### Principle

Classes should be small and focused, just like functions. A class should have a single responsibility and encapsulate related data and behavior. Avoid creating large, monolithic classes that try to do too much.

### Guidelines

- Follow the **Single Responsibility Principle (SRP)**: A class should have only one reason to change.
- Keep classes small and cohesive.
- Use meaningful names for classes that describe their purpose.
- Encapsulate internal details and expose only what is necessary through public methods.
- Prefer composition over inheritance when possible.

### ❌ Bad Example: Large, Monolithic Class

```typescript
// Bad: Class does too many things
class UserManager {
    private users: { name: string; email: string }[] = [];

    addUser(name: string, email: string): void {
        this.users.push({ name, email });
    }

    removeUser(email: string): void {
        this.users = this.users.filter((user) => user.email !== email);
    }

    sendEmail(email: string, message: string): void {
        console.log(`Sending email to ${email}: ${message}`);
    }

    listUsers(): void {
        console.log(this.users);
    }
}
```

### ✅ Good Example: Break It Down

```typescript
// Good: Separate responsibilities into smaller classes
class User {
    constructor(public name: string, public email: string) {}
}

class UserRepository {
    private users: User[] = [];

    addUser(user: User): void {
        this.users.push(user);
    }

    removeUser(email: string): void {
        this.users = this.users.filter((user) => user.email !== email);
    }

    getUsers(): User[] {
        return this.users;
    }
}

class EmailService {
    sendEmail(email: string, message: string): void {
        console.log(`Sending email to ${email}: ${message}`);
    }
}

// Usage
const userRepository = new UserRepository();
const emailService = new EmailService();

const user = new User("John Doe", "john@example.com");
userRepository.addUser(user);

emailService.sendEmail(user.email, "Welcome to our platform!");
console.log(userRepository.getUsers());
```

### Encapsulation Example

```typescript
// Good: Encapsulate internal details
class BankAccount {
    private balance: number;

    constructor(initialBalance: number) {
        this.balance = initialBalance;
    }

    deposit(amount: number): void {
        if (amount <= 0) {
            throw new Error("Deposit amount must be positive");
        }
        this.balance += amount;
    }

    withdraw(amount: number): void {
        if (amount > this.balance) {
            throw new Error("Insufficient funds");
        }
        this.balance -= amount;
    }

    getBalance(): number {
        return this.balance;
    }
}

// Usage
const account = new BankAccount(100);
account.deposit(50);
account.withdraw(30);
console.log(account.getBalance()); // Output: 120
```

---

## 7. Testing

### Principle

Testing is an essential part of writing clean code. Tests ensure that your code works as expected and remains maintainable over time. Clean tests are easy to read, understand, and maintain. They should focus on verifying behavior, not implementation details.

### Guidelines

- Write tests that are easy to read and understand.
- Follow the **FIRST** principles:
    - **Fast**: Tests should run quickly.
    - **Independent**: Tests should not depend on each other.
    - **Repeatable**: Tests should produce the same results every time.
    - **Self-validating**: Tests should automatically verify success or failure.
    - **Timely**: Write tests as soon as possible, ideally before writing the code (TDD).
- Use descriptive names for test cases.
- Test one thing at a time.
- Avoid testing implementation details; focus on behavior.

### ❌ Bad Example: Hard-to-Read Test

```typescript
// Bad: Test is hard to read and understand
test("should add user", () => {
    const u = new User("John", "john@example.com");
    const repo = new UserRepository();
    repo.addUser(u);
    expect(repo.getUsers().length).toBe(1);
    expect(repo.getUsers()[0].name).toBe("John");
});
```

### ✅ Good Example: Clean and Readable Test

```typescript
// Good: Test is clean and descriptive
test("should add a user to the repository", () => {
    // Arrange
    const user = new User("John", "john@example.com");
    const userRepository = new UserRepository();

    // Act
    userRepository.addUser(user);

    // Assert
    const users = userRepository.getUsers();
    expect(users).toHaveLength(1);
    expect(users[0].name).toBe("John");
    expect(users[0].email).toBe("john@example.com");
});
```

### Use Mocks and Stubs for Dependencies

```typescript
// Example: Mocking an email service
class MockEmailService {
    sendEmail(email: string, message: string): void {
        console.log(`Mock email sent to ${email}: ${message}`);
    }
}

test("should send a welcome email", () => {
    // Arrange
    const emailService = new MockEmailService();
    const user = new User("Jane", "jane@example.com");

    // Act
    emailService.sendEmail(user.email, "Welcome!");

    // Assert
    // (In a real test, you would verify that the email was sent correctly)
});
```

### Test Edge Cases

```typescript
// Example: Testing edge cases for a bank account
test("should throw an error when withdrawing more than the balance", () => {
    const account = new BankAccount(100);

    expect(() => account.withdraw(200)).toThrow("Insufficient funds");
});

test("should throw an error when depositing a negative amount", () => {
    const account = new BankAccount(100);

    expect(() => account.deposit(-50)).toThrow("Deposit amount must be positive");
});
```

---

## 8. General Principles

### Principle

Clean code is guided by a set of overarching principles that help developers write maintainable, efficient, and readable code. These principles ensure that the codebase remains simple, consistent, and free of unnecessary complexity.

### Guidelines

1. **Keep It Simple, Stupid (KISS)**: Avoid overengineering and keep the code as simple as possible.
2. **Don't Repeat Yourself (DRY)**: Eliminate duplication by abstracting common logic into reusable components.
3. **You Aren't Gonna Need It (YAGNI)**: Do not implement features or functionality until they are actually needed.
4. **Separation of Concerns (SoC)**: Divide the code into distinct sections, each responsible for a specific concern or functionality.
5. **Open/Closed Principle**: Code should be open for extension but closed for modification.
6. **Single Responsibility Principle (SRP)**: A class or function should have only one reason to change.
7. **Fail Fast**: Write code that fails early and clearly when something goes wrong.

### Examples in TypeScript

#### KISS: Keep It Simple

```typescript
// Bad: Overengineered solution
class MathOperations {
    static add(a: number, b: number): number {
        return a + b;
    }

    static subtract(a: number, b: number): number {
        return a - b;
    }
}

const result = MathOperations.add(5, 3);
console.log(result);

// Good: Simple and straightforward
const result = 5 + 3;
console.log(result);
```

#### DRY: Don't Repeat Yourself

```typescript
// Bad: Repeated logic
function calculateCircleArea(radius: number): number {
    return Math.PI * radius * radius;
}

function calculateSphereVolume(radius: number): number {
    return (4 / 3) * Math.PI * radius * radius * radius;
}

// Good: Abstract common logic
function calculatePower(base: number, exponent: number): number {
    return Math.pow(base, exponent);
}

function calculateCircleArea(radius: number): number {
    return Math.PI * calculatePower(radius, 2);
}

function calculateSphereVolume(radius: number): number {
    return (4 / 3) * Math.PI * calculatePower(radius, 3);
}
```

#### YAGNI: You Aren't Gonna Need It

```typescript
// Bad: Adding unnecessary functionality
class User {
    constructor(public name: string, public email: string) {}

    // Unused method
    getFullName(): string {
        return this.name;
    }
}

// Good: Only implement what is needed
class User {
    constructor(public name: string, public email: string) {}
}
```

#### Fail Fast

```typescript
// Bad: Fails silently
function getUserById(id: number): User | null {
    if (id <= 0) {
        return null; // Invalid ID
    }
    // Fetch user logic...
    return new User("John", "john@example.com");
}

// Good: Fails fast with clear error
function getUserById(id: number): User {
    if (id <= 0) {
        throw new Error("Invalid user ID");
    }
    // Fetch user logic...
    return new User("John", "john@example.com");
}
```