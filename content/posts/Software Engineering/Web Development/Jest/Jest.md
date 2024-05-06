---
title: "Jest"
date: 2024-04-01T20:21:01.077Z
---

# Jest

Jest is a library added to your development package list which test specific examples.

```javascript
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

```javascript
const sum = require("./sum");

test("adds 1 + 2 to equal 3", () => {
  expect(sum(1, 2)).toBe(3);
});
```

## Matchers

Matchers are the things to test whether the values 'match' what you 'expect'.

```javascript
expect(sum(1, 2)).toBe(3);
```

### Common Matchers

`toBe` matches against exact equality. It uses `Object.is` under the hood.
To match an object, you can use `toEqual` which recursively checks each value in the object or array. However, it ignores object keys with `undefined` properties. So you should use `toStrictEqual` instead.
The `not` matcher inverts the matcher to check that it is not that matcher.

```javascript
expect(a + b).not.toBe(0);
```

#### Truthiness

You can differentiate between `undefined`, `null`, and `false`. There are matchers that can pick and choose any of these.

`toBeNull` matches only null
`toBeUndefined` matches only undefined
`toBeDefined` is the opposite of toBeUndefined
`toBeTruthy` matches anything that an if statement treats as true
`toBeFalsy` matches anything that an if statement treats as false

#### Numbers

You can check greater than, equal to, or less than. Inclusive or exclusive. For `floating points`, you can use `toBeCloseTo`.

#### Strings

You can use `regex` to check against strings.

```javascript
expect("team").not.toMatch(/I/);
```

#### Arrays and iterables

You can check if an item exists in an array or iterable using `toContain`.

```javascript
const shoppingList = [
  "diapers",
  "kleenex",
  "trash bags",
  "paper towels",
  "milk",
];

test("the shopping list has milk on it", () => {
  expect(shoppingList).toContain("milk");
  expect(new Set(shoppingList)).toContain("milk");
});
```

#### Exceptions

To check if a function throws an error, use `toThrow`

```javascript
expect(() => compileAndroidCode()).toThrow(Error);
```

## Mocking

Jest mocking allows you to overwrite the existing behaviour of certain functions. For instance if you are making an asynchronous call, you might want to 'mock' that with a function that returns an expected behaviour. There are two types of mocking: **Using a Mocking function** or a **Manual Mock**.

### Using a Mocking Function

```javascript
export function forEach(items, callback) {
  for (const item of items) {
    callback(item);
  }
}

// test
const forEach = require("./forEach");

const mockCallback = jest.fn((x) => 42 + x);

test("forEach mock function", () => {
  forEach([0, 1], mockCallback);
});
```

#### .mock property

All mock functions have a .mock property which allows you access how the mock was used, what arguments was passed into it.

```javascript
const myMock1 = jest.fn();
const a = new myMock1();
console.log(myMock1.mock.instances);
// > [ <a> ]

const myMock2 = jest.fn();
const b = {};
const bound = myMock2.bind(b);
bound();
console.log(myMock2.mock.contexts);
// > [ <b> ]

// The function was called exactly once
expect(someMockFunction.mock.calls).toHaveLength(1);

// The first arg of the first call to the function was 'first arg'
expect(someMockFunction.mock.calls[0][0]).toBe("first arg");

// The second arg of the first call to the function was 'second arg'
expect(someMockFunction.mock.calls[0][1]).toBe("second arg");
```

#### Mocking Return Values

You can mock a return value. This can be chained as well.

```javascript
const myMock = jest.fn();
console.log(myMock());
// > undefined

myMock.mockReturnValueOnce(10).mockReturnValueOnce("x").mockReturnValue(true);

console.log(myMock(), myMock(), myMock(), myMock());
// > 10, 'x', true, true
```

## Jest Object

### Mock Functions

### jest.fn(implementation?)
A `jest.fn(implementation?)` returns a new unused mock function. Unused here means that when you check that it has been called before, it'll return 0.

```typescript
const mockFn = jest.fn();
mockFn();
expect(mockFn).toHaveBeenCalled();
```


The implementation here is a function that you can pass in to mock what this mock function should return.

```typescript
// With a mock implementation:
const returnsTrue = jest.fn(() => true);
console.log(returnsTrue()); // true;
```
