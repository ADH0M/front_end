# Compound Component Pattern in React

**Overview**
The **Compound Component Pattern** is one of the most powerful React patterns for building:

- Flexible components
- Reusable APIs
- Scalable UI systems
- Complex interactive components

Instead of placing all functionality inside a single component with dozens of props, this pattern distributes responsibilities across multiple cooperating components.

The compound component pattern allows you to create components that all work together to perform a task.

in React is a design pattern where a parent component works together with its child components to share an implicit state and behaviour. Instead of passing a long list of props, the parent manages the state and exposes flexible child components (<Modal.Header>, <Modal.Body>, <Modal.Footer>, and so on) so that consumers can compose the UI naturally, just like using native HTML elements.

## Understanding Compound Components

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

## The Problem with Monolithic Components

Large configurable components often become difficult to maintain.

- ❌ Monolithic Approach

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

✅ Compound Component Approach

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

## Benefits of Compound Components

### ✅ Flexibility

Easily include or exclude functionality depending on the use case.

### ✅ Better Maintainability

Responsibilities are distributed across smaller focused components.

### ✅ Better Developer Experience

- Cleaner JSX structure
- Easier IntelliSense
- Better readability

### Performance Optimization

For complex compound components, splitting contexts can reduce unnecessary re-renders.

```tsx
const CardStateContext = createContext({} as any);
const CardConfigContext = createContext({} as any);
```

## TypeScript Best Practices

```tsx
interface Props {
  children: React.ReactNode;
  isOpen: boolean;
  isClose: () => void;
}

interface CompoundComponent extends React.FC<Props> {
  Body: React.FC<{ children: React.ReactNode }>;
  Header: React.FC<{ children: React.ReactNode }>;
  Footer: React.FC<{ children: React.ReactNode }>;
}

const Model: CompoundComponent = ({ children, isOpen, isClose }) => {
```

## Testing Compound Components

```tsx
import { render, screen } from "@testing-library/react";
```

## Summary

The **Compound Component Pattern** helps developers:

- Build cleaner APIs
- Reduce prop complexity
- Improve flexibility
- Create reusable design systems

## Resources

1- [vercel.com](https://vercel.com/academy/shadcn-ui/compound-components-and-advanced-composition)

2- [patterns.dev](https://www.patterns.dev/react/compound-pattern/)

2- [freecodecamp.org](https://www.freecodecamp.org/news/compound-components-pattern-in-react/#heading-a-messy-modal-component)
