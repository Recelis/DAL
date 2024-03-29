+++
title = 'Typescript'
date = 2024-03-29T21:59:35.732Z
draft = false
+++

## discriminated union types
[Docs](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#discriminated-unions)
This is where one of the keys in the object will type the rest of the object.
```typescript
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };
```

