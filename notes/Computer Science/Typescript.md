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

## Mixins

[docs](https://www.typescriptlang.org/docs/handbook/mixins.html)

A mixin is a way of extending a class. In the example, you pass in a class Sprite to Scale which extends this class.

```typescript
class Sprite {
  name = "";
  x = 0;
  y = 0;

  constructor(name: string) {
    this.name = name;
  }
}
```

```typescript
// To get started, we need a type which we'll use to extend
// other classes from. The main responsibility is to declare
// that the type being passed in is a class.

type Constructor = new (...args: any[]) => {};

// This mixin adds a scale property, with getters and setters
// for changing it with an encapsulated private property:

function Scale<TBase extends Constructor>(Base: TBase) {
  return class Scaling extends Base {
    // Mixins may not declare private/protected properties
    // however, you can use ES2020 private fields
    _scale = 1;

    setScale(scale: number) {
      this._scale = scale;
    }

    get scale(): number {
      return this._scale;
    }
  };
}
```

Then to use it:

```typescript
// Compose a new class from the Sprite class,
// with the Mixin Scale applier:
const EightBitSprite = Scale(Sprite);

const flappySprite = new EightBitSprite("Bird");
flappySprite.setScale(0.8);
console.log(flappySprite.scale);
```

As you can see, you can now pass in any class to the Scale class to extend it.

### Constrainted Mixins
