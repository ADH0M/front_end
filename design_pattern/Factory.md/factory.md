# Factory Method Design Pattern

## Definition

Factory Method is a creational design pattern that:

- provides an interface for creating objects in a superclass
- but allows subclasses to alter the type of objects that will be created.

---

# Core Idea

## Superclass

- knows an object will be created
- DOES NOT know the exact concrete type

---

## Subclass

- decides which concrete object will be created
- overrides the factory method

---

# Main Goal

Decouple object creation from object usage.

يعني:

- الـ client يستخدم الـ object
- بدون ما يعرف تفاصيل إنشائه

---

# Factory Types

## 1️⃣ Simple Factory

- usually one class with `if/switch`
- creates objects based on conditions
- not an official GoF pattern

Example:

```ts
if (type === "email") {
  return new EmailNotification();
}
```

---

## 2️⃣ Factory Method (Official GoF Pattern)

Uses:

- inheritance
- polymorphism
- subclasses to decide object type

---

# Components of Factory Method

## 1️⃣ Product

Interface or abstract type for created objects.

Example:

```ts
interface Notification {
  send(msg: string): string;
}
```

---

## 2️⃣ Concrete Products

Actual implementations of Product.

Example:

```ts
class EmailNotification implements Notification
class SmsNotification implements Notification
```

---

## 3️⃣ Creator

Declares the factory method.

Example:

```ts
abstract class Creator {
  abstract createNotification(): Notification;
}
```

---

## 4️⃣ Concrete Creator

Overrides factory method to create specific products.

Example:

```ts
class EmailCreator extends Creator {
  createNotification() {
    return new EmailNotification();
  }
}
```

---

# Flow

```ts
Client
   |
   v
Creator
   |
   +---- FactoryMethod()
             |
             v
      Concrete Creator
             |
             v
      Concrete Product
```

---

# Important Concept

## Creator depends on abstraction

The creator works with:

```ts
Notification;
```

NOT:

```ts
EmailNotification;
```

This is called:

# Programming to abstractions

---

# Why Factory Method?

## Benefits

### 1️⃣ Loose Coupling

Client code does not depend on concrete classes.

---

### 2️⃣ Open/Closed Principle

You can add new products without modifying existing code.

Example:

```ts
PushNotification;
```

can be added without changing:

- client
- creator

---

### 3️⃣ Better Maintainability

Object creation logic is centralized.

---

### 4️⃣ Extensibility

Easy to extend new product types.

---

# When to Use It

Use Factory Method when:

- object type is determined at runtime
- creation logic may vary
- you want loose coupling
- subclasses should control object creation
- you want to follow Open/Closed Principle

---

# Disadvantages

## 1️⃣ More Classes

Every new product usually needs:

- new product class
- new creator class

---

## 2️⃣ More Complexity

Can become overengineered for simple cases.

---

# Notification Example

```ts
interface Notification {
  send(msg: string): string;
}

class EmailNotification implements Notification {
  send(msg: string) {
    return msg;
  }
}

class SmsNotification implements Notification {
  send(msg: string) {
    return msg;
  }
}

abstract class Creator {
  public abstract createNotification(): Notification;

  public notify(msg: string) {
    const notify = this.createNotification();
    return notify.send("sending msg : " + msg);
  }
}

class ConcreteEmailNotification extends Creator {
  createNotification() {
    return new EmailNotification();
  }
}

class ConcreteSmsNotification extends Creator {
  createNotification() {
    return new SmsNotification();
  }
}

function client(creator: Creator, msg: string) {
  console.log(creator.notify(msg));
}

client(new ConcreteEmailNotification(), "email");
client(new ConcreteSmsNotification(), "sms");
```

---

# Important Observation 🧠

The most important line is:

```ts
const notify = this.createNotification();
```

Why?

Because:

- `Creator` uses the object
- but `ConcreteCreator` decides the real type

This is polymorphism.

---

# Pizza Example

```ts
// Product interface
interface Pizza {
    void prepare();
    void bake();
    void cut();
    void box();
}

// ConcreteProduct implementations
class MargheritaPizza implements Pizza {
    // Implementations of Pizza methods
}

class PepperoniPizza implements Pizza {
    // Implementations of Pizza methods
}

// Creator interface
interface PizzaFactory {
    Pizza createPizza();
}

// ConcreteCreator implementations
class MargheritaPizzaFactory implements PizzaFactory {
    @Override
    public Pizza createPizza() {
        return new MargheritaPizza();
    }
}

class PepperoniPizzaFactory implements PizzaFactory {
    @Override
    public Pizza createPizza() {
        return new PepperoniPizza();
    }
}

// Client code
public class PizzaStore {
    public static void main(String[] args) {
        PizzaFactory pizzaFactory = new MargheritaPizzaFactory();
        Pizza pizza = pizzaFactory.createPizza();
        // Use the pizza object
    }
}
```

---

# Simple Factory vs Factory Method

| Simple Factory    | Factory Method              |
| ----------------- | --------------------------- |
| Uses `if/switch`  | Uses inheritance            |
| One factory class | Multiple creator subclasses |
| Harder to extend  | Easier to extend            |
| Not official GoF  | Official GoF pattern        |
| Less flexible     | More flexible               |
