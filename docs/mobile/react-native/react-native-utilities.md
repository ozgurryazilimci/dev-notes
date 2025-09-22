# JavaScript & TypeScript Utilities Reference

This document contains frequently used utility functions and helpers for **React Native / TypeScript** projects.
Includes arrays, objects, strings, dates, math, and TS utility types.

---

## 1. Arrays

### fill

Fills all elements of an array with a static value.

```ts
const arr = new Array(5).fill(0);
// [0, 0, 0, 0, 0]
```

### map

Transforms each element in the array.

```ts
const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2);
// [2, 4, 6]
```

### flatMap

Maps each element and flattens the result.

```ts
const phrases = ["hello world", "good bye"];
const words = phrases.flatMap(str => str.split(" "));
// ["hello", "world", "good", "bye"]
```

### filter

Returns elements that satisfy a condition.

```ts
const nums = [1, 2, 3, 4, 5];
const even = nums.filter(n => n % 2 === 0);
// [2, 4]
```

### reduce

Accumulates values into a single output.

```ts
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, curr) => acc + curr, 0);
// 10
```

### find

Finds the first element that matches a condition.

```ts
const nums = [1, 2, 3, 4];
const found = nums.find(n => n > 3);
// 4
```

### some / every

```ts
const nums = [1, 2, 3, 4];
const hasEven = nums.some(n => n % 2 === 0); // true
const allPositive = nums.every(n => n > 0); // true
```

---

## 2. Objects

### Object.keys / values / entries

```ts
const obj = {name: "John", age: 25};

Object.keys(obj);   // ["name", "age"]
Object.values(obj); // ["John", 25]
Object.entries(obj);// [["name","John"], ["age",25]]
```

### hasOwnProperty

```ts
obj.hasOwnProperty('name'); // true
```

---

## 3. Strings

```ts
const str = "  Hello World!  ";

str.trim();       // "Hello World!"
str.toUpperCase(); // "HELLO WORLD!"
str.toLowerCase(); // "hello world"
str.replace("World", "React"); // "  Hello React!  "
str.split(" "); // ["", "Hello", "React!"]
```

### Template Strings

```ts
const name = "John";
const greeting = `Hello, ${name}!`;
// "Hello, John!"
```

---

## 4. Dates

```ts
const now = Date.now(); // milliseconds since 1970
const today = new Date();
today.toLocaleString(); // e.g., "9/23/2025, 10:30:15 AM"
```

---

## 5. Math

```ts
Math.random(); // random number 0-1
Math.floor(4.7); // 4
Math.ceil(4.2); // 5
Math.round(4.5); // 5
```

---

## 6. TypeScript Utility Types

### Partial<T>

Marks all properties optional.

```ts
type User = { name: string; age: number };
const partialUser: Partial<User> = {name: "John"};
```

### Required<T>

Marks all properties required.

```ts
type OptionalUser = { name?: string; age?: number };
const user: Required<OptionalUser> = {name: "John", age: 25};
```

### Pick<T, K>

Selects specific properties from a type.

```ts
type User = { name: string; age: number; email: string };
type UserPreview = Pick<User, "name" | "email">;
```

### Omit<T, K>

Excludes specific properties from a type.

```ts
type User = { name: string; age: number; email: string };
type UserWithoutEmail = Omit<User, "email">;
```

---

## 7. Faker.js (Random Data)

```bash
npm install @faker-js/faker --save-dev
```

```ts
import { faker } from '@faker-js/faker';

faker.name.firstName(); // e.g., "John"
faker.name.lastName();  // e.g., "Yılmaz"
faker.internet.email(); // e.g., "john@example.com"
faker.image.avatar();   // avatar URL
```

---

# Notes

- These utilities are extremely useful for creating mock data, manipulating arrays/objects, formatting strings, dates,
  numbers, and managing types safely in TypeScript.
- You can link this file from your main React Native documentation for quick reference.

---

# References

- [Faker JS](https://fakerjs.dev/)
