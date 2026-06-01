# Render Props pattern

At its core, the Render Props pattern is a technique for sharing code between React components
using a prop whose value is a function.

- This function
  - returns a React element
  - allows you to control what gets (التي يتم) rendered dynamically.

- Essentially, Render Props enables you to create reusable and adaptable (قابل للتكيف) components without having to (دون الحاجة إلى) resort to ( اللجوء إلى ) inheritance or complex setups.

## Purpose

- Render props are used to:
  - Share logic between components without using higher-order components (HOCs)
  - Make components more reusable and composable (قابل للتركيب)
  - Improve code readability and maintainability

In modern React, custom hooks are the preferred way to share stateful logic between function components. Render props remain a good fit when the shared component also needs to own some piece of UI structure
(e.g. wrapping children in an event listener container, virtualized lists, or "headless" UI libraries).

## How it works

- A component that uses a render prop takes a function as a prop.
- The component calls this function during render with whatever state or data it wants to expose,
  and renders the React element that the function returns.

## Example

```tsx
import { useState } from "react";

function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = (event) => {
    setPosition({ x: event.clientX, y: event.clientY });
  };

  return (
    <div style={{ height: "100vh" }} onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  );
}

// Usage
<MouseTracker
  render={({ x, y }) => (
    <h1>
      The mouse position is ({x}, {y})
    </h1>
  )}
/>;
```

## Benefits vs. Drawbacks

While there are numerous benefits to using Render Props, it is essential to also consider some limitations:

* Benefits

- Enhances Reusability: Logic encapsulation increases the reusability of code.
  Promotes Composition: Encourages a functional programming style, leading to clearer and more manageable components.

* Drawbacks

- Potential for Prop Drilling: Overusing Render Props can lead to prop drilling issues, especially in deeply nested components.
  Performance Concerns: If not optimized properly, frequent updates can lead to unnecessary re-renders, affecting performance.
