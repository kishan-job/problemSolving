# TypeScript Crash Course — 3 Hour Complete Guide

---

## Table of Contents

* **[Block 1 — 🏗️ Foundation (30 min)](#block-1--️-foundation-30-min)**
  * [What is TypeScript & Why](#what-is-typescript--why)
  * [Setup — tsc, ts-node, tsconfig](#setup--tsc-ts-node-tsconfig)
  * [Basic Types](#basic-types)
  * [Type Inference](#type-inference)
  * [Type Aliases & Interfaces + Differences](#type-aliases--interfaces--differences)
  * [Union & Intersection Types](#union--intersection-types)

* **[Block 2 — ⚙️ Functions & Objects (30 min)](#block-2--️-functions--objects-30-min)**
  * [Function Types](#function-types)
  * [Function Overloads](#function-overloads)
  * [Object Types](#object-types)
  * [Destructuring with Types](#destructuring-with-types)
  * [keyof, typeof, Indexed Access T\[K\]](#keyof-typeof-indexed-access-tk)

* **[Block 3 — 🏛️ Classes (20 min)](#block-3--️-classes-20-min)**
  * [Class Basics](#class-basics)
  * [Access Modifiers](#access-modifiers)
  * [Inheritance — extends, super](#inheritance--extends-super)
  * [Abstract Classes](#abstract-classes)
  * [Implementing Interfaces](#implementing-interfaces)

* **[Block 4 — 🧬 Generics (30 min)](#block-4--generics-30-min)**
  * [Generic Functions](#generic-functions)
  * [Generic Interfaces & Type Aliases](#generic-interfaces--type-aliases)
  * [Generic Classes](#generic-classes)
  * [Constraints — extends, keyof](#constraints--extends-keyof)
  * [Default Generic Types](#default-generic-types)
  * [Built-in Generics](#built-in-generics)

* **[Block 5 — 🔍 Type System Deep Dive (30 min)](#block-5--type-system-deep-dive-30-min)**
  * [Type Narrowing](#type-narrowing)
  * [Discriminated Unions](#discriminated-unions)
  * [Custom Type Guards — is keyword](#custom-type-guards--is-keyword)
  * [Assertion Functions](#assertion-functions)
  * [Mapped Types](#mapped-types)
  * [Conditional Types + infer](#conditional-types--infer)
  * [Template Literal Types](#template-literal-types)

* **[Block 6 — 🛠️ Utility Types (15 min)](#block-6--️-utility-types-15-min)**
  * [Partial, Required, Readonly, Pick, Omit, Record](#partial-required-readonly-pick-omit-record)
  * [Exclude, Extract, NonNullable](#exclude-extract-nonnullable)
  * [ReturnType, Parameters, InstanceType, Awaited](#returntype-parameters-instancetype-awaited)

* **[Block 7 — 🌍 Real World (25 min)](#block-7--real-world-25-min)**
  * [Module System](#module-system)
  * [tsconfig Must-Know Options](#tsconfig-must-know-options)
  * [Common Patterns](#common-patterns)
  * [Error Handling with Types](#error-handling-with-types)
  * [Declaration Merging](#declaration-merging)
  * [Enums vs Literal Unions](#enums-vs-literal-unions)

* **[Block 8 — 📌 Topics Not Covered](#block-8--topics-not-covered)**

---

## Block 1 — 🏗️ Foundation (30 min)

### What is TypeScript & Why

TypeScript is JavaScript with **static types** — it catches errors at compile time before your code runs.

```
JavaScript  →  TypeScript adds types  →  compiles back to JavaScript
```

**Why use it:**
- Catch bugs before runtime
- Better IDE autocomplete & refactoring
- Self-documenting code
- Scales better in large teams

```typescript
// JavaScript — error only at runtime
function add(a, b) { return a + b; }
add("5", 10);  // "510" — silent bug!

// TypeScript — error at compile time
function add(a: number, b: number): number { return a + b; }
add("5", 10);  // ❌ Error: Argument of type 'string' is not assignable to 'number'
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Setup — tsc, ts-node, tsconfig

```bash
# Install TypeScript globally
npm install -g typescript

# Check version
tsc --version

# Compile a file
tsc index.ts

# Watch mode — recompile on save
tsc --watch

# Run TypeScript directly (no compile step)
npm install -g ts-node
ts-node index.ts

# Initialize tsconfig.json
tsc --init
```

```json
// tsconfig.json — minimum to get started
{
  "compilerOptions": {
    "target": "ES2020",        // output JS version
    "module": "CommonJS",      // module system
    "strict": true,            // enable all strict checks
    "outDir": "./dist",        // compiled output folder
    "rootDir": "./src"         // source folder
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Basic Types

```typescript
// Primitives
let name: string = "Alice";
let age: number = 30;
let isActive: boolean = true;
let nothing: null = null;
let undef: undefined = undefined;
let big: bigint = 100n;
let sym: symbol = Symbol("id");

// any — opt out of type checking (avoid when possible)
let data: any = "hello";
data = 42;          // ✅ no error
data = true;        // ✅ no error
data.foo.bar();     // ✅ no error — but crashes at runtime!

// unknown — safer version of any (must narrow before use)
let input: unknown = getUserInput();
input.toUpperCase();              // ❌ Error — must check first
if (typeof input === "string") {
  input.toUpperCase();            // ✅ safe now
}

// never — function that never returns
function throwError(msg: string): never {
  throw new Error(msg);
}
function infiniteLoop(): never {
  while (true) {}
}

// void — function with no return value
function log(msg: string): void {
  console.log(msg);
  // no return
}

// Arrays
let nums: number[] = [1, 2, 3];
let strs: string[] = ["a", "b"];
let mixed: (string | number)[] = ["a", 1, "b", 2];

// Tuple — fixed length, fixed types
let tuple: [string, number] = ["Alice", 30];
let named: [name: string, age: number] = ["Alice", 30];  // named tuple
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Type Inference

TypeScript figures out the type automatically — you don't always need to annotate.

```typescript
// Inferred as string
let name = "Alice";
name = 42;  // ❌ Error: Type 'number' is not assignable to type 'string'

// Inferred as number
let age = 30;

// Inferred as boolean
let active = true;

// Inferred return type — number
function add(a: number, b: number) {
  return a + b;  // return type inferred as number
}

// Inferred as string[]
let fruits = ["apple", "banana", "mango"];

// When to annotate explicitly:
let value: string | number;   // can't infer — not initialized
value = "hello";
value = 42;

// Function params — always annotate (can't be inferred)
function greet(name: string) {   // ← must annotate
  return `Hello ${name}`;
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Type Aliases & Interfaces + Differences

```typescript
// --- TYPE ALIAS ---
type ID = string | number;
type Point = { x: number; y: number };
type Callback = (err: Error | null, result: string) => void;
type StringOrNumber = string | number;   // unions — only possible with type

// --- INTERFACE ---
interface User {
  id: number;
  name: string;
  email?: string;          // optional
  readonly role: string;   // cannot change after creation
}

// Extending interface
interface Admin extends User {
  permissions: string[];
}

// Extending type alias
type AdminType = User & { permissions: string[] };
```

**Differences — when to use which:**

| Feature | `interface` | `type` |
|---|---|---|
| Object shapes | ✓ | ✓ |
| Primitives & unions | ✗ | ✓ |
| Declaration merging | ✓ | ✗ |
| Extends keyword | ✓ | via `&` |
| Implements in class | ✓ | ✓ |
| Computed properties | ✗ | ✓ |

```typescript
// Declaration merging — only interface can do this
interface Window { title: string; }
interface Window { theme: string; }
// Window now has both: title & theme ✅

// Union type — only type alias can do this
type Result = "success" | "error" | "pending";  // ✅
interface Result = "success" | "error";          // ❌ not possible
```

> **Rule of thumb:** Use `interface` for objects & classes. Use `type` for unions, primitives, and complex compositions.

[⬆ Back to Table of Contents](#table-of-contents)

---

### Union & Intersection Types

```typescript
// --- UNION (|) — one of these types ---
type ID = string | number;
type Status = "active" | "inactive" | "banned";

let id: ID = "abc123";
id = 42;           // ✅ both valid

function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}

// --- INTERSECTION (&) — all of these types combined ---
interface HasName { name: string; }
interface HasAge  { age: number; }

type Person = HasName & HasAge;
// Person = { name: string; age: number }

const person: Person = { name: "Alice", age: 30 };  // must have both

// Combining interfaces
interface Employee extends HasName {
  company: string;
}
type EmployeeWithAge = Employee & HasAge;
// { name: string; company: string; age: number }
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 2 — ⚙️ Functions & Objects (30 min)

### Function Types

```typescript
// Basic function
function add(a: number, b: number): number {
  return a + b;
}

// Arrow function
const multiply = (a: number, b: number): number => a * b;

// Optional parameter
function greet(name: string, greeting?: string): string {
  return `${greeting ?? "Hello"}, ${name}`;
}
greet("Alice");           // ✅ "Hello, Alice"
greet("Alice", "Hi");     // ✅ "Hi, Alice"

// Default parameter
function greet2(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}`;
}

// Rest parameters
function sum(...nums: number[]): number {
  return nums.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4, 5);  // 15

// Function type as variable
let operation: (a: number, b: number) => number;
operation = add;
operation = multiply;

// Function type alias
type MathFn = (a: number, b: number) => number;
const divide: MathFn = (a, b) => a / b;

// Void vs never return
function logMessage(msg: string): void { console.log(msg); }
function fail(msg: string): never { throw new Error(msg); }
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Function Overloads

Define multiple signatures for the same function — useful when return type depends on input type.

```typescript
// Overload signatures (no implementation)
function format(value: string): string;
function format(value: number): string;
function format(value: Date): string;

// Implementation signature (handles all cases)
function format(value: string | number | Date): string {
  if (typeof value === "string") return value.toUpperCase();
  if (typeof value === "number") return value.toFixed(2);
  return value.toISOString();
}

format("hello");      // ✅ "HELLO"
format(3.14159);      // ✅ "3.14"
format(new Date());   // ✅ "2024-01-01T..."
format(true);         // ❌ Error — no matching overload

// Overload with different return types
function parse(input: string): number;
function parse(input: number): string;
function parse(input: string | number): string | number {
  if (typeof input === "string") return parseInt(input);
  return input.toString();
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Object Types

```typescript
// Inline object type
function printUser(user: { name: string; age: number }): void {
  console.log(`${user.name} is ${user.age}`);
}

// Optional properties
interface Config {
  host: string;
  port?: number;         // optional
  readonly apiKey: string; // cannot reassign
}

const config: Config = { host: "localhost", apiKey: "abc" };
config.host = "prod.server.com";  // ✅
config.apiKey = "xyz";            // ❌ Error: readonly

// Index signatures — dynamic keys
interface StringMap {
  [key: string]: string;
}
const headers: StringMap = {
  "Content-Type": "application/json",
  "Authorization": "Bearer token"
};

// Mixed — known + dynamic keys
interface FlexibleObj {
  id: number;           // known key
  [key: string]: unknown; // dynamic keys — must be compatible
}

// Nested objects
interface Address {
  street: string;
  city: string;
  zip: string;
}
interface Person {
  name: string;
  address: Address;
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Destructuring with Types

```typescript
// Object destructuring
const { name, age }: { name: string; age: number } = user;

// With interface
interface Point { x: number; y: number; }
const { x, y }: Point = { x: 10, y: 20 };

// Rename while destructuring
const { name: userName, age: userAge }: User = user;

// Default values
const { host = "localhost", port = 3000 }: Config = config;

// Array destructuring
const [first, second]: [string, number] = ["Alice", 30];
const [head, ...tail]: number[] = [1, 2, 3, 4];

// Function parameter destructuring
function display({ name, age }: User): void {
  console.log(`${name} — ${age}`);
}

// Nested destructuring
function getCity({ address: { city } }: Person): string {
  return city;
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### keyof, typeof, Indexed Access T[K]

```typescript
// --- keyof — get union of keys of a type ---
interface User {
  id: number;
  name: string;
  email: string;
}

type UserKeys = keyof User;  // "id" | "name" | "email"

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user: User = { id: 1, name: "Alice", email: "a@b.com" };
getProperty(user, "name");   // ✅ returns string
getProperty(user, "foo");    // ❌ Error — "foo" not in User

// --- typeof — get type of a value ---
const config = { host: "localhost", port: 3000 };
type ConfigType = typeof config;  // { host: string; port: number }

// Use typeof for function signatures
function add(a: number, b: number) { return a + b; }
type AddFn = typeof add;  // (a: number, b: number) => number

// --- Indexed Access T[K] — get type of a property ---
type NameType = User["name"];   // string
type IdType = User["id"];       // number

// Union of value types
type UserValues = User[keyof User];  // string | number

// Array element type
type Fruits = string[];
type Fruit = Fruits[number];  // string

// Nested access
interface Company {
  employees: { name: string; role: string }[];
}
type Employee = Company["employees"][number];  // { name: string; role: string }
type Role = Company["employees"][number]["role"];  // string
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 3 — 🏛️ Classes (20 min)

### Class Basics

```typescript
class Person {
  // Properties must be declared
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hi, I'm ${this.name}`;
  }

  // Getter & Setter
  get info(): string {
    return `${this.name} (${this.age})`;
  }

  set setAge(age: number) {
    if (age < 0) throw new Error("Age cannot be negative");
    this.age = age;
  }

  // Static — belongs to class, not instance
  static species = "Homo Sapiens";
  static create(name: string, age: number): Person {
    return new Person(name, age);
  }
}

const alice = new Person("Alice", 30);
alice.greet();              // "Hi, I'm Alice"
alice.info;                 // "Alice (30)"
alice.setAge = 31;          // uses setter
Person.species;             // "Homo Sapiens"
Person.create("Bob", 25);   // static factory
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Access Modifiers

```typescript
class BankAccount {
  public owner: string;         // accessible everywhere (default)
  private balance: number;      // only inside this class
  protected accountType: string; // inside this class + subclasses
  readonly id: string;          // set once, never change

  constructor(owner: string, balance: number) {
    this.owner = owner;
    this.balance = balance;
    this.accountType = "savings";
    this.id = Math.random().toString(36);
  }

  // Shorthand — declare + assign in constructor
  // same as above but cleaner:
}

class BankAccountShort {
  readonly id: string = Math.random().toString(36);

  constructor(
    public owner: string,
    private balance: number,
    protected accountType: string = "savings"
  ) {}

  getBalance(): number {
    return this.balance;  // ✅ private — accessible inside class
  }
}

class SavingsAccount extends BankAccountShort {
  showType(): string {
    return this.accountType;  // ✅ protected — accessible in subclass
    // this.balance            // ❌ private — NOT accessible in subclass
  }
}

const acc = new BankAccountShort("Alice", 1000);
acc.owner;       // ✅ public
acc.getBalance(); // ✅ method is public
// acc.balance   // ❌ Error: private
// acc.id = "x"  // ❌ Error: readonly
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Inheritance — extends, super

```typescript
class Animal {
  constructor(public name: string) {}

  speak(): string {
    return `${this.name} makes a sound`;
  }

  move(distance: number = 0): void {
    console.log(`${this.name} moved ${distance}m`);
  }
}

class Dog extends Animal {
  constructor(name: string, public breed: string) {
    super(name);  // must call super() before using 'this'
  }

  // Override parent method
  speak(): string {
    return `${this.name} barks`;
  }

  // Extend parent method
  move(distance: number = 5): void {
    console.log("Running...");
    super.move(distance);  // call parent's move
  }
}

const dog = new Dog("Rex", "Labrador");
dog.speak();      // "Rex barks"
dog.move(10);     // "Running..." then "Rex moved 10m"
dog instanceof Dog;     // true
dog instanceof Animal;  // true
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Abstract Classes

Abstract classes cannot be instantiated — they exist to be extended.

```typescript
abstract class Shape {
  color: string;

  constructor(color: string) {
    this.color = color;
  }

  // Abstract method — subclass MUST implement
  abstract area(): number;
  abstract perimeter(): number;

  // Concrete method — shared by all subclasses
  describe(): string {
    return `A ${this.color} shape with area ${this.area().toFixed(2)}`;
  }
}

class Circle extends Shape {
  constructor(color: string, private radius: number) {
    super(color);
  }

  area(): number { return Math.PI * this.radius ** 2; }
  perimeter(): number { return 2 * Math.PI * this.radius; }
}

class Rectangle extends Shape {
  constructor(color: string, private w: number, private h: number) {
    super(color);
  }

  area(): number { return this.w * this.h; }
  perimeter(): number { return 2 * (this.w + this.h); }
}

// new Shape("red")  // ❌ Error: Cannot instantiate abstract class
const c = new Circle("red", 5);
c.describe();  // "A red shape with area 78.54"
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Implementing Interfaces

```typescript
interface Printable {
  print(): void;
}

interface Serializable {
  serialize(): string;
  deserialize(data: string): void;
}

interface Loggable {
  log(message: string): void;
}

// Class can implement multiple interfaces
class Document implements Printable, Serializable, Loggable {
  constructor(private content: string) {}

  print(): void {
    console.log(this.content);
  }

  serialize(): string {
    return JSON.stringify({ content: this.content });
  }

  deserialize(data: string): void {
    this.content = JSON.parse(data).content;
  }

  log(message: string): void {
    console.log(`[LOG] ${message}`);
  }
}

// Interface can describe a class shape
interface UserConstructor {
  new (name: string, age: number): User;
}

// Abstract class + interface together
abstract class Base implements Loggable {
  log(message: string): void {  // implements interface
    console.log(`[${new Date().toISOString()}] ${message}`);
  }
  abstract process(): void;     // subclass must implement
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 4 — 🧬 Generics (30 min)

### Generic Functions

```typescript
// Without generics — loses type info
function identity(arg: any): any { return arg; }

// With generics — preserves type info
function identity<T>(arg: T): T { return arg; }

identity<string>("hello");  // explicit
identity("hello");          // inferred — T = string
identity(42);               // inferred — T = number

// Multiple type parameters
function pair<A, B>(first: A, second: B): [A, B] {
  return [first, second];
}
pair("Alice", 30);   // [string, number]
pair(true, "yes");   // [boolean, string]

// Generic with array
function first<T>(arr: T[]): T | undefined {
  return arr[0];
}
first([1, 2, 3]);      // number | undefined
first(["a", "b"]);     // string | undefined

// Generic with multiple uses
function map<T, U>(arr: T[], fn: (item: T) => U): U[] {
  return arr.map(fn);
}
map([1, 2, 3], n => n.toString());  // string[]
map(["a", "b"], s => s.length);     // number[]
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Generic Interfaces & Type Aliases

```typescript
// Generic interface
interface Box<T> {
  value: T;
  getValue(): T;
}

interface Pair<A, B> {
  first: A;
  second: B;
  swap(): Pair<B, A>;
}

interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
  timestamp: Date;
}

// Usage
const numBox: Box<number> = {
  value: 42,
  getValue() { return this.value; }
};

const response: ApiResponse<User[]> = {
  data: users,
  status: 200,
  message: "OK",
  timestamp: new Date()
};

// Generic type aliases
type Nullable<T> = T | null;
type Maybe<T> = T | null | undefined;
type List<T> = T[];
type Dictionary<V> = Record<string, V>;
type Callback<T> = (err: Error | null, result: T) => void;

// Nested generics
type Tree<T> = {
  value: T;
  children: Tree<T>[];
};
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Generic Classes

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  get size(): number {
    return this.items.length;
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }
}

const numStack = new Stack<number>();
numStack.push(1);
numStack.push(2);
numStack.pop();   // 2

const strStack = new Stack<string>();
strStack.push("hello");

// Generic class with multiple params
class KeyValueStore<K, V> {
  private store = new Map<K, V>();

  set(key: K, value: V): void { this.store.set(key, value); }
  get(key: K): V | undefined { return this.store.get(key); }
  has(key: K): boolean { return this.store.has(key); }
}

const store = new KeyValueStore<string, number>();
store.set("age", 30);
store.get("age");  // number | undefined
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Constraints — extends, keyof

```typescript
// extends constraint — T must have this shape
function getLength<T extends { length: number }>(arg: T): number {
  return arg.length;
}
getLength("hello");      // ✅ string has length
getLength([1, 2, 3]);    // ✅ array has length
getLength(42);           // ❌ number has no length

// keyof constraint — K must be a key of T
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
const user = { id: 1, name: "Alice", age: 30 };
getProperty(user, "name");   // ✅ "Alice" — string
getProperty(user, "id");     // ✅ 1 — number
getProperty(user, "foo");    // ❌ "foo" not in user

// Multiple constraints
function merge<T extends object, U extends object>(a: T, b: U): T & U {
  return { ...a, ...b };
}
merge({ name: "Alice" }, { age: 30 });  // { name: string; age: number }

// Constraint with interface
interface HasId { id: number; }
function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}

// Conditional constraint
type OnlyStrings<T> = T extends string ? T : never;
type Test = OnlyStrings<string | number | boolean>;  // string
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Default Generic Types

```typescript
// Default type parameter
interface Response<T = string> {
  data: T;
  status: number;
}

const r1: Response = { data: "hello", status: 200 };        // T defaults to string
const r2: Response<number> = { data: 42, status: 200 };     // T = number
const r3: Response<User[]> = { data: users, status: 200 };  // T = User[]

// Multiple params with defaults
interface Table<
  TData = unknown,
  TError = Error,
  TSelected = TData   // can reference previous param
> {
  data: TData | null;
  error: TError | null;
  selected: TSelected | null;
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Built-in Generics

These are pre-defined by TypeScript — you just pass your type as `T`.

```typescript
// Array<T>
const nums: Array<number> = [1, 2, 3];         // same as number[]
const strs: Array<string> = ["a", "b"];        // same as string[]

// Promise<T>
const p: Promise<string> = fetch("/api").then(r => r.text());
async function getUser(): Promise<User> {
  return await fetchUser();
}

// Map<K, V>
const map = new Map<string, number>();
map.set("age", 30);
map.get("age");   // number | undefined

// Set<T>
const set = new Set<string>();
set.add("hello");
set.has("hello");  // boolean

// Record<K, V>
const scores: Record<string, number> = { Alice: 95, Bob: 87 };

// ReadonlyArray<T>
const frozen: ReadonlyArray<number> = [1, 2, 3];
frozen.push(4);  // ❌ Error: readonly

// Partial / Required / Readonly (utility generics — covered in Block 6)
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 5 — 🔍 Type System Deep Dive (30 min)

### Type Narrowing

TypeScript narrows the type inside conditional blocks.

```typescript
// typeof narrowing
function process(val: string | number) {
  if (typeof val === "string") {
    val.toUpperCase();   // val is string here
  } else {
    val.toFixed(2);      // val is number here
  }
}

// instanceof narrowing
function handleError(err: Error | string) {
  if (err instanceof Error) {
    console.log(err.message);  // Error type
  } else {
    console.log(err);          // string type
  }
}

// in operator narrowing
interface Cat { meow(): void; }
interface Dog { bark(): void; }

function makeSound(pet: Cat | Dog) {
  if ("meow" in pet) {
    pet.meow();   // Cat
  } else {
    pet.bark();   // Dog
  }
}

// Truthiness narrowing
function printLength(val: string | null | undefined) {
  if (val) {
    val.length;   // string — null/undefined filtered out
  }
}

// Equality narrowing
function compare(a: string | number, b: string | boolean) {
  if (a === b) {
    a.toUpperCase();  // both must be string here
  }
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Discriminated Unions

Use a shared **literal property** to distinguish union members — TypeScript narrows automatically.

```typescript
type Shape =
  | { kind: "circle";    radius: number }
  | { kind: "square";    side: number }
  | { kind: "rectangle"; width: number; height: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;      // only radius available
    case "square":
      return shape.side ** 2;                  // only side available
    case "rectangle":
      return shape.width * shape.height;       // only width, height available
  }
}

// Exhaustiveness check with never
function assertNever(x: never): never {
  throw new Error("Unexpected value: " + x);
}

function area2(shape: Shape): number {
  switch (shape.kind) {
    case "circle":    return Math.PI * shape.radius ** 2;
    case "square":    return shape.side ** 2;
    case "rectangle": return shape.width * shape.height;
    default:          return assertNever(shape);  // ❌ Error if a case is missing
  }
}

// Real-world example — API responses
type ApiResult<T> =
  | { status: "success"; data: T }
  | { status: "error";   error: string }
  | { status: "loading" };

function handle<T>(result: ApiResult<T>) {
  switch (result.status) {
    case "success": return result.data;
    case "error":   return result.error;
    case "loading": return null;
  }
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Custom Type Guards — is keyword

```typescript
// Type predicate — "value is Type"
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function isUser(obj: unknown): obj is User {
  return (
    typeof obj === "object" &&
    obj !== null &&
    "id" in obj &&
    "name" in obj
  );
}

// Usage
function process(value: unknown) {
  if (isString(value)) {
    value.toUpperCase();  // ✅ TypeScript knows it's string
  }
  if (isUser(value)) {
    value.name;           // ✅ TypeScript knows it's User
  }
}

// Generic type guard
function isNonNull<T>(value: T | null | undefined): value is T {
  return value !== null && value !== undefined;
}

const items: (string | null)[] = ["a", null, "b", null, "c"];
const strings: string[] = items.filter(isNonNull);  // ✅ string[]
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Assertion Functions

```typescript
// Assert and narrow — throws if condition fails
function assert(condition: unknown, msg: string): asserts condition {
  if (!condition) throw new Error(msg);
}

function assertIsString(val: unknown): asserts val is string {
  if (typeof val !== "string") throw new Error("Not a string!");
}

function assertDefined<T>(val: T | null | undefined): asserts val is T {
  if (val == null) throw new Error("Value is null or undefined");
}

// Usage
function processInput(input: string | null) {
  assertDefined(input);
  input.toUpperCase();  // ✅ TypeScript knows input is string after assertion
}

let value: unknown = "hello";
assertIsString(value);
value.toUpperCase();  // ✅ narrowed to string
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Mapped Types

Transform every property in a type programmatically.

```typescript
// Syntax: { [K in keyof T]: NewType }

// Make all props optional (same as Partial<T>)
type MyPartial<T> = {
  [K in keyof T]?: T[K];
};

// Make all props readonly (same as Readonly<T>)
type MyReadonly<T> = {
  readonly [K in keyof T]: T[K];
};

// Make all props nullable
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

// Remove readonly modifier
type Mutable<T> = {
  -readonly [K in keyof T]: T[K];
};

// Remove optional modifier
type Concrete<T> = {
  [K in keyof T]-?: T[K];
};

// Transform values
type Stringify<T> = {
  [K in keyof T]: string;
};

// Remap keys with 'as'
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

interface User { id: number; name: string; }
type UserGetters = Getters<User>;
// { getId: () => number; getName: () => string }
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Conditional Types + infer

```typescript
// Basic conditional type — like a ternary for types
type IsString<T> = T extends string ? true : false;
type A = IsString<string>;   // true
type B = IsString<number>;   // false

// infer — extract a type from inside another type
type UnpackArray<T> = T extends (infer U)[] ? U : T;
type C = UnpackArray<string[]>;   // string
type D = UnpackArray<number[]>;   // number
type E = UnpackArray<string>;     // string (not array — returns T itself)

type UnpackPromise<T> = T extends Promise<infer U> ? U : T;
type F = UnpackPromise<Promise<string>>;  // string
type G = UnpackPromise<number>;           // number

// Extract return type manually (same as ReturnType<T>)
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
type H = MyReturnType<() => string>;         // string
type I = MyReturnType<(n: number) => void>;  // void

// Extract first param
type FirstParam<T> = T extends (first: infer F, ...args: any[]) => any ? F : never;
type J = FirstParam<(name: string, age: number) => void>;  // string

// Distributive conditional types — applies to each union member
type ToArray<T> = T extends any ? T[] : never;
type K = ToArray<string | number>;  // string[] | number[]
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Template Literal Types

```typescript
// Combine string literal types
type Direction = "top" | "bottom" | "left" | "right";
type CSSProperty = `margin-${Direction}`;
// "margin-top" | "margin-bottom" | "margin-left" | "margin-right"

// Event names
type EventName = `on${Capitalize<string>}`;  // onAnyString

type HTMLEvents = "click" | "focus" | "blur";
type HTMLEventHandlers = `on${Capitalize<HTMLEvents>}`;
// "onClick" | "onFocus" | "onBlur"

// API routes
type Method = "GET" | "POST" | "PUT" | "DELETE";
type Route = "/users" | "/posts" | "/comments";
type Endpoint = `${Method} ${Route}`;
// "GET /users" | "GET /posts" | ... all combinations

// Getters & Setters generation
type PropNames = "name" | "age" | "email";
type Getters2 = `get${Capitalize<PropNames>}`;  // "getName" | "getAge" | "getEmail"
type Setters = `set${Capitalize<PropNames>}`;   // "setName" | "setAge" | "setEmail"

// With infer — parse template literals
type GetRouteParam<T extends string> =
  T extends `${string}:${infer Param}/${string}` ? Param :
  T extends `${string}:${infer Param}` ? Param :
  never;

type L = GetRouteParam<"/users/:id">;        // "id"
type M = GetRouteParam<"/posts/:id/comments">; // "id"
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 6 — 🛠️ Utility Types (15 min)

### Partial, Required, Readonly, Pick, Omit, Record

```typescript
interface Todo {
  id: number;
  title: string;
  description: string;
  done: boolean;
  createdAt: Date;
}

// Partial<T> — all props optional (great for update/patch operations)
type TodoUpdate = Partial<Todo>;
function updateTodo(id: number, update: Partial<Todo>) { /*...*/ }
updateTodo(1, { done: true });  // ✅ only pass what you want to update

// Required<T> — all props required (opposite of Partial)
type RequiredTodo = Required<Partial<Todo>>;  // back to all required

// Readonly<T> — all props readonly
type FrozenTodo = Readonly<Todo>;
const todo: FrozenTodo = { id: 1, title: "Buy milk", description: "", done: false, createdAt: new Date() };
todo.done = true;  // ❌ Error: readonly

// Pick<T, K> — keep only selected keys
type TodoPreview = Pick<Todo, "id" | "title" | "done">;
// { id: number; title: string; done: boolean }

// Omit<T, K> — exclude selected keys
type TodoWithoutDates = Omit<Todo, "createdAt">;
// { id: number; title: string; description: string; done: boolean }

// Record<K, V> — key-value map
type PageMap = Record<string, Todo[]>;
type ScoreBoard = Record<"Alice" | "Bob" | "Charlie", number>;
const scores: ScoreBoard = { Alice: 95, Bob: 87, Charlie: 92 };
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Exclude, Extract, NonNullable

```typescript
type T = "a" | "b" | "c" | "d";

// Exclude<T, U> — remove types from union
type WithoutAB = Exclude<T, "a" | "b">;  // "c" | "d"
type NoStrings = Exclude<string | number | boolean, string>;  // number | boolean

// Extract<T, U> — keep only matching types
type OnlyAB = Extract<T, "a" | "b">;  // "a" | "b"
type OnlyStrings = Extract<string | number | boolean, string>;  // string

// NonNullable<T> — remove null and undefined
type MaybeString = string | null | undefined;
type DefinitelyString = NonNullable<MaybeString>;  // string

// Real-world usage
type ApiStatus = "idle" | "loading" | "success" | "error";
type ActiveStatus = Exclude<ApiStatus, "idle">;  // "loading" | "success" | "error"

// NonNullable with generics
function compact<T>(arr: (T | null | undefined)[]): T[] {
  return arr.filter((x): x is T => x != null);
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### ReturnType, Parameters, InstanceType, Awaited

```typescript
// ReturnType<T> — extract return type of a function
function getUser() { return { id: 1, name: "Alice" }; }
type UserShape = ReturnType<typeof getUser>;  // { id: number; name: string }

async function fetchUsers() { return [] as User[]; }
type FetchResult = ReturnType<typeof fetchUsers>;  // Promise<User[]>

// Parameters<T> — extract param types as tuple
function createUser(name: string, age: number, role: string) {}
type CreateUserParams = Parameters<typeof createUser>;  // [string, number, string]
type FirstParam = Parameters<typeof createUser>[0];     // string

// InstanceType<T> — get instance type of a class constructor
class Database {
  query(sql: string): unknown[] { return []; }
}
type DBInstance = InstanceType<typeof Database>;  // Database

// Awaited<T> — unwrap Promise (works recursively)
type A = Awaited<Promise<string>>;                    // string
type B = Awaited<Promise<Promise<number>>>;           // number
type C = Awaited<ReturnType<typeof fetchUsers>>;      // User[]

// Combining utility types — real patterns
type AsyncFn = (...args: any[]) => Promise<any>;
type UnwrapAsync<T extends AsyncFn> = Awaited<ReturnType<T>>;

type D = UnwrapAsync<typeof fetchUsers>;  // User[]
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 7 — 🌍 Real World (25 min)

### Module System

```typescript
// --- NAMED EXPORTS ---
export interface Config { debug: boolean; port: number; }
export type ID = string | number;
export const VERSION = "1.0.0";
export function helper() {}
export class App {}

// --- DEFAULT EXPORT ---
export default class Server {}

// --- IMPORT ---
import Server from "./server";                    // default
import { Config, helper, VERSION } from "./app";  // named
import { Config as AppConfig } from "./app";      // renamed
import * as Utils from "./utils";                 // namespace import

// --- TYPE-ONLY IMPORT (no runtime cost) ---
import type { Config } from "./types";
import type { User } from "./models";

// --- RE-EXPORTS ---
export { helper as util } from "./utils";
export * from "./types";
export * as Models from "./models";

// --- DYNAMIC IMPORT ---
async function loadModule() {
  const { helper } = await import("./utils");
  helper();
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### tsconfig Must-Know Options

```json
{
  "compilerOptions": {
    // Output target
    "target": "ES2020",           // JS version to compile to
    "module": "ESNext",           // module system (ESNext, CommonJS)
    "moduleResolution": "bundler", // how to resolve imports

    // Strict checks — always enable these
    "strict": true,               // enables all strict options below
    "strictNullChecks": true,     // null/undefined are not assignable to other types
    "noImplicitAny": true,        // error on implicit 'any'
    "strictFunctionTypes": true,  // strict function type checking

    // Extra safety
    "noUnusedLocals": true,       // error on unused variables
    "noUnusedParameters": true,   // error on unused params
    "noImplicitReturns": true,    // all code paths must return
    "noFallthroughCasesInSwitch": true,

    // Paths
    "outDir": "./dist",           // compiled output
    "rootDir": "./src",           // source root
    "baseUrl": ".",               // base for path aliases
    "paths": {
      "@/*": ["src/*"],           // @/utils → src/utils
      "@components/*": ["src/components/*"]
    },

    // Interop
    "esModuleInterop": true,      // allow default imports from CommonJS
    "allowSyntheticDefaultImports": true,
    "skipLibCheck": true,         // skip type checking of .d.ts files

    // Output extras
    "declaration": true,          // generate .d.ts files
    "sourceMap": true,            // generate source maps
    "experimentalDecorators": true // enable decorators
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Common Patterns

```typescript
// --- NON-NULL ASSERTION (!) ---
// Tell TS: "trust me, this is not null"
const el = document.getElementById("app")!;
el.innerHTML = "Hello";  // ✅ no error

// --- TYPE ASSERTION ---
const input = event.target as HTMLInputElement;
const num = someValue as number;

// Double assertion (when direct assertion fails)
const x = (value as unknown) as SpecificType;

// --- CONST ASSERTION ---
const config = { host: "localhost", port: 3000 } as const;
// config.host is now "localhost" (literal), not string
config.host = "other";  // ❌ Error: readonly

const routes = ["home", "about", "contact"] as const;
type Route = typeof routes[number];  // "home" | "about" | "contact"

// --- SATISFIES OPERATOR (TS 4.9+) ---
// Validates type without widening
type Colors = Record<string, [number, number, number] | string>;
const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
  blue: [0, 0, 255]
} satisfies Colors;

palette.red;        // still [number, number, number] (not widened to string)
palette.green;      // still string

// --- OPTIONAL CHAINING & NULLISH COALESCING ---
const city = user?.address?.city ?? "Unknown";
const len = str?.length ?? 0;

// --- OBJECT SPREAD WITH TYPES ---
const updated: User = { ...existing, name: "New Name" };
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Error Handling with Types

```typescript
// Basic typed error handling
function divide(a: number, b: number): number {
  if (b === 0) throw new Error("Division by zero");
  return a / b;
}

try {
  divide(10, 0);
} catch (err) {
  // err is 'unknown' in strict mode (TS 4.0+)
  if (err instanceof Error) {
    console.log(err.message);  // ✅ safe
  }
}

// Result pattern — no thrown errors
type Result<T, E = Error> =
  | { success: true;  data: T }
  | { success: false; error: E };

function safeDivide(a: number, b: number): Result<number> {
  if (b === 0) return { success: false, error: new Error("Division by zero") };
  return { success: true, data: a / b };
}

const result = safeDivide(10, 2);
if (result.success) {
  result.data;    // number
} else {
  result.error;   // Error
}

// Custom error types
class ValidationError extends Error {
  constructor(public field: string, message: string) {
    super(message);
    this.name = "ValidationError";
  }
}

class NotFoundError extends Error {
  constructor(public resource: string, public id: number) {
    super(`${resource} with id ${id} not found`);
    this.name = "NotFoundError";
  }
}

type AppError = ValidationError | NotFoundError | Error;

function handleError(err: AppError) {
  if (err instanceof ValidationError) {
    console.log(`Validation failed on field: ${err.field}`);
  } else if (err instanceof NotFoundError) {
    console.log(`${err.resource} #${err.id} not found`);
  } else {
    console.log(`Unknown error: ${err.message}`);
  }
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Declaration Merging

TypeScript merges multiple declarations of the same name automatically.

```typescript
// --- INTERFACE MERGING ---
interface User { id: number; }
interface User { name: string; }
interface User { email?: string; }
// Result: { id: number; name: string; email?: string }

// Real-world use: extending third-party types
// Extend Express Request to add custom user property
interface Request {
  user?: AuthUser;
}

// --- NAMESPACE MERGING ---
function validate(value: string): boolean { return value.length > 0; }
namespace validate {
  export const MIN_LENGTH = 3;
  export function email(val: string): boolean { return val.includes("@"); }
}

validate("hello");          // calls function
validate.email("a@b.com");  // calls namespace member
validate.MIN_LENGTH;        // namespace constant

// --- MODULE AUGMENTATION ---
// Add types to an existing module
declare module "express" {
  interface Request {
    userId?: string;
  }
}
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Enums vs Literal Unions

```typescript
// --- ENUMS ---
enum Direction { Up, Down, Left, Right }   // 0, 1, 2, 3

enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Banned = "BANNED"
}

const enum Size { Small, Medium, Large }   // inlined at compile time

// Usage
let dir: Direction = Direction.Up;
if (dir === Direction.Down) { /*...*/ }

// Enum cons:
// - generates runtime JS object
// - can have numeric reverse mapping (confusing)
// - harder to tree-shake

// --- LITERAL UNIONS (preferred) ---
type DirectionLiteral = "up" | "down" | "left" | "right";
type StatusLiteral = "active" | "inactive" | "banned";

// ✅ No runtime overhead
// ✅ Works with string operations
// ✅ Easier to extend
// ✅ Better type narrowing

// When to use enums:
// - When you need reverse mapping
// - When working with external libs that expect enums
// - Flags with bitwise operations
const enum Permissions {
  Read = 1 << 0,    // 1
  Write = 1 << 1,   // 2
  Execute = 1 << 2, // 4
}
const userPerms = Permissions.Read | Permissions.Write;  // 3

// When to use literal unions (most cases):
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE" | "PATCH";
type LogLevel = "debug" | "info" | "warn" | "error";

function setLogLevel(level: LogLevel) { /*...*/ }
setLogLevel("info");   // ✅
setLogLevel("verbose");// ❌ Error — not in union
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 8 — 📌 Topics Not Covered

These are valid TypeScript features but either niche, legacy, or framework-specific. Worth knowing they exist:

- **Decorators** — `@Component`, `@Injectable` etc. used in Angular, NestJS. Requires `experimentalDecorators: true`
- **Namespaces** — legacy way to organize code before ES modules. Avoid in modern TS
- **DOM Types** — `HTMLElement`, `NodeList`, `EventTarget` etc. Straightforward once you know the basics
- **WeakMap / WeakRef / WeakSet types** — niche memory management APIs
- **Iterator & Generator types** — `Iterator<T>`, `Generator<T, TReturn, TNext>`, `AsyncGenerator<T>`
- **Triple-slash directives** — `/// <reference types="..." />` for older module resolution
- **Declaration files (.d.ts)** — writing type definitions for untyped JS libraries
- **Project references** — `tsc --build` for monorepos with multiple tsconfig files
- **Ambient declarations** — `declare module`, `declare global`, `declare var`
- **Variance annotations** — `in`, `out` modifiers on generic type params (TS 4.7+)
- **Using declarations** — `using` keyword for explicit resource management (TS 5.2+)
- **const type parameters** — `function fn<const T>()` (TS 5.0+)

[⬆ Back to Table of Contents](#table-of-contents)

---

*TypeScript version reference: 5.x | Estimated time: ~3 hours*
