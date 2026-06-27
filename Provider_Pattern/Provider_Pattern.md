# Provider Pattern

- Make data available to multiple child components.

## Problem

- When multiple components need access to the same state, passing props through every intermediate
  component (prop drilling) becomes difficult to maintain.

- The Provider Pattern solves this problem by making shared data available to any component
  within a subtree.

## **How It Works**

- it uses React's **context api** ,which is a way to easily share data between components.
- It allows a parent component to expose shared state and behavior to any descendant component
  without manually passing props through every level of the component tree.

### Example

Imagine a landing page that supports both **Light Mode** and **Dark Mode**.

Several components depend on the current theme:

- Top Navigation
- Sidebar
- Cards
- Main Content
- Theme Toggle

Instead of passing the `theme` prop through every intermediate component, the Provider Pattern shares this state with all descendants.

- Several components change their style based on the currently active theme, such as the
  TopNav, the Listing cards, the Main section, and the Toggle.

- With the Provider pattern, we can share the theme state across multiple components throughout
  our application. The provider provides this context to components, which in turn consume this data.

## **_prop-drilling_**

- Before the Context API was available, we often ended up we often end up with something called
  prop drilling when we wanted to share data across multiple components. This is the case when we
  pass props far down the component tree.

```tsx
App
│
├── Layout
│      │
│      ▼
│   Sidebar
│      │
│      ▼
│   Navigation
│      │
│      ▼
│   ThemeButton
```

```tsx
App
│
▼
ThemeProvider
│
├── TopNav
├── Sidebar
├── Cards
├── Main
└── Toggle

```

## Implementation

- A Context object created with `createContext` exposes a `Provider` component.
- The Provider supplies a value that becomes available to every descendant component.

---

> **Note**
>
> `Context.Consumer` is the older API.
> Modern React applications typically use `useContext` because it is simpler and more readable.

```tsx
import { createContext, useState } from "react";
const ThemeContext = createContext(null);
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

---

```tsx
    import { ThemeProvider, ThemeContext } from "../context";

    const LandingPage = () => {
    <ThemeProvider>
        <TopNav />
        <Main />
    </ThemeProvider>;
    };

    const TopNav = () => {
    return (
        <ThemeContext.Consumer>
        {{ theme }} =>{" "}
        <div style={{ backgroundColor: theme === "light" ? "#fff" : "#000 " }}>
            ...
        </div>{" "}
        }
        </ThemeContext.Consumer>
    );
    };

    const Toggle = () => {
    return (
        <ThemeContext.Consumer>
        {{ theme, setTheme }} => (
        <button
            onClick={() => setTheme(theme === "light" ? "dark" : "light")}
            style={{
            backgroundColor: theme === "light" ? "#fff" : "#000",
            color: theme === "light" ? "#000" : "#fff",
            }}
        >
            Use {theme === "light" ? "Dark" : "Light"} Theme
        </button>
        )}
        </ThemeContext.Consumer>
    );
    };
```

---

- However, we can also combine the Provider with the Hooks pattern. Instead of wrapping components
  in a <ThemeContext.Consumer> component, we can use the built-in useContext hook.

  ```tsx
  export function useThemeContext() {
    const { theme, setTheme } = useContext(ThemeContext);

    return { theme, setTheme };
  }
  ```

  - Each component that needs to have access to the ThemeContext, can now simply use the
    useThemeContext hook.

    ```tsx
    const LandingPage = () => {
      <ThemeProvider>
        <TopNav />
        <Main />
      </ThemeProvider>;
    };

    const TopNav = () => {
      const { theme } = useThemeContext();
      return (
        <div style={{ backgroundColor: theme === "light" ? "#fff" : "#000 " }}>
          ...
        </div>
      );
    };

    const Toggle = () => {
      const { theme, setTheme } = useThemeContext();
      return (
        <button
          onClick={() => setTheme(theme === "light" ? "dark" : "light")}
          style={{
            backgroundColor: theme === "light" ? "#fff" : "#000",
            color: theme === "light" ? "#000" : "#fff",
          }}
        >
          Use {theme === "light" ? "Dark" : "Light"} Theme
        </button>
      );
    };
    ```

    - By creating hooks for the different contexts, it's easy to separate the providers's logic
      from the components that render the data.

## When to Use

- Global theme
- Authentication
- User preferences
- Language (i18n)
- Feature flags
- Shopping cart

## When NOT to Use

- Highly dynamic values
- Mouse position
- Animation state
- Scroll position
- Frequently updated game state

---

## Advantages

- Eliminates prop drilling.
- Makes shared state easy to access.
- Improves component reusability.
- Keeps related logic centralized.

---

## Disadvantages

- Every consumer re-renders when the context value changes.
- Not suitable for frequently changing state.
- Can become difficult to maintain if a single context stores too many unrelated values.

## Best Practices

### Split contexts by responsibility

```text
ThemeContext
UserContext
CartContext
LanguageContext
```

### Memoize provider values

```tsx
const value = useMemo(() => ({ theme, setTheme }), [theme]);
```

### Prefer custom hooks

Instead of

```tsx
const { theme } = useContext(ThemeContext);
```

Prefer

```tsx
const { theme } = useThemeContext();
```

---

### Keep contexts focused

Avoid putting unrelated state into a single context.

❌ Bad

```text
AppContext
├── user
├── theme
├── language
├── notifications
├── cart
├── settings
```

✅ Better

```text
UserContext
ThemeContext
CartContext
LanguageContext
```

## Performance Considerations

- **Scalability**: The Provider Pattern scales well because shared state is centralized and easy
  to access across the application.

- **Performance**: Components that consume the Provider's context re-render whenever a value changes,
  This can cause performance issues If you aren't careful which components are consuming the context.

## Provider vs Prop Drilling

| Prop Drilling          | Provider               |
| ---------------------- | ---------------------- |
| Pass props manually    | Shared automatically   |
| Difficult to scale     | Easier to maintain     |
| Many unnecessary props | Cleaner component tree |

---

## Interview Notes

**When should Context NOT be used?**

When state changes very frequently.

Examples:

- Mouse position
- Animation state
- Scroll position

In these cases, Context may trigger unnecessary re-renders.

## Summary

The Provider Pattern is built on top of React Context and is primarily used to share state across multiple components without prop drilling.

It works best for global or application-wide state such as themes, authentication, localization, and user preferences.

However, because every consumer re-renders when the context value changes, it should not be used for rapidly changing state.

## Related Patterns

- Custom Hooks
- Compound Components
- State Reducer
- Context Composition
