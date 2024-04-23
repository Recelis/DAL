---
title: "Node Modules"
date: 2024-04-22T22:25:06.653Z
---


[ECMA Standard](https://tc39.es/ecma262/#sec-intro)

# Node Modules
[node docs](https://nodejs.org/api/esm.html)

# Imports

## Namespace import
A namespace import is an import that resolves under a named object. i.e, hello in the example below.

```typescript
import * as hello from "./exports-function";
```

# Exports

## CommonJS

### Default Exports
```typescript
exports.default = "Hello, world!";
```

### Named exports
```typescript
exports.A = {};
```

## ESM

### Default Exports
```typescript
export default "Hello, world!";
```

### Named exports
```typescript
export const A = {};
```

## Interoperability with Typescript
[docs](https://www.typescriptlang.org/docs/handbook/modules/appendices/esm-cjs-interop.html)
Transpiling an ESM module to CJS is problematic because of the way it handles default exports. An ESM module will always transpile to an object.

```Typescript
import * as mod from "./module";
console.log(mod.default, mod.A, mod.B);
// transpiles to:
const mod = require("./module");
console.log(mod.default, mod.A, mod.B);
```

However, CJS modules allow default exports as non-objects such as strings or functions.
```typescript
// @Filename: exports-function.js
module.exports = function hello() {
  console.log("Hello, world!");
};

// importing as a named import becomes
import * as hello from './exports_function'
hello();
```

CJS modules allow hello to be called as a function. So how does a transpiler know whether an imported module is an ESM-transpiled-CJS module can only do object imports or a CommonJS module that can allow other types?



