# Container & Presenter Pattern in React

## Overview

The **Container & Presenter Pattern** is a React design pattern used to separate:

- Business logic
- Data handling
- UI rendering

The main goal of this pattern is to improve:

- Readability
- Reusability
- Testability
- Maintainability

It also helps prevent violations of the **Single Responsibility Principle (SRP)**.

---

# The Problem This Pattern Solves

Imagine a React component that:

- Fetches data from an API
- Manages loading and error states
- Contains business logic
- Renders UI

When all these responsibilities are placed inside a single component, the component becomes:

- Hard to understand
- Hard to maintain
- Hard to test
- Hard to reuse

The **Container & Presenter Pattern** solves this by separating responsibilities into dedicated components.

---

# Core Idea

> Separate **how things work** from **how things look**

| Component Type      | Responsibility         |
| ------------------- | ---------------------- |
| Container Component | Handles logic and data |
| Presenter Component | Handles UI rendering   |

---

# Container Component

## What Is a Container Component?

A **Container Component** is responsible for handling application logic and data flow.

It focuses on:

- Fetching data
- Managing state
- Handling side effects
- Implementing business logic

The container then passes data and callbacks to presenter components through props.

---

## Responsibilities

A Container Component typically:

- Fetches data from APIs
- Manages application state
- Handles user interactions
- Contains business logic
- Handles side effects
- Connects to external stores or services

Common tools used inside container components:

- `useState`
- `useEffect`
- Redux
- RTK Query
- Zustand
- React Query

---

## Important Notes

A container component:

✅ Manages state  
✅ Handles side effects  
✅ Contains business logic  
❌ Does NOT focus on UI design

---

# Presenter Component

## What Is a Presenter Component?

A **Presenter Component** is responsible only for rendering UI.

It receives:

- Data via props
- Event handlers via props

Then it renders JSX without caring where the data came from.

---

## Responsibilities

A Presenter Component typically:

- Focuses on UI rendering
- Receives data through props
- Receives callbacks through props
- Renders JSX only
- Remains reusable and easy to test

---

## Important Notes

A presenter component:

❌ Does NOT fetch data  
❌ Does NOT contain business logic  
❌ Does NOT subscribe to external stores  
❌ Does NOT manage global application state

It may contain small local UI state such as:

- Dropdown visibility
- Tooltip visibility
- Modal open/close state
- Uncontrolled input values

---
 
# Example Structure

```txt
components/
├── UserContainer.tsx
├── UserPresenter.tsx

features/
└── user/
    ├── components/
    │   └── UserPresenter.tsx
    ├── containers/
    │   └── UserContainer.tsx
    └── hooks/
        └── useUser.ts
```

---

# Example

## Container Component

```tsx
import { useEffect, useState } from "react";
import UserPresenter from "./UserPresenter";

export default function UserContainer() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users/1")
      .then((res) => res.json())
      .then(setUser);
  }, []);

  return <UserPresenter user={user} />;
}
```

---

## Presenter Component

```tsx
type Props = {
  user: {
    name: string;
    email: string;
  } | null;
};

export default function UserPresenter({ user }: Props) {
  if (!user) return <p>Loading...</p>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

---

# Modern Alternative: Custom Hooks

In modern React applications, developers often move container logic into **custom hooks**.

This still follows the same principle:

> Separate logic from UI.

---

## Example Using a Custom Hook

```tsx
import { useEffect, useState } from "react";

export const useUser = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users/1")
      .then((res) => res.json())
      .then(setUser);
  }, []);

  return { user };
};
```

---

## Using the Hook Inside a Component

```tsx
import { useUser } from "./useUser";

export default function UserProfile() {
  const { user } = useUser();

  if (!user) return <p>Loading...</p>;

  return <h2>{user.name}</h2>;
}
```

---

# Advantages of This Pattern

## ✅ Clear Separation of Responsibilities

You immediately know:

- Where business logic lives
- Where UI rendering happens

---

## ✅ Easier to Read

- Smaller files
- Cleaner structure
- Focused responsibilities

---

## ✅ Better Reusability

The same presenter component can work with multiple containers.

---

## ✅ Easier Testing

Presenter components are easy to test because they only depend on props.

---

## ✅ Better Scalability

This pattern becomes very useful as applications grow larger and business logic becomes more complex.

---

# Trade-offs

While this pattern improves separation of concerns,
it can introduce additional files and abstraction.
For very small components, this may become unnecessary complexity.

# When Should You Use This Pattern?

## ✅ Use It When

- Components become large
- Business logic becomes complex
- UI needs to be reusable
- Multiple developers work on the same feature
- You are building scalable applications

---

## ❌ Avoid It When

- Components are very small
- Logic is trivial
- Splitting components adds unnecessary complexity

---

# Common Beginner Mistakes

## ❌ Avoid These Mistakes

- Putting API calls inside presenter components
- Adding unnecessary `useState` or `useEffect` inside presenter components
- Overusing the pattern for tiny components
- Mixing UI logic with business logic again after separation

---

# Summary

The **Container & Presenter Pattern** helps developers:

- Separate logic from UI
- Improve code organization
- Build scalable applications
- Create reusable components
- Write easier-to-test code

This pattern is especially useful in medium and large React applications where components start handling too many responsibilities.
