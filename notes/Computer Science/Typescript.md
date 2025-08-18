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

[docs](https://www.typescriptlang.org/docs/handbook/2/generics.html)
This is a way to build components to work over a variety of types.

```typescript
function identity<Type>(arg: Type): Type {
  return arg;
}
```

Now with this `generic` type T, you can pass in an arg of any type and it would work.

```typescript
function identity<Type>(arg: Type): Type {
  return arg;
}
```

#### Generic Interface

We can create a type for describing a generic function.

```typescript
interface GenericIdentityFn<Type> {
  (arg: Type): Type;
}

function identity<Type>(arg: Type): Type {
  return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

Now the myIdentity function has been assigned the identity generic function, but we also tell it what type it takes in.

#### Generic Classes

A generic class is similar to a generic interface.

```typescript
class GenericNumber<NumType> {
  zeroValue: NumType;
  add: (x: NumType, y: NumType) => NumType;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

Here we define a class that can take in a generic type for its properties and method arguments. Then when it is used, the Generic Class is provided a specific number type.

Generic classes are only generic on the `instance` side and not their `static side`.

#### Generic Constraints

If you want the generic to be constrained to only work with certain types. i.e. if you want it to only work for things with length. You can extend your generic off an interface.

```typescript
interface Lengthwise {
  length: number;
}

function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length); // Now we know it has a .length property, so no more error
  return arg;
}
```

#### Using Type Parameters in Generic Constraints

#### Using Class Types in Generics

#### Generic Parameter Defaults

#### Variance Annotations

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

A mixin is a way of extending a class. In the example, you pass in a class Sprite to the mixin Scale which extends this class.

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

We can constrain to the class that the mixin is applied to by passing in a generic in the constructor.

```typescript
type GConstructor<T = {}> = new (...args: any[]) => T;
```

Then the classes that you create from the mixin can only be built from the base class.

```typescript
type Positionable = GConstructor<{ setPos: (x: number, y: number) => void }>;
```

Now any classes that you want to extend has to extend from Positionable as well.

```typescript
function Jumpable<TBase extends Positionable>(Base: TBase) {
  return class Jumpable extends Base {
    jump() {
      // This mixin will only work if it is passed a base
      // class which has setPos defined because of the
      // Positionable constraint.
      this.setPos(0, 20);
    }
  };
}
```
