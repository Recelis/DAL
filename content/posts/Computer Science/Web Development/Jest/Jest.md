---
title: 'Jest'
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
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
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
expect('team').not.toMatch(/I/);
```

#### Arrays and iterables
You can check if an item exists in an array or iterable using `toContain`.
```javascript
const shoppingList = [
  'diapers',
  'kleenex',
  'trash bags',
  'paper towels',
  'milk',
];

test('the shopping list has milk on it', () => {
  expect(shoppingList).toContain('milk');
  expect(new Set(shoppingList)).toContain('milk');
});
```

#### Exceptions
To check if a function throws an error, use `toThrow`
```javascript
expect(() => compileAndroidCode()).toThrow(Error);
```


## Mocking
