# Compound Component Pattern in React

## Overview

The **Compound Component Pattern** is one of the most powerful React patterns for building:

- Flexible components
- Reusable APIs
- Scalable UI systems
- Complex interactive components

Instead of placing all functionality inside a single component with dozens of props, this pattern distributes responsibilities across multiple cooperating components.

---

# Understanding Compound Components

Compound components work together as a cohesive unit while maintaining separate responsibilities.

A good mental model is native HTML elements like:

```html
<select>
  <option>Option 1</option>
  <option>Option 2</option>
</select>
```

Both `<select>` and `<option>` are separate elements, but together they create one complete feature.

The same concept applies in React.

---

# The Problem with Monolithic Components

Large configurable components often become difficult to maintain.

---

## ❌ Monolithic Approach

```tsx
<DataTable
  data={data}
  columns={columns}
  pagination={true}
  sorting={true}
  filtering={true}
  actions={["edit", "delete"]}
  rowSelection={true}
/>
```

Problems with this approach:

- Too many props
- Difficult API
- Harder maintenance
- Poor readability
- Reduced flexibility

---

# Compound Component Solution

## ✅ Compound Component Approach

```tsx
<DataTable.Root data={data}>
  <DataTable.Toolbar>
    <DataTable.Search />
    <DataTable.Filter />
    <DataTable.Actions />
  </DataTable.Toolbar>

  <DataTable.Content>
    <DataTable.Header>
      <DataTable.Column sortable>Name</DataTable.Column>
      <DataTable.Column sortable>Date</DataTable.Column>
      <DataTable.Column>Actions</DataTable.Column>
    </DataTable.Header>
  </DataTable.Content>

  <DataTable.Pagination />
</DataTable.Root>
```

---

# Benefits of Compound Components

## ✅ Flexibility

Easily include or exclude functionality depending on the use case.

---

## ✅ Better Maintainability

Responsibilities are distributed across smaller focused components.

---

## ✅ Better Developer Experience

- Cleaner JSX structure
- Easier IntelliSense
- Better readability

---

# Performance Optimization

For complex compound components, splitting contexts can reduce unnecessary re-renders.

```tsx
const CardStateContext = createContext({} as any);
const CardConfigContext = createContext({} as any);
```

---

# TypeScript Best Practices

```tsx
interface CardRootProps {
  children: React.ReactNode;
  variant?: "default" | "outline" | "ghost";
  size?: "default" | "sm" | "lg";
}
```

---

# Testing Compound Components

```tsx
import { render, screen } from "@testing-library/react";
```

---

# Summary

The **Compound Component Pattern** helps developers:

- Build cleaner APIs
- Reduce prop complexity
- Improve flexibility
- Create reusable design systems