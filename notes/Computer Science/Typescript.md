# TypeScript

## Everyday Types

### Type Assertions

## Narrowing

### Type Predicates

## Discriminated union types

[Docs](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#discriminated-unions)
This is where one of the keys in the object will type the rest of the object.

```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };
```

## Type Manipulation

### Generics

### Keyof Type Operator

### Typeof Type Operator

### Indexed Access Types

### Conditional Types

### Mapped Types

### Template Literal Types

## Classes

### super

### this

### Implements

### Extends

### Decorators and Attributes
