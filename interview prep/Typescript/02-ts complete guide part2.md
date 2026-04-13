# TypeScript Complete Interview & Learning Guide
## Part 2: Categories 8–14 (Classes → Mapped Types)
### For React Developers Who Know JavaScript

---

# Table of Contents — Part 2

- [Category 8: Classes in TypeScript](#category-8-classes-in-typescript)
- [Category 9: Utility Types](#category-9-utility-types)
- [Category 10: Typing React Components](#category-10-typing-react-components)
- [Category 11: Typing Hooks](#category-11-typing-hooks)
- [Category 12: Event & DOM Typing](#category-12-event--dom-typing)
- [Category 13: Data Fetching & API Typing](#category-13-data-fetching--api-typing)
- [Category 14: Mapped & Template Types](#category-14-mapped--template-types)
- [Quick Revision — Part 2 One-Liners](#quick-revision--part-2-one-liners)

---

# Category 8: Classes in TypeScript

---

### 🔴 8.1 Class Property Types and Constructor Typing

**Explanation:** TypeScript requires class properties to be declared with types before use. All declared properties must be initialized in the constructor (or via property initializers) unless you use the `!` definite assignment assertion.

```typescript
class User {
  // Declare properties with their types
  id: number;
  name: string;
  email: string;
  createdAt: Date;

  constructor(id: number, name: string, email: string) {
    this.id        = id;
    this.name      = name;
    this.email     = email;
    this.createdAt = new Date();
  }

  greet(): string {
    return `Hello, I'm ${this.name}`;
  }
}

const user = new User(1, "Alice", "alice@example.com");
user.name;     // string ✅
user.notExist; // ❌ Property 'notExist' does not exist

// Property initializers — assign directly on the declaration
class Config {
  timeout: number  = 5000;   // initialized in place
  retries: number  = 3;
  baseUrl: string  = "https://api.example.com";
}

// Definite assignment assertion (!) — promise you'll assign before use
class LazyLoader {
  data!: User[]; // ! = "I know what I'm doing, this WILL be assigned"

  async init() {
    this.data = await fetchUsers(); // assigned in async init, not constructor
  }

  process() {
    this.data.forEach(u => console.log(u.name)); // ✅ (trust the !)
  }
}
```

**Interview Q: Why does TypeScript require property declarations in classes, and what is `!` used for?** 🔴
**A:** TypeScript's `strictPropertyInitialization` flag (part of `strict`) ensures every declared property is definitely assigned before use. If you can't assign in the constructor — for example, async setup — you use `!` (definite assignment assertion) to tell TypeScript "trust me, this will be assigned before I use it." Misusing `!` means a runtime crash if you access before assignment.

**Aha moment:** In JS you can silently add properties anywhere. In TS, class properties must be declared upfront — the class shape is explicit and readable at a glance.

**Common mistake:** Using `!` on properties you're not 100% certain will be assigned before first use. Every `!` is a promise — break it and you get a runtime crash with no TS warning.

**One-liner:** Declare class properties with types. Initialize in constructor or with `=`. Use `!` only when async init is certain.

---

### 🔴 8.2 Access Modifiers: public, private, protected

**Explanation:** TypeScript's access modifiers control which code can see class members. They're compile-time checks only — JavaScript output still exposes everything at runtime.

```typescript
class BankAccount {
  public owner: string;         // accessible anywhere (this is the default)
  private balance: number;      // only inside THIS class
  protected accountNumber: string; // inside this class AND subclasses

  constructor(owner: string, balance: number) {
    this.owner         = owner;
    this.balance       = balance;
    this.accountNumber = Math.random().toString(36).slice(2);
  }

  // Public method — part of the API
  public getBalance(): number {
    return this.balance; // ✅ private accessible inside the class
  }

  // Private helper — implementation detail
  private validateAmount(amount: number): boolean {
    return amount > 0 && amount <= this.balance;
  }

  public withdraw(amount: number): boolean {
    if (!this.validateAmount(amount)) return false; // ✅ private method OK here
    this.balance -= amount;
    return true;
  }
}

const acc = new BankAccount("Alice", 1000);
acc.owner;         // ✅ public
acc.balance;       // ❌ Error: private
acc.accountNumber; // ❌ Error: protected

// Subclass CAN access protected
class SavingsAccount extends BankAccount {
  showAccountNumber(): string {
    return this.accountNumber; // ✅ protected — accessible in subclass
  }
}

// For TRUE runtime privacy, use JS private fields (#)
class Modern {
  #reallyPrivate = 42;        // JS private — runtime enforced
  private tsPrivate = 42;     // TS private — compile-time only
}
const m = new Modern();
// (m as any).tsPrivate  → 42  (accessible at runtime! TS just warns)
// (m as any).#reallyPrivate → Error (actually private at runtime)
```

**Interview Q: What is the difference between TypeScript's `private` and JavaScript's `#private` fields?** 🔴
**A:** TypeScript's `private` is compile-time only — TypeScript prevents access in your code but the property is still fully accessible at runtime (you can reach it with `(obj as any).prop`). JavaScript's `#privateField` syntax (ES2022) enforces privacy at runtime — the property genuinely can't be accessed from outside the class even with tricks. Use `#` for true runtime encapsulation.

**Aha moment:** TypeScript `private` is for you and your team (compile-time guardrail). JavaScript `#private` is for everyone including your library users (runtime enforcement).

**Common mistake:** Thinking TypeScript `private` enforces runtime encapsulation. It doesn't — only `#private` does.

**One-liner:** `public` = anywhere. `private` = this class only. `protected` = this + subclasses. All compile-time only — use `#` for runtime privacy.

---

### 🔴 8.3 Parameter Properties Shorthand

**Explanation:** Adding an access modifier to a constructor parameter automatically declares AND assigns the class property. Eliminates boilerplate — the single most common TypeScript class pattern.

```typescript
// ── Without shorthand (verbose) ───────────────────────────
class User {
  public id: number;
  private name: string;
  protected email: string;
  readonly createdAt: Date;

  constructor(id: number, name: string, email: string) {
    this.id        = id;
    this.name      = name;
    this.email     = email;
    this.createdAt = new Date();
  }
}

// ── With parameter properties (concise — PREFERRED) ────────
class User {
  readonly createdAt = new Date(); // still declare manually if needed

  constructor(
    public id: number,
    private name: string,
    protected email: string,
  ) {
    // TypeScript automatically:
    // 1. Declares each as a property
    // 2. Assigns this.id = id, this.name = name, etc.
    // Constructor body can be completely empty!
  }
}

// Super common in Angular and NestJS DI
class UserService {
  constructor(
    private readonly userRepository: UserRepository,
    private readonly emailService: EmailService,
    private readonly logger: Logger,
  ) {}
  // All three injected and stored automatically!
}

// With defaults
class Config {
  constructor(
    public host: string = "localhost",
    public port: number = 8080,
  ) {}
}
const cfg = new Config();         // host: "localhost", port: 8080
const cfg2 = new Config("prod.example.com", 443);
```

**Interview Q: What are parameter properties in TypeScript and what do they save you from?** 🔴
**A:** Parameter properties let you declare AND initialize a class property with a single constructor parameter annotation. Adding `public`, `private`, `protected`, or `readonly` to a constructor parameter tells TypeScript to declare that property and assign it automatically. They eliminate the repetitive "declare at top, assign in constructor" boilerplate — very common in dependency injection patterns.

**Aha moment:** This is a TypeScript-only feature — the compiled JS still writes out full property assignments. It's purely syntactic sugar that saves significant boilerplate.

**Common mistake:** Mixing parameter properties and manually declared properties inconsistently. Pick one style per class.

**One-liner:** `constructor(private name: string)` = declares + assigns in one shot. Eliminates property boilerplate. Very common.

---

### 🔴 8.4 Implementing an Interface: implements

**Explanation:** `implements` is a compile-time check that a class satisfies an interface's contract. It provides no implementation — you still write all the methods yourself.

```typescript
interface Serializable {
  serialize(): string;
  deserialize(data: string): this;
}

interface Validatable {
  validate(): boolean;
  errors: string[];
}

// Class must implement ALL required members
class UserForm implements Serializable, Validatable {
  errors: string[] = []; // required by Validatable

  constructor(private data: Record<string, string>) {}

  serialize(): string {           // required by Serializable
    return JSON.stringify(this.data);
  }

  deserialize(json: string): this { // required by Serializable
    Object.assign(this.data, JSON.parse(json));
    return this;
  }

  validate(): boolean {            // required by Validatable
    this.errors = [];
    if (!this.data.name)  this.errors.push("Name is required");
    if (!this.data.email) this.errors.push("Email is required");
    return this.errors.length === 0;
  }
}

// TypeScript errors if any required member is missing
class IncompleteForm implements Serializable {
  serialize(): string { return "{}"; }
  // ❌ Error: Class missing 'deserialize' required by Serializable
}
```

**Interview Q: What is the difference between `implements` and `extends` for classes?** 🔴

| | extends | implements |
|---|---|---|
| Inherits implementation | ✅ | ❌ |
| Type-checks the shape | ✅ | ✅ |
| How many | Only one | Multiple |
| Output code | Yes (prototype chain) | Nothing (erased) |
| Use for | Inheritance | Contracts / interfaces |

**A:** `extends` inherits implementation and creates a prototype chain. `implements` is only a compile-time type check — it verifies the class has all required members but provides no code at all. A class can extend only one class but implement multiple interfaces.

**Aha moment:** `implements` generates zero JavaScript output — it's purely a TypeScript safety check that disappears after compilation.

**Common mistake:** Expecting `implements` to provide method implementations. It only checks — you write all the code yourself.

**One-liner:** `implements` = compile-time contract check only. No code generated, no inheritance. Just "does this class have these methods?"

---

### 🔴 8.5 Abstract Classes and Abstract Methods

**Explanation:** Abstract classes can't be instantiated directly — they're base classes meant to be extended. Abstract methods define a signature with no implementation that subclasses MUST provide.

```typescript
abstract class Shape {
  // Abstract method — signature only, no body, MUST be implemented by subclasses
  abstract getArea(): number;
  abstract getPerimeter(): number;

  // Concrete method — shared implementation for all subclasses
  toString(): string {
    return `${this.constructor.name}: area=${this.getArea().toFixed(2)}`;
  }
}

class Circle extends Shape {
  constructor(private radius: number) { super(); }

  getArea(): number      { return Math.PI * this.radius ** 2; }    // ✅ implemented
  getPerimeter(): number { return 2 * Math.PI * this.radius; } // ✅ implemented
}

class Rectangle extends Shape {
  constructor(private w: number, private h: number) { super(); }

  getArea(): number      { return this.w * this.h; }           // ✅ implemented
  getPerimeter(): number { return 2 * (this.w + this.h); } // ✅ implemented
}

const c = new Circle(5);
console.log(c.toString()); // "Circle: area=78.54" — inherited concrete method

const s = new Shape(); // ❌ Error: Cannot create an instance of an abstract class

// If a subclass forgets an abstract method:
class Triangle extends Shape {
  getArea(): number { return 0; }
  // ❌ Error: Non-abstract class 'Triangle' doesn't implement 'getPerimeter'
}
```

**Interview Q: When would you use an abstract class instead of an interface?** 🔴
**A:** Use an abstract class when you have shared implementation to provide — concrete methods or properties that all subclasses should inherit. Use an interface when you only want a contract with no implementation. The practical rule: if subclasses should share code, use an abstract class. If they just need to satisfy a shape, use an interface (and a class can implement multiple interfaces but only extend one abstract class).

**Aha moment:** Abstract classes are the sweet spot between a fully concrete class (too specific) and an interface (no implementation) — they provide reusable shared behavior while mandating specific extensions.

**Common mistake:** Using an abstract class when an interface would suffice. If there's no shared implementation to provide, use a simpler interface.

**One-liner:** Abstract class = non-instantiable base with shared concrete methods + required abstract methods subclasses must implement.

---

### 🟡 8.6 Readonly and Static Class Members

**Explanation:** `readonly` on a class property prevents reassignment after construction. `static` creates class-level properties/methods shared across all instances — not per-instance.

```typescript
class AppConfig {
  // static — shared across all instances, access via class name
  static readonly VERSION = "1.0.0";
  static instanceCount   = 0;

  // readonly — set in constructor, never changed again
  readonly id: string;
  name: string;

  constructor(name: string) {
    AppConfig.instanceCount++; // update class-level counter
    this.id   = crypto.randomUUID(); // set once, readonly from here
    this.name = name;
  }

  static getVersion(): string {
    return AppConfig.VERSION; // access static from static method
  }
}

const a = new AppConfig("App A");
const b = new AppConfig("App B");

AppConfig.instanceCount; // 2 — shared class property
AppConfig.VERSION;       // "1.0.0"
a.id;                    // unique UUID — readonly, can't change
a.id = "new";            // ❌ Error: readonly property

// Static factory pattern (common in React context providers)
class Logger {
  private static instance: Logger | null = null;

  private constructor(private prefix: string) {} // private constructor!

  static create(prefix: string): Logger {
    if (!Logger.instance) {
      Logger.instance = new Logger(prefix);
    }
    return Logger.instance;
  }

  log(msg: string) { console.log(`[${this.prefix}] ${msg}`); }
}

const logger = Logger.create("App"); // must use factory — can't `new Logger`
```

**Interview Q: What is the difference between `readonly` and `static` in TypeScript classes?** 🟡
**A:** `readonly` is per-instance and prevents reassignment after construction — each instance has its own readonly copy. `static` is class-level — one shared value for all instances, accessed via the class name, not `this`. They can be combined: `static readonly VERSION = "1.0.0"` is a class-level constant.

**Aha moment:** Static properties in TypeScript classes are just properties on the constructor function — same as in JavaScript, TypeScript just type-checks them properly.

**Common mistake:** Accessing a `static` property via `this` inside a static method. Use the class name: `AppConfig.instanceCount`, not `this.instanceCount`.

**One-liner:** `readonly` = per-instance, immutable after construction. `static` = class-level, shared across instances, accessed via ClassName.

---

### 🟡 8.7 Type Compatibility — Structural Typing with Classes

**Explanation:** TypeScript uses structural typing for classes — if a class has the right shape (properties and methods), it satisfies an interface or other class type, even without explicit `implements`. This can be surprising coming from Java/C#.

```typescript
interface Point { x: number; y: number }

class CartesianPoint {
  constructor(public x: number, public y: number) {}
  // No 'implements Point' — but it satisfies the interface!
}

function plotPoint(p: Point) {
  console.log(`(${p.x}, ${p.y})`);
}

const cp = new CartesianPoint(3, 4);
plotPoint(cp); // ✅ TypeScript accepts it — shape matches!

// Even a plain object satisfies it
plotPoint({ x: 1, y: 2 }); // ✅ shape matches

// Classes are compatible if they have compatible shapes
class Cat {
  constructor(public name: string) {}
  meow() { return "Meow!"; }
}

class Dog {
  constructor(public name: string) {}
  meow() { return "Woof... I mean Meow!"; } // has same shape as Cat!
}

function makeCatSound(cat: Cat) { cat.meow(); }
makeCatSound(new Dog("Rex")); // ✅ Dog has same shape as Cat — TypeScript allows!
```

**Interview Q: What is structural typing and how does it affect TypeScript classes?** 🟡
**A:** TypeScript uses structural (duck) typing — compatibility is based on shape (which properties and methods a type has), not on explicit declarations. A class doesn't need `implements SomeInterface` to be compatible with it — if it has the right shape, TypeScript accepts it. This differs from nominal typing languages like Java/C# where `implements` declarations are required.

**Aha moment:** In TypeScript, "if it has the right properties and methods, it works." No explicit `implements` required. This is very different from Java/C#/Java-style OOP thinking.

**Common mistake:** Over-using `implements` when you just need a class to have a certain shape — TypeScript will accept it anyway if the shape matches.

**One-liner:** TypeScript uses structural typing — compatible shape = compatible type. `implements` isn't required for structural compatibility.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 9: Utility Types

---

### 🔴 9.1 Partial&lt;T&gt; — Make All Properties Optional

**Explanation:** `Partial<T>` creates a type where every property of T becomes optional. Essential for update operations, form state, and configuration objects where you only provide changed fields.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  role: "user" | "admin";
  createdAt: Date;
}

// Partial<User> = { id?: number; name?: string; email?: string; ... }

// ── Use case 1: update/patch functions ────────────────────
function updateUser(id: number, updates: Partial<User>): User {
  const existing = getUserById(id);
  return { ...existing, ...updates };
}

updateUser(1, { name: "Bob" });                  // ✅ only name
updateUser(1, { name: "Bob", role: "admin" });   // ✅ name + role
updateUser(1, { id: 999 });                      // ✅ even id (careful!)
updateUser(1, { role: "superuser" });            // ❌ "superuser" not valid role

// ── Use case 2: React form state that fills in gradually ──
const [formData, setFormData] = useState<Partial<User>>({});
// start empty, add fields as user types

// ── Use case 3: configuration with defaults ────────────────
interface Config {
  timeout: number;
  retries: number;
  debug: boolean;
}

function createClient(options: Partial<Config> = {}): Config {
  const defaults: Config = { timeout: 5000, retries: 3, debug: false };
  return { ...defaults, ...options }; // caller can override any subset
}

// ── How Partial is implemented (you can do this yourself!) ─
type MyPartial<T> = {
  [K in keyof T]?: T[K]; // ? makes every property optional
};
```

**Interview Q: What is `Partial<T>` and when would you use it vs the original type?** 🔴
**A:** `Partial<T>` makes all properties optional. Use the original type when ALL properties are required (e.g., creating a new entity). Use `Partial<T>` for update operations (only changed fields), form state (builds up gradually), and configuration objects with defaults. It's implemented as `{ [K in keyof T]?: T[K] }`.

**Aha moment:** `Partial<T>` is implemented with a mapped type — understanding mapped types means you could write `Partial` yourself from scratch.

**Common mistake:** Using `Partial<User>` everywhere including for creation. A new user needs all fields — use the full type. Partial is for UPDATES.

**One-liner:** `Partial<T>` = all properties optional. Use for update functions, form state, and config-with-defaults patterns.

---

### 🔴 9.2 Required&lt;T&gt; — All Properties Mandatory

**Explanation:** The opposite of `Partial` — `Required<T>` removes all `?` optional markers, making every property mandatory. Used after applying defaults to signal all values are now present.

```typescript
interface Config {
  timeout?: number;
  retries?: number;
  baseUrl?: string;
  apiKey?: string;
}

// After resolving defaults, everything is guaranteed present
type ResolvedConfig = Required<Config>;
// { timeout: number; retries: number; baseUrl: string; apiKey: string }

function resolveConfig(partial: Config): ResolvedConfig {
  return {
    timeout: partial.timeout  ?? 5000,
    retries: partial.retries  ?? 3,
    baseUrl: partial.baseUrl  ?? "https://api.example.com",
    apiKey:  partial.apiKey   ?? process.env.API_KEY ?? "",
  };
}

// Now callers know everything is defined — no optional chaining needed
const cfg = resolveConfig(userInput);
cfg.timeout; // number (not number | undefined!)

// Implementation (remove the ? modifier):
type MyRequired<T> = {
  [K in keyof T]-?: T[K]; // -? removes the optional marker
};
```

**Interview Q: When would you use `Required<T>`?** 🔴
**A:** When you've received optional input, applied defaults, and want the type to reflect that all values are now present. A common pattern: `function configure(opts: Partial<Config>): Required<Config>` — callers provide only what they want to override, but the return type signals everything is guaranteed.

**Aha moment:** `Required<T>` uses `-?` in its mapped type — the `-` removes the `?` modifier. TypeScript lets you add and remove type modifiers this way.

**Common mistake:** Using `Required<T>` thinking it will fill in values — it only changes the type. You still need to actually provide values (usually with `??` defaults).

**One-liner:** `Required<T>` = all properties mandatory (removes `?`). Use after applying defaults to signal completeness.

---

### 🔴 9.3 Readonly&lt;T&gt; — All Properties Read-Only

**Explanation:** `Readonly<T>` makes all properties of T read-only at the type level — prevents reassignment. Shallow only: nested objects can still be mutated.

```typescript
interface User { id: number; name: string; scores: number[] }

const user: Readonly<User> = { id: 1, name: "Alice", scores: [1, 2, 3] };

user.name = "Bob";       // ❌ Error: read-only
user.id   = 2;           // ❌ Error: read-only
user.scores.push(4);     // ✅ push still works — Readonly is SHALLOW!
user.scores = [99];      // ❌ Error: the reference is read-only

// Deeper readonly for arrays
const immutableUser: Readonly<{ scores: readonly number[] }> = {
  scores: [1, 2, 3]
};
immutableUser.scores.push(4); // ❌ Error: readonly array

// React: props should be treated as readonly
// React's type system already enforces this:
function Component(props: Readonly<{ name: string }>) {
  props.name = "Bob"; // ❌ Error: read-only — correctly prevents prop mutation
}

// Readonly vs Object.freeze
const frozen  = Object.freeze({ x: 1 }); // runtime enforcement
const tsRO: Readonly<{x: number}> = { x: 1 }; // compile-time only
(tsRO as any).x = 2; // still works at runtime!
```

**Interview Q: What is the difference between `Readonly<T>` and `Object.freeze()`?** 🔴
**A:** `Readonly<T>` is compile-time only — TypeScript prevents reassignment in your code, but at runtime the object is still mutable. `Object.freeze()` works at runtime — the object genuinely can't be mutated. They're complementary: use both together for maximum safety. Both are shallow — nested objects remain mutable.

**Aha moment:** `Readonly<T>` protects you from accidentally mutating in TypeScript code. `Object.freeze()` protects at runtime. You need both for real immutability.

**Common mistake:** Thinking `Readonly<T>` prevents deep mutation. `readonly scores: number[]` prevents replacing the array but not `.push()` on it. Use `readonly number[]` for the array too.

**One-liner:** `Readonly<T>` = all properties read-only at compile time (shallow). Prevents reassignment, not deep mutation.

---

### 🔴 9.4 Pick&lt;T, K&gt; and Omit&lt;T, K&gt;

**Explanation:** `Pick<T, K>` creates a type with ONLY the specified keys from T. `Omit<T, K>` creates a type with all keys EXCEPT the specified ones. Complementary tools for shaping types for specific contexts.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  role: "user" | "admin";
  createdAt: Date;
}

// Pick — keep ONLY these keys
type PublicUser     = Pick<User, "id" | "name" | "email">;
// { id: number; name: string; email: string }

// Omit — remove THESE keys, keep everything else
type UserWithoutPassword = Omit<User, "password">;
// { id: number; name: string; email: string; role: ...; createdAt: Date }

// Real use: safe API responses (never send password!)
function getPublicProfile(user: User): PublicUser {
  const { id, name, email } = user;
  return { id, name, email };
}

// React: sub-components that need a subset of parent props
interface TableProps {
  data: User[];
  onEdit: (user: User) => void;
  onDelete: (id: number) => void;
  isLoading: boolean;
}

type RowActions = Pick<TableProps, "onEdit" | "onDelete">; // just the handlers

// Override an event handler signature (very common pattern)
type InputProps = Omit<React.InputHTMLAttributes<HTMLInputElement>, "onChange"> & {
  onChange: (value: string) => void; // cleaner API than native onChange event
};

// Decision rule: fewer keys to LIST = choose that one
// Keeping 3 from 10 → use Pick (list 3)
// Removing 1 from 10 → use Omit  (list 1)
```

**Interview Q: What is the difference between `Pick` and `Omit`, and when would you use each?** 🔴
**A:** `Pick<T, K>` keeps only the listed keys. `Omit<T, K>` removes the listed keys. Choose whichever requires listing fewer keys: keeping 2 of 10 properties → use `Pick` (list 2). Removing 1 of 10 → use `Omit` (list 1). A common React pattern: `Omit<HTMLInputAttributes, "onChange"> & { onChange: (value: string) => void }` to override a specific prop.

**Aha moment:** `Omit` is implemented using `Pick` + `Exclude` internally: `type Omit<T, K> = Pick<T, Exclude<keyof T, K>>`. They're built from the same primitives.

**Common mistake:** Using `Partial<User>` when you actually want `Omit<User, "password">`. They're very different: `Partial` makes properties optional; `Omit` removes them entirely.

**One-liner:** `Pick<T, K>` = keep listed keys. `Omit<T, K>` = remove listed keys. Choose whichever lists fewer keys.

---

### 🔴 9.5 Record&lt;K, V&gt; — Typed Object / Dictionary

**Explanation:** `Record<K, V>` creates an object type where keys are type K and values are type V. When K is a literal union, TypeScript requires ALL keys to be present and rejects unknown keys.

```typescript
// Record with string keys — dynamic dictionary
type UserMap    = Record<string, User>;    // any string key → User
type ScoreBoard = Record<string, number>;  // any string key → number

const scores: ScoreBoard = { alice: 95, bob: 87 };
scores["anyone"] = 100; // ✅ any string key works

// Record with literal union keys — EXHAUSTIVE lookup table
type Status = "active" | "inactive" | "pending";

const statusColors: Record<Status, string> = {
  active:   "#00FF00",
  inactive: "#888888",
  pending:  "#FFA500",
  // ❌ miss one key → TypeScript errors!
  // ❌ add "unknown" key → TypeScript errors!
};

// Safe lookup — no "might be undefined" warning
const color = statusColors["active"]; // string (TypeScript knows this key exists)

// React: icon/component mapping by type
type IconName = "home" | "search" | "settings" | "profile";

const IconComponents: Record<IconName, React.FC> = {
  home:     HomeIcon,
  search:   SearchIcon,
  settings: SettingsIcon,
  profile:  ProfileIcon,
};

// Usage — TypeScript validates the key
function renderIcon(name: IconName) {
  const Icon = IconComponents[name]; // React.FC — always defined
  return <Icon />;
}

// Implementation of Record (it's a mapped type):
// type Record<K extends keyof any, V> = { [P in K]: V };
```

**Interview Q: How does `Record<K, V>` differ from an index signature `{ [key: string]: V }`?** 🔴
**A:** Both create typed dictionaries. The key difference is what K can be: index signatures accept `string` or `number` only. `Record` accepts any union type as K — and when K is a literal union, TypeScript requires ALL keys to be present and rejects unknown ones. `Record<Status, string>` enforces exhaustiveness; `{ [key: string]: string }` doesn't.

**Aha moment:** `Record<Status, string>` with a literal union K is a compile-time-verified lookup table — you can't miss a key or add an invalid one.

**Common mistake:** Using `Record<string, T>` when you know the specific keys. Use a literal union for K to get exhaustiveness checking.

**One-liner:** `Record<K, V>` = typed object. With literal union K = exhaustive — all keys required, no unknown keys allowed.

---

### 🔴 9.6 Exclude&lt;T, U&gt; and Extract&lt;T, U&gt;

**Explanation:** `Exclude<T, U>` removes types matching U from union T (keep what doesn't match). `Extract<T, U>` keeps only types matching U from union T (keep what does match). They're inverses of each other.

```typescript
type All = string | number | boolean | null | undefined;

// Exclude — REMOVE matching types from the union
type NonNullish   = Exclude<All, null | undefined>;  // string | number | boolean
type Strings      = Exclude<All, number | boolean | null | undefined>; // string

// Extract — KEEP only matching types
type Nullish       = Extract<All, null | undefined>;  // null | undefined
type OnlyPrimitives = Extract<All, string | number>;  // string | number

// Practical: event name filtering
type AllEvents     = "click" | "focus" | "blur" | "mouseenter" | "keydown";
type MouseEvents   = Extract<AllEvents, "click" | "mouseenter">;
// = "click" | "mouseenter"
type NonMouse      = Exclude<AllEvents, "click" | "mouseenter">;
// = "focus" | "blur" | "keydown"

// Exclude between object types
type Shapes = Circle | Square | Triangle;
type NonCircle = Exclude<Shapes, Circle>; // Square | Triangle

// How they're built (both use conditional types):
// type Exclude<T, U> = T extends U ? never : T;
// type Extract<T, U> = T extends U ? T : never;
// Memory trick: Exclude REMOVES (keeps non-matching). Extract KEEPS (keeps matching).
```

**Interview Q: What is the difference between `Exclude` and `Extract`?** 🔴

| | `Exclude<T, U>` | `Extract<T, U>` |
|---|---|---|
| Action | Removes matching | Keeps matching |
| Think of as | Filter OUT | Filter IN |
| Result | Non-matching types | Matching types |

**A:** `Exclude<T, U>` removes from T any types assignable to U — keeps what doesn't match. `Extract<T, U>` keeps from T only types assignable to U — keeps what does match. They're implemented with distributive conditional types.

**Aha moment:** `NonNullable<T>` is just `Exclude<T, null | undefined>`. Understanding `Exclude` means you can implement `NonNullable` yourself.

**Common mistake:** Confusing which one removes and which keeps. Memory trick: **Ex**clude = **Ex**clude the matching types (remove them). **Ex**tract = **Ex**tract the matching types (keep them).

**One-liner:** `Exclude<T, U>` removes U-matching types. `Extract<T, U>` keeps U-matching types. `NonNullable<T>` = `Exclude<T, null | undefined>`.

---

### 🔴 9.7 NonNullable&lt;T&gt;

**Explanation:** `NonNullable<T>` removes `null` and `undefined` from a type. The most commonly used Exclude-based utility — especially valuable in generic functions.

```typescript
type MaybeUser = User | null | undefined;
type DefiniteUser = NonNullable<MaybeUser>; // User

// Generic function that guarantees non-null return
function ensureDefined<T>(value: T | null | undefined, name: string): NonNullable<T> {
  if (value == null) throw new Error(`${name} must be defined`);
  return value as NonNullable<T>;
}

const user = ensureDefined(maybeUser, "user"); // type: User (not User | null)

// React: after a null check, tell TypeScript it's clean
function requireUser(user: User | null): User {
  if (!user) throw new Error("User required");
  return user; // TypeScript already narrows here — NonNullable used in generic contexts
}

// Typed array filter (removing nulls)
const maybeUsers: (User | null)[] = [user1, null, user2, null];
const users: User[] = maybeUsers.filter((u): u is User => u !== null);
// Type guard needed — filter alone doesn't narrow

// Implementation:
// type NonNullable<T> = T extends null | undefined ? never : T;
// = Exclude<T, null | undefined>
```

**Interview Q: What is `NonNullable<T>` and when do you need it vs a runtime null check?** 🔴
**A:** `NonNullable<T>` removes `null` and `undefined` from a type. TypeScript's control flow usually narrows types automatically inside if-checks, so you mainly need `NonNullable` in generic contexts — as a return type annotation when you want to signal the caller "this function guarantees a non-null result." It doesn't do runtime checking — pair it with a real null check.

**Aha moment:** TypeScript's automatic narrowing handles most cases. `NonNullable` shines as a return type in generic functions where automatic narrowing doesn't apply.

**Common mistake:** Using `NonNullable` as a substitute for actual null checks. It's type-level only — you still need runtime logic.

**One-liner:** `NonNullable<T>` = remove null/undefined from type. Use as return type in generic functions to signal guaranteed non-null.

---

### 🔴 9.8 ReturnType&lt;T&gt; and Parameters&lt;T&gt;

**Explanation:** `ReturnType<T>` extracts the return type of a function. `Parameters<T>` extracts the parameter types as a tuple. Essential for typing wrappers and utilities that must match a function's signature.

```typescript
// ReturnType — extract what a function returns
function getUser() {
  return { id: 1, name: "Alice", email: "a@test.com" };
}
type User = ReturnType<typeof getUser>;
// { id: number; name: string; email: string }
// No need to define the type separately — function IS the source of truth!

// Parameters — extract function parameters as a tuple
function createUser(name: string, email: string, role: "user" | "admin") {
  return { name, email, role };
}
type CreateParams = Parameters<typeof createUser>;
// [name: string, email: string, role: "user" | "admin"]

type FirstParam = Parameters<typeof createUser>[0]; // string

// ── Practical: typing wrappers that MUST match the original ──
function withLogging<T extends (...args: any[]) => any>(fn: T) {
  return (...args: Parameters<T>): ReturnType<T> => {
    console.log("Calling:", fn.name);
    const result = fn(...args);
    console.log("Result:", result);
    return result;
  };
}

const loggedCreate = withLogging(createUser);
// loggedCreate has the EXACT same signature as createUser — guaranteed!

// Async: combine Awaited + ReturnType
async function fetchUser(id: number) {
  return { id, name: "Alice" };
}
type FetchResult    = ReturnType<typeof fetchUser>;  // Promise<{id: number; name: string}>
type ResolvedResult = Awaited<ReturnType<typeof fetchUser>>; // {id: number; name: string}
```

**Interview Q: What is `ReturnType<T>` and when is it more useful than a manual type definition?** 🔴
**A:** `ReturnType<T>` extracts what a function returns. It's more useful than a manual type when you want the function itself to be the single source of truth — if the function's return type changes, `ReturnType<typeof fn>` automatically updates, while a manually written type would go stale. Especially useful for third-party functions you don't control.

**Aha moment:** `ReturnType<typeof someFunction>` means you never need to manually re-type what a function returns. The function IS the type definition.

**Common mistake:** Using `ReturnType` on an async function and forgetting it returns `Promise<T>`. Use `Awaited<ReturnType<typeof fn>>` to get the resolved value type.

**One-liner:** `ReturnType<F>` = extract return type. `Parameters<F>` = extract params as tuple. Use for wrappers that must match a function's signature.

---

### 🟡 9.9 Awaited&lt;T&gt;

**Explanation:** `Awaited<T>` recursively unwraps `Promise` types to get the resolved value type. The type-level equivalent of `await`.

```typescript
type A = Awaited<Promise<string>>;           // string
type B = Awaited<Promise<Promise<number>>>; // number (recursive unwrap!)
type C = Awaited<string>;                   // string (not a Promise, passes through)

// Most common use: get resolved type from async functions
async function fetchUser(id: number) {
  const res = await fetch(`/api/users/${id}`);
  return res.json() as User;
}

type FetchReturn    = ReturnType<typeof fetchUser>;          // Promise<User>
type ResolvedUser   = Awaited<ReturnType<typeof fetchUser>>; // User ✅

// With Promise.all — each position resolved
async function loadAll() {
  const [users, products] = await Promise.all([
    fetchUsers(),   // Promise<User[]>
    fetchProducts() // Promise<Product[]>
  ]);
  // users: User[], products: Product[] — TypeScript knows each position!
}

type AllResolved = Awaited<ReturnType<typeof loadAll>>;
// [User[], Product[]] — both unwrapped
```

**Interview Q: What does `Awaited<T>` do and why do you need it with `ReturnType`?** 🟡
**A:** `Awaited<T>` unwraps Promise types recursively. When you use `ReturnType` on an async function, you get `Promise<T>` — the outer Promise wrapper. `Awaited<ReturnType<typeof fn>>` strips the Promise to give you `T` directly. This mirrors what `await` does at runtime, but at the type level.

**Aha moment:** `Awaited` is the type-level `await`. Just as `await fetchUser()` gives you `User`, `Awaited<ReturnType<typeof fetchUser>>` gives you the `User` type.

**Common mistake:** Using `ReturnType` on async functions and getting `Promise<User>` instead of `User`. Wrap with `Awaited<>` to unwrap the Promise.

**One-liner:** `Awaited<Promise<T>>` = T. The type-level await. Use with `ReturnType` on async functions to get the resolved type.

---

### 🟡 9.10 ConstructorParameters&lt;T&gt; and InstanceType&lt;T&gt;

**Explanation:** `ConstructorParameters<T>` extracts the constructor's parameter types. `InstanceType<T>` extracts the instance type that `new T()` produces. Useful when working with class constructors dynamically.

```typescript
class User {
  constructor(
    public name: string,
    public email: string,
    public role: "user" | "admin" = "user"
  ) {}
}

// Extract constructor params
type UserCtorParams = ConstructorParameters<typeof User>;
// [name: string, email: string, role?: "user" | "admin"]

// Extract instance type
type UserInstance = InstanceType<typeof User>;
// User (the instance type)

// Why this matters: factory functions that must match a constructor
function createInstance<T extends new (...args: any[]) => any>(
  Ctor: T,
  ...args: ConstructorParameters<T>
): InstanceType<T> {
  return new Ctor(...args);
}

const user = createInstance(User, "Alice", "alice@test.com");
// user: User — TypeScript knows it!
// Args are type-checked against User's constructor params ✅
```

**Interview Q: When would you use `ConstructorParameters<T>` and `InstanceType<T>`?** 🟡
**A:** When building generic factory functions or dependency injection systems that work with class constructors dynamically. `ConstructorParameters` lets you type-check arguments passed to a dynamically chosen constructor. `InstanceType` lets you type the return value. Together they enable fully type-safe class factories.

**Aha moment:** These are how Angular-style DI and test factory utilities are typed — they're less common in everyday React code but powerful in library/framework code.

**Common mistake:** Forgetting `typeof` — `ConstructorParameters<User>` errors (User is a value, not a type). Correct: `ConstructorParameters<typeof User>`.

**One-liner:** `ConstructorParameters<typeof Cls>` = constructor param types. `InstanceType<typeof Cls>` = instance type. Use for dynamic class factories.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 10: Typing React Components

---

### 🔴 10.1 Typing Props with an Interface

**Explanation:** Every React component's props should be typed. The convention is `ComponentNameProps` as an interface or type alias. TypeScript validates every prop at every call site — wrong types and missing required props are compile errors.

```typescript
// Define the props type
interface ButtonProps {
  label: string;              // required
  onClick: () => void;        // required callback
  disabled?: boolean;         // optional
  variant?: "primary" | "secondary" | "danger"; // optional literal union
  size?: "sm" | "md" | "lg"; // optional
}

// Use it in the function signature
function Button({
  label,
  onClick,
  disabled = false,         // default value for optional prop
  variant  = "primary",
  size     = "md",
}: ButtonProps) {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn--${variant} btn--${size}`}
    >
      {label}
    </button>
  );
}

// TypeScript validates every usage
<Button label="Save" onClick={handleSave} />                // ✅ minimal
<Button label="Delete" onClick={handleDelete} variant="danger" /> // ✅ with optional
<Button onClick={handleSave} />                             // ❌ label is required
<Button label={42} onClick={handleSave} />                  // ❌ label must be string
<Button label="x" onClick={handleSave} variant="huge" />    // ❌ "huge" not in union

// Arrow function component (equivalent)
const Alert: React.FC<AlertProps> = ({ type, message }) => { ... };
// Note: modern convention often avoids React.FC — plain function is cleaner
```

**Interview Q: Should you use `React.FC<Props>` or a plain typed function for components?** 🔴
**A:** Modern convention prefers plain typed functions over `React.FC`. `React.FC` previously added an implicit `children` prop (removed in React 18 types) and adds a layer of indirection. A plain function `function Button({ label }: ButtonProps)` is simpler, doesn't have these quirks, and is what most style guides (including TypeScript's own) recommend today.

**Aha moment:** Your TypeScript-typed React component is its own documentation — every caller immediately sees exactly what's required and what's optional from the type.

**Common mistake:** Using `React.FC` out of habit from older codebases. In React 18+ with modern TypeScript, prefer plain typed functions.

**One-liner:** Define `interface ButtonProps`, destructure in function signature. Plain typed function > `React.FC` in modern React.

---

### 🔴 10.2 JSX.Element vs ReactElement vs ReactNode

**Explanation:** Three related types for what components can return or accept as children. Knowing which to use prevents common type errors in React+TypeScript.

```typescript
// JSX.Element — a specific JSX output (what <div /> produces)
// = React.ReactElement<any, any>
function Title(): JSX.Element {
  return <h1>Hello</h1>;
}

// ReactElement — same as JSX.Element in modern React
function Title(): React.ReactElement {
  return <h1>Hello</h1>;
}

// Neither accepts null — causes errors for conditional renders:
function MaybeTitle(): JSX.Element {
  if (!show) return null; // ❌ Error: null not JSX.Element
}

// Fix 1: explicit union
function MaybeTitle(): JSX.Element | null {
  if (!show) return null; // ✅
  return <h1>Hello</h1>;
}

// Fix 2: let TypeScript infer (BEST — no annotation needed)
function MaybeTitle() {
  if (!show) return null; // ✅ inferred correctly
  return <h1>Hello</h1>;
}

// ReactNode — the most inclusive type (USE FOR children props)
// Can be: ReactElement | string | number | boolean | null | undefined | array
interface WrapperProps {
  children: React.ReactNode; // ✅ accepts ANYTHING React can render
}

// Wrong types for children:
interface BadProps {
  children: JSX.Element;     // ❌ doesn't accept strings, null, arrays
  children: React.ReactElement; // ❌ same problem
}

<Container>Hello</Container>            // string child — needs ReactNode ✅
<Container>{null}</Container>           // null child — needs ReactNode ✅
<Container>{items.map(i => <Li />)}</Container> // array — needs ReactNode ✅
```

**Interview Q: What is the correct type for the `children` prop in TypeScript?** 🔴
**A:** `React.ReactNode` — it's the most inclusive type, accepting React elements, strings, numbers, booleans, null, undefined, and arrays. `JSX.Element` and `ReactElement` are too restrictive — they reject text content (`<Button>Click me</Button>`), null (for conditional renders), and arrays. Always use `ReactNode` for children.

**Aha moment:** `children: JSX.Element` is the most common React TypeScript mistake. Text content like `"Click me"` is a string, not a `JSX.Element`. Use `ReactNode`.

**Common mistake:** Using `JSX.Element` for children and then being confused why `<Button>Click me</Button>` causes a type error.

**One-liner:** `ReactNode` = use for `children` (accepts everything). `JSX.Element` = too restrictive. Let TS infer component return types.

---

### 🔴 10.3 Optional vs Required Props

**Explanation:** Props without `?` are required — TypeScript errors at every call site where they're missing. Props with `?` can be omitted and should have defaults in destructuring.

```typescript
interface ModalProps {
  // Required — the component cannot function without these
  isOpen: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;

  // Optional — have sensible defaults
  size?:               "sm" | "md" | "lg" | "xl";
  closeOnOverlayClick?: boolean;
  showCloseButton?:    boolean;
}

function Modal({
  isOpen,
  onClose,
  title,
  children,
  size               = "md",    // default
  closeOnOverlayClick = true,   // default
  showCloseButton    = true,    // default
}: ModalProps) {
  if (!isOpen) return null;

  return (
    <div
      className={`modal modal--${size}`}
      onClick={closeOnOverlayClick ? onClose : undefined}
    >
      <div className="modal__content">
        <h2>{title}</h2>
        {showCloseButton && <button onClick={onClose}>✕</button>}
        {children}
      </div>
    </div>
  );
}

// TypeScript enforces required props at every call site
<Modal isOpen={true} onClose={handleClose} title="Confirm">
  Are you sure?
</Modal>

<Modal onClose={handleClose} title="Confirm"> // ❌ isOpen is required
  ...
</Modal>
```

**Interview Q: How does TypeScript enforce required vs optional component props?** 🔴
**A:** Required props (no `?`) must be provided at every JSX usage site — TypeScript errors if they're missing. Optional props (with `?`) are typed as `T | undefined` inside the component; default values in destructuring ensure they're always defined in the body. This makes component contracts explicit and prevents "forgot a required prop" runtime bugs.

**Aha moment:** TypeScript-typed React components are self-documenting. The interface shows exactly what a component needs — required vs optional is instantly clear.

**Common mistake:** Making everything optional to avoid TypeScript errors. If the component crashes without a prop, it should be required. Excessive optionality makes component APIs confusing.

**One-liner:** Props without `?` = required (TypeScript errors if missing). Props with `?` = optional (provide defaults in destructuring).

---

### 🔴 10.4 Generic Components

**Explanation:** React components can have type parameters when prop types depend on each other. This enables type-safe reusable components like lists, tables, and select inputs where the item type varies.

```typescript
// Generic List — item type flows through
interface ListProps<T> {
  items: T[];
  renderItem: (item: T, index: number) => React.ReactNode;
  keyExtractor: (item: T) => string | number;
  emptyMessage?: string;
}

function List<T>({
  items,
  renderItem,
  keyExtractor,
  emptyMessage = "No items",
}: ListProps<T>) {
  if (items.length === 0) return <p>{emptyMessage}</p>;
  return (
    <ul>
      {items.map((item, i) => (
        <li key={keyExtractor(item)}>{renderItem(item, i)}</li>
      ))}
    </ul>
  );
}

// TypeScript infers T from the items prop
<List
  items={users}                                    // T inferred as User
  renderItem={(user) => <span>{user.name}</span>} // user: User ✅
  keyExtractor={(user) => user.id}                 // user: User ✅
/>

<List
  items={["Alice", "Bob"]}                  // T inferred as string
  renderItem={(name) => <span>{name}</span>} // name: string ✅
  keyExtractor={(name) => name}
/>

// Arrow function in .tsx needs <T,> (trailing comma) to avoid JSX ambiguity
const List = <T,>({ items, renderItem, keyExtractor }: ListProps<T>) => { ... };
// OR: const List = <T extends object>({ ... }: ListProps<T>) => { ... };
```

**Interview Q: How do you create a generic React component, and what is the `.tsx` JSX ambiguity issue?** 🔴
**A:** Add a type parameter to the function: `function List<T>(props: ListProps<T>)`. TypeScript infers T from props. In `.tsx` files, `<T>` is ambiguous — the parser can't tell if it's a generic or JSX. Fix with a trailing comma `<T,>` or a constraint `<T extends object>`. Regular function syntax avoids this; arrow functions in `.tsx` have the issue.

**Aha moment:** A single `<Table>` component can be fully type-safe for User tables, Product tables, and Order tables — all from the same implementation.

**Common mistake:** Forgetting the trailing comma in `.tsx` arrow function generics. `<T>` in `.tsx` = JSX parsing ambiguity = cryptic error.

**One-liner:** `function List<T>({ items }: { items: T[] })` — TypeScript infers T from usage. In `.tsx` arrow functions use `<T,>`.

---

### 🔴 10.5 Extending HTML Element Props

**Explanation:** `React.ComponentProps<'element'>` or `React.HTMLAttributes<HTMLElement>` gives you all native HTML attributes. Extending your props from these lets users pass native attributes through to the underlying element.

```typescript
// ComponentProps extracts ALL native props for an element
type DivProps = React.ComponentProps<"div">;
// Includes: className, style, onClick, id, aria-*, data-*, tabIndex, etc.

// Pattern: extend native element props + add your own
interface CardProps extends React.ComponentProps<"div"> {
  title: string;
  subtitle?: string;
  isHighlighted?: boolean;
}

function Card({ title, subtitle, isHighlighted, className, ...rest }: CardProps) {
  return (
    <div
      {...rest}  // pass through ALL native div props (id, style, onClick, etc.)
      className={`card ${isHighlighted ? "card--highlighted" : ""} ${className ?? ""}`}
    >
      <h2>{title}</h2>
      {subtitle && <p>{subtitle}</p>}
    </div>
  );
}

// Users get the full native div API
<Card
  title="Hello"
  id="main-card"            // ✅ from ComponentProps<"div">
  style={{ padding: 16 }}   // ✅ from ComponentProps<"div">
  onClick={handleClick}     // ✅ from ComponentProps<"div">
  aria-label="main card"    // ✅ from ComponentProps<"div">
  isHighlighted             // ✅ your custom prop
/>

// For buttons — more specific
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  isLoading?: boolean;
  variant?: "primary" | "secondary";
}
```

**Interview Q: What is the difference between `React.ComponentProps<'div'>` and `React.HTMLAttributes<HTMLDivElement>`?** 🔴
**A:** `React.ComponentProps<'div'>` is the more modern approach — it uses TypeScript's generic inference to get props for any intrinsic element. `React.HTMLAttributes<HTMLDivElement>` is older but equivalent for HTML elements. `ButtonHTMLAttributes<HTMLButtonElement>` is more specific, including button-specific attrs like `type`, `form`, `disabled`. Use `ComponentProps<'tagname'>` for simplicity, or the specific `*HTMLAttributes<*>` for finer control.

**Aha moment:** This is the pattern behind every well-designed component library. Your custom Button works exactly like a native button — all attrs pass through — because of this extension pattern.

**Common mistake:** Extending the types but forgetting `{...rest}` on the element. Types say "these props are accepted" but you never pass them to the DOM element.

**One-liner:** `extends React.ComponentProps<'div'>` + `{...rest}` on the element = your component accepts ALL native HTML attrs automatically.

---

### 🟡 10.6 forwardRef with TypeScript

**Explanation:** `React.forwardRef` requires two explicit type parameters: the element type the ref will point to, and the component's props type. They must be in the correct order.

```typescript
// forwardRef<RefType, PropsType>
// Note the order: REF type first, PROPS type second

interface InputProps {
  label: string;
  placeholder?: string;
  disabled?: boolean;
}

const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({ label, placeholder, disabled }, ref) => {
    return (
      <div>
        <label>{label}</label>
        <input
          ref={ref}           // forwarded to native input
          placeholder={placeholder}
          disabled={disabled}
        />
      </div>
    );
  }
);

Input.displayName = "Input"; // Good practice with forwardRef (for DevTools)

// Usage — TypeScript knows the ref is HTMLInputElement
const inputRef = React.useRef<HTMLInputElement>(null);

<Input ref={inputRef} label="Email" placeholder="Enter email" />

// Accessing the ref
inputRef.current?.focus();   // ✅ TypeScript knows it's HTMLInputElement
inputRef.current?.select();  // ✅
```

**Interview Q: What are the two type parameters for `React.forwardRef` and what order do they go in?** 🟡
**A:** `forwardRef<ElementType, PropsType>` — element type first, props type second. The first generic is the type of the ref element (e.g., `HTMLInputElement`). The second is the component's props interface. TypeScript can't infer these from the function body, so both must be explicit. Inside the callback, `ref` is typed as `React.ForwardedRef<ElementType>`.

**Aha moment:** The order feels counterintuitive — you'd expect props first since the component's main concern is its props. But the ref type comes first in `forwardRef`'s definition.

**Common mistake:** Swapping the order — `forwardRef<InputProps, HTMLInputElement>` is wrong and TypeScript gives confusing errors.

**One-liner:** `forwardRef<HTMLInputElement, Props>` — ref element type first, props second. Add `displayName` for DevTools.

---

### 🟡 10.7 memo and lazy with TypeScript

**Explanation:** `React.memo` and `React.lazy` work with TypeScript with minimal extra typing. `memo` infers from the wrapped component; `lazy` needs the dynamic import to have a default export.

```typescript
// React.memo — TypeScript infers the props type from the component
interface UserCardProps {
  user: User;
  onSelect: (id: number) => void;
}

const UserCard = React.memo<UserCardProps>(({ user, onSelect }) => {
  return <div onClick={() => onSelect(user.id)}>{user.name}</div>;
});
// Or just wrap an existing component:
const MemoCard = React.memo(UserCard); // props type inferred automatically

// React.lazy — must use with Suspense, import must be default export
const LazyDashboard = React.lazy(() => import("./Dashboard"));
// TypeScript: import returns Promise<{ default: ComponentType<any> }>

function App() {
  return (
    <React.Suspense fallback={<div>Loading...</div>}>
      <LazyDashboard /> {/* props of LazyDashboard are typed if Dashboard exports types */}
    </React.Suspense>
  );
}

// Custom comparison in memo
const OptimizedCard = React.memo(
  UserCard,
  (prevProps: UserCardProps, nextProps: UserCardProps) => {
    return prevProps.user.id === nextProps.user.id; // true = skip re-render
  }
);
```

**Interview Q: How does TypeScript work with `React.memo`?** 🟡
**A:** `React.memo<Props>()` accepts an explicit type parameter, but TypeScript can usually infer the props type from the wrapped component. The return type of `React.memo(Component)` is `React.MemoExoticComponent<typeof Component>`, which preserves the original component's prop types. The memoized component has the same TypeScript interface as the original.

**Aha moment:** TypeScript's inference handles `React.memo` well — you rarely need to add type annotations beyond what the wrapped component already has.

**Common mistake:** Wrapping a component with `memo` and then writing overly permissive comparison functions. The `prevProps` and `nextProps` should be explicitly typed to prevent comparing wrong fields.

**One-liner:** `React.memo<Props>(Component)` or just `React.memo(Component)` — TypeScript infers props from the component. `lazy` = `lazy(() => import('./Component'))`.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 11: Typing Hooks

---

### 🔴 11.1 useState&lt;T&gt; — When to Annotate

**Explanation:** `useState` is generic — `useState<T>` sets the state type. TypeScript infers from the initial value when possible. Explicit annotation is required when the initial value doesn't represent all possible states.

```typescript
// ── TypeScript INFERS — no annotation needed ──────────────
const [count, setCount]   = useState(0);        // count: number
const [name, setName]     = useState("");        // name: string
const [isOpen, setIsOpen] = useState(false);     // isOpen: boolean

// ── MUST annotate — initial value doesn't show full type ──

// 1. State starts as null but becomes a value later
const [user, setUser] = useState<User | null>(null);
// Without annotation: inferred as null — TypeScript won't let you setUser(userObject)!

// 2. Empty array — TypeScript infers never[]
const [items, setItems] = useState<string[]>([]);
// Without annotation: inferred as never[] — can't push anything!

// 3. Specific union of states
const [status, setStatus] = useState<"idle" | "loading" | "error">("idle");

// 4. Discriminated union for complex state
type FormState =
  | { kind: "idle" }
  | { kind: "submitting" }
  | { kind: "success"; result: string }
  | { kind: "error"; message: string };

const [formState, setFormState] = useState<FormState>({ kind: "idle" });

// setCount's type: Dispatch<SetStateAction<number>>
// SetStateAction<T> = T | ((prev: T) => T) — supports both direct value and updater fn
setCount(5);              // ✅ direct value
setCount(c => c + 1);    // ✅ updater function
```

**Interview Q: When do you need to explicitly type `useState` and what is the most common mistake?** 🔴
**A:** TypeScript infers state type from the initial value. Explicitly annotate when: the state starts as null/undefined (`useState<User | null>(null)`), the array starts empty (`useState<string[]>([])`), or the state is a union that the initial value doesn't cover. The most common mistake: `useState(null)` — TypeScript infers the type as `null` permanently, making it impossible to ever set a non-null value.

**Aha moment:** `useState(null)` with no generic is almost always a bug — TypeScript thinks the state is and will always be `null`. Always write `useState<User | null>(null)`.

**Common mistake:** `const [user, setUser] = useState(null)` — user will be typed as `null` always. TypeScript won't let you call `setUser(fetchedUser)`.

**One-liner:** TS infers from initial value. Always annotate when starting null: `useState<User | null>(null)` or empty array: `useState<string[]>([])`.

---

### 🔴 11.2 useRef — The Three Different Forms

**Explanation:** `useRef` has three distinct TypeScript forms depending on the use case. Getting this wrong is one of the most common sources of TypeScript errors in React.

```typescript
// ── FORM 1: DOM ref — attach to a DOM element ──────────────
// Pass null as initial → TypeScript gives you RefObject<T>
// RefObject<T>.current is: T | null (null until element mounts)
const inputRef = useRef<HTMLInputElement>(null);

<input ref={inputRef} /> // ✅ attaches to the DOM element

// Access — must handle null
inputRef.current?.focus();   // safe: optional chaining
inputRef.current!.focus();   // unsafe: non-null assertion
if (inputRef.current) {
  inputRef.current.focus();  // safe: explicit check
}

// ── FORM 2: Mutable value — like an instance variable ──────
// Pass an actual initial value → TypeScript gives you MutableRefObject<T>
// MutableRefObject<T>.current is: T (never null)
const timerRef    = useRef<number>(0);
const prevUserRef = useRef<User | null>(null); // can be null, but you control it

timerRef.current = setTimeout(cleanup, 5000); // ✅ direct write, no null check

// ── FORM 3: No initial value ───────────────────────────────
// TypeScript gives you MutableRefObject<T | undefined>
const callbackRef = useRef<() => void>();
// current is (() => void) | undefined

// ── KEY INSIGHT ────────────────────────────────────────────
// useRef(null)  → RefObject<T>          — current: T | null (read-only-ish, for DOM)
// useRef(value) → MutableRefObject<T>   — current: T        (writable, for values)
// useRef<T>()   → MutableRefObject<T | undefined>           (writable, starts undef)
```

**Interview Q: Why does `useRef` sometimes return `RefObject` and sometimes `MutableRefObject`?** 🔴
**A:** When you pass `null` as the initial value (`useRef<T>(null)`), TypeScript infers `RefObject<T>` — `current` is `T | null` and readonly (React manages it). When you pass any other value (`useRef(0)`), you get `MutableRefObject<T>` — `current` is writable. React's JSX `ref` prop only accepts `RefObject` (not MutableRefObject with the same element type), so use `null` initial for DOM refs and a real initial value for mutable data refs.

**Aha moment:** The `null` initial value is a signal: "this ref will be managed by React attaching to a DOM element." Any other initial value means "this is my mutable instance variable."

**Common mistake:** `useRef<HTMLInputElement>()` (no initial) for a DOM ref. TypeScript gives you `MutableRefObject<HTMLInputElement | undefined>` which may not be accepted by React's `ref` prop.

**One-liner:** `useRef<T>(null)` for DOM refs (current: T | null). `useRef(value)` for mutable instance vars (current: T). Never confuse the two.

---

### 🔴 11.3 useReducer — Typing State and Actions

**Explanation:** `useReducer` is a natural fit for discriminated union action types. TypeScript narrows the action type in each case of the reducer, giving type-safe access to action payloads.

```typescript
// Define state type
interface State {
  count: number;
  status: "idle" | "loading" | "error";
  errorMessage: string | null;
}

// Define actions as discriminated union
type Action =
  | { type: "INCREMENT" }
  | { type: "DECREMENT" }
  | { type: "RESET" }
  | { type: "SET_LOADING"; isLoading: boolean }
  | { type: "SET_ERROR"; message: string }
  | { type: "SET_COUNT"; count: number };

const initialState: State = { count: 0, status: "idle", errorMessage: null };

// Reducer — TypeScript narrows action type in each case
function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1 };
    case "DECREMENT":
      return { ...state, count: state.count - 1 };
    case "RESET":
      return initialState;
    case "SET_LOADING":
      // action.isLoading is available ✅ (narrowed to SET_LOADING action)
      return { ...state, status: action.isLoading ? "loading" : "idle" };
    case "SET_ERROR":
      return { ...state, status: "error", errorMessage: action.message }; // ✅
    case "SET_COUNT":
      return { ...state, count: action.count }; // ✅
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  // state: State ✅   dispatch: Dispatch<Action> ✅

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>
      {/* Dispatch validates payloads! */}
      <button onClick={() => dispatch({ type: "SET_ERROR" })}>
        {/* ❌ Error: missing required 'message' property! */}
      </button>
    </div>
  );
}
```

**Interview Q: Why is `useReducer` with discriminated union actions considered a best practice in TypeScript?** 🔴
**A:** The discriminated union action type gives you two compile-time guarantees: (1) in the reducer, TypeScript narrows to the specific action type in each `case`, giving typed access to payload properties, and (2) at every `dispatch()` call site, TypeScript validates that the action has the correct payload. Dispatch with a missing payload field = TypeScript error before runtime.

**Aha moment:** Every `dispatch({ type: "..." })` call is validated at compile time. "Forgot to include the required data" is no longer a runtime bug — it's a TypeScript error in your editor.

**Common mistake:** Using `type: string` for all actions instead of a discriminated union. Completely removes type safety — TypeScript can't narrow and can't check payloads.

**One-liner:** `useReducer(reducer, initial)` — type State and Action as discriminated union. TypeScript narrows in each case and validates dispatch payloads.

---

### 🔴 11.4 useContext — Creating and Typing a Context

**Explanation:** The standard pattern for typed contexts: `createContext<Type>(null!)` with a type parameter and non-null assertion. A custom hook wraps `useContext` and throws if used outside the Provider.

```typescript
// 1. Define the context value's type
interface AuthContextValue {
  user: User | null;
  isAuthenticated: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

// 2. Create context with null! (never actually null — Provider always supplies)
const AuthContext = React.createContext<AuthContextValue>(null!);
// null! = non-null assertion on null
// The real value comes from AuthProvider below

// 3. Create the Provider
function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = React.useState<User | null>(null);

  const login = async (email: string, password: string) => {
    const loggedInUser = await authApi.login(email, password);
    setUser(loggedInUser);
  };

  const logout = () => setUser(null);

  const value: AuthContextValue = {
    user,
    isAuthenticated: user !== null,
    login,
    logout,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

// 4. Custom hook with error for usage outside Provider
function useAuth(): AuthContextValue {
  const ctx = React.useContext(AuthContext);
  if (!ctx) {
    throw new Error("useAuth must be used inside <AuthProvider>");
  }
  return ctx; // always AuthContextValue — never null after the check
}

// 5. Usage — fully typed, no null checks needed
function Profile() {
  const { user, logout, isAuthenticated } = useAuth();
  // user: User | null ✅   logout: () => void ✅
  return isAuthenticated ? <div>{user!.name}</div> : <p>Not logged in</p>;
}
```

**Interview Q: Why use `null!` for `createContext` and why wrap in a custom hook?** 🔴
**A:** `createContext<Type>(null!)` provides the type argument while satisfying TypeScript's need for an initial value. The `null!` is a deliberate non-null assertion — we know the context will always be provided by the Provider at runtime. The custom hook (`useAuth`) throws if context is null (used outside Provider), giving a helpful error instead of a cryptic crash. After the null check, callers get a fully typed value with no optional chaining needed.

**Aha moment:** The `null!` pattern is deliberate: we're lying to TypeScript ("this is definitely not null") but backing it up at runtime with a throw in the custom hook.

**Common mistake:** Using `createContext({} as AuthContextValue)` — a Consumer used outside the Provider gets an empty object, causing silent failures instead of a helpful error.

**One-liner:** `createContext<Type>(null!)` + custom hook with null check = fully typed context without null-check boilerplate at every usage.

---

### 🔴 11.5 Custom Hooks — Typing Parameters and Return Values

**Explanation:** Custom hooks are just functions. Annotate parameters, return a typed object or `as const` tuple. For tuple returns, `as const` prevents widening to `(A | B)[]`.

```typescript
// Simple hook — return type inferred (fine for simple cases)
function useToggle(initialState = false) {
  const [value, setValue] = React.useState(initialState);
  const toggle = React.useCallback(() => setValue(v => !v), []);
  return [value, toggle] as const; // as const = tuple, not (boolean | (() => void))[]
}

const [isOpen, toggleOpen] = useToggle(false);
// isOpen: boolean ✅   toggleOpen: () => void ✅

// Complex hook — explicit return type interface
interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => void;
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData]       = React.useState<T | null>(null);
  const [loading, setLoading] = React.useState(true);
  const [error, setError]     = React.useState<string | null>(null);

  const fetchData = React.useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      const res  = await fetch(url);
      const json = await res.json() as T;
      setData(json);
    } catch (e) {
      setError(e instanceof Error ? e.message : "Unknown error");
    } finally {
      setLoading(false);
    }
  }, [url]);

  React.useEffect(() => { fetchData(); }, [fetchData]);

  return { data, loading, error, refetch: fetchData };
}

// Usage — T is inferred from explicit annotation
const { data: users, loading } = useFetch<User[]>("/api/users");
// users: User[] | null ✅   loading: boolean ✅
```

**Interview Q: Why do tuple-returning custom hooks need `as const` and what happens without it?** 🔴
**A:** Without `as const`, `return [value, toggle]` gives the array type `(boolean | (() => void))[]` — a union array where TypeScript doesn't know which position is which. With `as const`, TypeScript infers the exact tuple type `[boolean, () => void]`, so destructuring gives correctly typed individual variables. `as const` preserves positional type information.

**Aha moment:** `as const` on return arrays is one of the most important custom hook patterns. Without it, every destructured value from a hook tuple gets an unhelpful union type.

**Common mistake:** Returning a plain array from a hook instead of `as const` — callers get `(Type1 | Type2)[]` instead of a typed tuple.

**One-liner:** `return [value, handler] as const` = typed tuple. Define return type interface for complex hooks. `as const` is non-negotiable for tuple returns.

---

### 🟡 11.6 useCallback and useMemo — Typing the Callbacks

**Explanation:** `useCallback` and `useMemo` infer their return types from the callback — you rarely need explicit type annotations. The important typing is the callback parameters and return value.

```typescript
// useCallback — TypeScript infers the function type from the callback
const handleSubmit = React.useCallback(
  (formData: FormData) => {      // param needs annotation
    submitForm(formData);
    setSubmitted(true);
  },
  [submitForm]                   // dependency array
);
// handleSubmit: (formData: FormData) => void — inferred ✅

// When you need explicit typing (event handlers)
const handleClick = React.useCallback<React.MouseEventHandler<HTMLButtonElement>>(
  (event) => {
    // event: React.MouseEvent<HTMLButtonElement> — inferred from the type param
    event.preventDefault();
    onClick(event.currentTarget.value);
  },
  [onClick]
);

// useMemo — return type inferred from the callback
const expensiveValue = React.useMemo(
  () => {
    return items.filter(i => i.active).map(i => i.value);
  },
  [items]
);
// expensiveValue: string[] — inferred ✅

// Explicit type param for useMemo (rarely needed)
const sortedItems = React.useMemo<User[]>(
  () => [...users].sort((a, b) => a.name.localeCompare(b.name)),
  [users]
);
```

**Interview Q: Do `useCallback` and `useMemo` need explicit TypeScript annotations?** 🟡
**A:** Rarely. `useCallback` infers the function type from the callback body (including return type). `useMemo` infers the return type from the callback. Explicit type params are occasionally useful when TypeScript can't infer correctly or when you want the type enforced as a contract. Most of the time, correct parameter types on the callback are all you need.

**Aha moment:** TypeScript's inference handles `useCallback` and `useMemo` so well that you rarely need to type them explicitly — just type the callback parameters.

**Common mistake:** Adding unnecessary explicit type params to `useCallback` and `useMemo`. Let TypeScript infer — only annotate when inference fails.

**One-liner:** `useCallback` and `useMemo` infer types from callbacks. Annotate callback params, let TS infer the rest.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 12: Event & DOM Typing

---

### 🔴 12.1 React.ChangeEvent for Input onChange

**Explanation:** React wraps native DOM events in `SyntheticEvent`. For `<input>` onChange, the event type is `React.ChangeEvent<HTMLInputElement>`. The generic parameter specifies which element triggered the event.

```typescript
// Extracted handler — needs explicit type
function handleChange(event: React.ChangeEvent<HTMLInputElement>): void {
  const value = event.target.value;    // string ✅
  const name  = event.target.name;     // string ✅
  setUsername(value);
}

<input onChange={handleChange} />

// Inline — TypeScript INFERS from JSX context (contextual typing!)
<input onChange={(e) => setUsername(e.target.value)} />
// e is automatically React.ChangeEvent<HTMLInputElement> ✅

// Different elements — different generics
<textarea
  onChange={(e: React.ChangeEvent<HTMLTextAreaElement>) => setText(e.target.value)}
/>

<select
  onChange={(e: React.ChangeEvent<HTMLSelectElement>) => setRole(e.target.value)}
/>

// Checkbox — use checked, not value
<input
  type="checkbox"
  onChange={(e: React.ChangeEvent<HTMLInputElement>) => {
    setChecked(e.target.checked); // boolean ✅ (not value!)
  }}
/>

// Generic handler for multiple inputs
function handleFormChange(
  e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
) {
  setForm(prev => ({ ...prev, [e.target.name]: e.target.value }));
}
```

**Interview Q: When do you need to explicitly type React event handlers vs when does TypeScript infer?** 🔴
**A:** For inline handlers in JSX, TypeScript uses contextual typing to automatically infer the event type — no annotation needed. For extracted handler functions defined outside JSX, you must provide the explicit type. The pattern: write handlers inline first (TS infers), then extract and copy the inferred type from your editor's hover tooltip.

**Aha moment:** TypeScript knows `<input onChange={(e) => ...}>` means `e` must be `ChangeEvent<HTMLInputElement>` — from the JSX context. You get full autocomplete on `e.target.value` without writing a single type annotation inline.

**Common mistake:** Using `event: any` for handlers. The correct type is always specific — look it up once per element type and it becomes automatic.

**One-liner:** `React.ChangeEvent<HTMLInputElement>` for extracted handlers. Inline JSX handlers = inferred automatically. Checkbox uses `e.target.checked`.

---

### 🔴 12.2 Mouse, Keyboard, and Focus Events

**Explanation:** React provides typed event objects for every DOM event. The generic parameter specifies the element. Inline JSX handlers get these types automatically via contextual typing.

```typescript
// Mouse events — onClick, onMouseEnter, onMouseLeave, etc.
function handleClick(e: React.MouseEvent<HTMLButtonElement>): void {
  e.preventDefault();
  e.stopPropagation();
  console.log(e.clientX, e.clientY); // mouse coordinates ✅
  const target = e.currentTarget;    // HTMLButtonElement ✅
}

// Keyboard events — onKeyDown, onKeyUp, onKeyPress
function handleKeyDown(e: React.KeyboardEvent<HTMLInputElement>): void {
  if (e.key === "Enter")           submitForm();
  if (e.key === "Escape")          closeModal();
  if (e.ctrlKey && e.key === "s") saveDocument();
}

// Focus events — onFocus, onBlur
function handleFocus(e: React.FocusEvent<HTMLInputElement>): void {
  e.target.select(); // select all text on focus ✅
}

// Form submission
function handleSubmit(e: React.FormEvent<HTMLFormElement>): void {
  e.preventDefault(); // CRITICAL — prevent page reload
  const formData = new FormData(e.currentTarget);
  submitForm(Object.fromEntries(formData));
}

// Quick reference
// React.MouseEvent<T>       — click, mousedown, mouseenter, etc.
// React.KeyboardEvent<T>    — keydown, keyup, keypress
// React.ChangeEvent<T>      — onChange for inputs
// React.FocusEvent<T>       — onFocus, onBlur
// React.FormEvent<T>        — onSubmit
// React.DragEvent<T>        — drag and drop
// React.WheelEvent<T>       — scroll wheel
// React.TouchEvent<T>       — touch events (mobile)
// React.PointerEvent<T>     — pointer events (mouse + touch)
// React.ClipboardEvent<T>   — copy, cut, paste
```

**Interview Q: How do you find the correct TypeScript type for a React event handler you haven't used before?** 🔴
**A:** Three methods: (1) Write the handler inline first — TypeScript contextually infers the type, then hover over the parameter to see it. (2) Follow the naming pattern: `React.EventNameEvent<HTMLElementType>` (e.g., `React.KeyboardEvent<HTMLInputElement>`). (3) Check the JSX type definitions — the element's on* prop types tell you exactly what event type each handler receives.

**Aha moment:** You only need to memorize the naming pattern: `React.MouseEvent<T>`, `React.KeyboardEvent<T>`, etc. Your editor's autocomplete fills in the rest.

**Common mistake:** Using `React.SyntheticEvent` as a catch-all type for everything. It works but gives you a very generic type with few properties — use the specific event type for proper autocomplete.

**One-liner:** Pattern: `React.EventNameEvent<HTMLElementType>`. Inline handlers = auto-inferred. `e.preventDefault()` is always on the event object.

---

### 🔴 12.3 Form onSubmit Typing

**Explanation:** Form submission uses `React.FormEvent<HTMLFormElement>`. The first thing you always do is `e.preventDefault()`. Access form values via controlled state (most common) or through `e.currentTarget`.

```typescript
interface LoginForm {
  email: string;
  password: string;
}

function LoginForm() {
  const [form, setForm] = React.useState<LoginForm>({
    email: "",
    password: "",
  });

  // onSubmit handler — React.FormEvent<HTMLFormElement>
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>): void => {
    e.preventDefault(); // ALWAYS first — prevents browser page reload
    login(form);        // use controlled state values
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = e.target;
    setForm(prev => ({ ...prev, [name]: value }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" value={form.email} onChange={handleChange} />
      <input name="password" type="password" value={form.password} onChange={handleChange} />
      <button type="submit">Login</button>
    </form>
  );
}

// Alternative: uncontrolled form with FormData
function UncontrolledForm() {
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>): void => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget); // e.currentTarget: HTMLFormElement
    const email    = formData.get("email") as string;
    const password = formData.get("password") as string;
    login({ email, password });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" />
      <input name="password" type="password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Interview Q: What is the correct type for a React form's `onSubmit` handler and what is the most important thing to do first?** 🔴
**A:** `React.FormEvent<HTMLFormElement>`. The most important first line is always `e.preventDefault()` — without it, the browser submits the form natively, reloading the page. `e.currentTarget` is typed as `HTMLFormElement`, giving access to `FormData` constructor. For controlled forms, read values from React state instead.

**Aha moment:** The difference between `e.target` and `e.currentTarget`: `target` is the element that triggered the event; `currentTarget` is the element the handler is attached to. For form submit, both are the form element.

**Common mistake:** Forgetting `e.preventDefault()`. Without it, the page reloads and all React state is lost.

**One-liner:** `React.FormEvent<HTMLFormElement>` + `e.preventDefault()` first. Access values via controlled state or `new FormData(e.currentTarget)`.

---

### 🟡 12.4 DOM Queries and Type Assertions

**Explanation:** DOM query methods like `getElementById` return broad types (`HTMLElement | null`) that you need to narrow or assert to the specific type you need.

```typescript
// getElementById returns HTMLElement | null
const el = document.getElementById("search");
// el: HTMLElement | null

el.value;   // ❌ Error: Property 'value' doesn't exist on HTMLElement

// Option 1: type assertion (you're certain what element it is)
const input = document.getElementById("search") as HTMLInputElement;
input.value; // ✅

// Option 2: instanceof check (safer — runtime verification)
const el2 = document.getElementById("search");
if (el2 instanceof HTMLInputElement) {
  el2.value; // ✅ TypeScript knows it's HTMLInputElement here
}

// querySelector — generic, infers from selector when possible
const div    = document.querySelector("div");     // HTMLDivElement | null
const custom = document.querySelector(".my-btn"); // Element | null (no specific inference)
const typed  = document.querySelector<HTMLButtonElement>(".my-btn"); // HTMLButtonElement | null

// Common TypeScript DOM type hierarchy:
// HTMLElement
//   ├── HTMLInputElement    — value, checked, type
//   ├── HTMLButtonElement   — disabled, type
//   ├── HTMLAnchorElement   — href, target
//   ├── HTMLImageElement    — src, alt
//   ├── HTMLFormElement     — elements, submit()
//   ├── HTMLSelectElement   — value, selectedIndex
//   └── HTMLCanvasElement   — getContext()
```

**Interview Q: What is the difference between using `as HTMLInputElement` vs `instanceof HTMLInputElement` for DOM queries?** 🟡
**A:** `as HTMLInputElement` is a compile-time type assertion — TypeScript trusts you, but if the element doesn't exist or is a different type, you get a runtime crash with no warning. `instanceof HTMLInputElement` is a runtime check — TypeScript narrows inside the block AND verifies at runtime. Use `instanceof` when you're not certain; use `as` only when you're confident and the code is obvious.

**Aha moment:** `querySelector<HTMLButtonElement>('.btn')` is the cleanest approach — the generic type parameter tells TypeScript what you expect, and you still need to null-check the result.

**Common mistake:** Forgetting the `| null` return from DOM queries. `getElementById` returns `HTMLElement | null` — access without null check causes TS errors.

**One-liner:** DOM queries return broad types + null. Use `querySelector<T>()` for typed queries, `instanceof` for safe narrowing, `as` for confident assertions.

---

### 🟡 12.5 Event Delegation and Target vs CurrentTarget

**Explanation:** In React events, `e.target` is the element that triggered the event; `e.currentTarget` is the element the handler is attached to. TypeScript types `currentTarget` more specifically — it knows exactly what element has the handler.

```typescript
// currentTarget — element with the handler (known type)
// target        — element that triggered (may be a child — less specific type)

function handleListClick(e: React.MouseEvent<HTMLUListElement>): void {
  e.currentTarget; // HTMLUListElement ✅ — the <ul> with the handler
  e.target;        // EventTarget — could be any child element (less specific)

  // Need to narrow target:
  if (e.target instanceof HTMLLIElement) {
    const id = e.target.dataset.id; // ✅ HTMLLIElement narrowed
    selectItem(id);
  }
}

<ul onClick={handleListClick}>
  <li data-id="1">Item 1</li>
  <li data-id="2">Item 2</li>
</ul>

// currentTarget vs target in practice
<form onSubmit={(e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  // e.currentTarget = the <form> element — always HTMLFormElement ✅
  // e.target = could be the button that triggered submit — less specific
  const data = new FormData(e.currentTarget); // use currentTarget for form
}}>
```

**Interview Q: What is the difference between `e.target` and `e.currentTarget` in React TypeScript?** 🟡
**A:** `e.currentTarget` is typed to the element with the event handler — TypeScript knows its exact type. `e.target` is typed as `EventTarget` (very broad) — it could be any child element that triggered the event. For getting form data, use `e.currentTarget` (typed as `HTMLFormElement`). For event delegation, narrow `e.target` with `instanceof`.

**Aha moment:** `e.currentTarget` is almost always what you want in typed React code — it's the element you attached the handler to, and TypeScript knows its exact type.

**Common mistake:** Using `e.target` when you want `e.currentTarget` — then wondering why TypeScript says the type doesn't have the property you need.

**One-liner:** `e.currentTarget` = handler's element (precise type). `e.target` = triggering element (broad type, must narrow). Use `currentTarget` for form data.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 13: Data Fetching & API Typing

---

### 🔴 13.1 Typing fetch Responses

**Explanation:** `fetch().json()` returns `Promise<any>`. You must add types yourself. The safest approach uses runtime validation (Zod); the common quick approach uses type assertions.

```typescript
// ── Approach 1: Type assertion (trusts the API — common) ──
async function fetchUser(id: number): Promise<User> {
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) {
    throw new Error(`HTTP ${res.status}: ${res.statusText}`);
  }
  return res.json() as Promise<User>; // trust assertion — no runtime check
}

// ── Approach 2: Generic typed fetch utility (better) ───────
async function typedFetch<T>(url: string, options?: RequestInit): Promise<T> {
  const res = await fetch(url, options);
  if (!res.ok) {
    throw new Error(`HTTP ${res.status}: ${res.statusText}`);
  }
  return res.json() as Promise<T>;
}

// Usage — T specified by caller
const user    = await typedFetch<User>("/api/users/1");    // user: User ✅
const users   = await typedFetch<User[]>("/api/users");    // users: User[] ✅
const config  = await typedFetch<AppConfig>("/api/config"); // config: AppConfig ✅

// POST request
async function createUser(data: CreateUserInput): Promise<User> {
  return typedFetch<User>("/api/users", {
    method:  "POST",
    headers: { "Content-Type": "application/json" },
    body:    JSON.stringify(data),
  });
}

// ── IMPORTANT: always check res.ok before .json() ──────────
// Error responses (404, 500) ALSO return JSON (usually an error object)
// Without the check, you'd type-assert an error payload as User!
```

**Interview Q: What is wrong with just doing `res.json() as User` and what is the better approach?** 🔴
**A:** `res.json() as User` is a compile-time-only type assertion — TypeScript trusts you, but if the API returns different data, you get runtime errors. The better approach is runtime validation with a library like Zod: define a schema, validate the response against it, and let the schema infer the TypeScript type. This guarantees the type matches reality, not just your assertion.

**Aha moment:** Type assertions on API responses are a lie. The API could change tomorrow and return `{ error: "not found" }` — your code thinks it's a `User` because of the assertion. Zod fixes this.

**Common mistake:** Not checking `res.ok` before calling `.json()`. A 404 response still has a JSON body — without the check you'll type-assert an error object as `User`.

**One-liner:** Create `typedFetch<T>()` utility. Always check `res.ok`. For real safety, use Zod to validate the shape at runtime.

---

### 🔴 13.2 Async Function Return Types: Promise&lt;T&gt;

**Explanation:** Async functions always return `Promise<T>`. TypeScript infers the Promise wrapping automatically. `Promise.all` returns a typed tuple — TypeScript knows the type at each position.

```typescript
// TypeScript infers Promise<T> from the return statement
async function fetchUser(id: number) {
  const res = await fetch(`/api/users/${id}`);
  return res.json() as User; // return type inferred: Promise<User>
}

// Explicit annotation — good for public API functions
async function fetchUser(id: number): Promise<User> {
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

// Common return type patterns
async function saveData(): Promise<void> {
  await db.save(data); // nothing to return
}

async function findUser(id: number): Promise<User | null> {
  try { return await fetchUser(id); }
  catch { return null; }
}

// Promise.all — TypeScript infers a TUPLE (knows each position's type!)
async function loadDashboard() {
  const [users, products, orders] = await Promise.all([
    fetchUsers(),   // Promise<User[]>
    fetchProducts(), // Promise<Product[]>
    fetchOrders(),   // Promise<Order[]>
  ]);
  // users: User[] ✅  products: Product[] ✅  orders: Order[] ✅
  // TypeScript knows the type at EACH position in the tuple
}

// Result pattern for safe async
type Result<T, E extends Error = Error> =
  | { ok: true; data: T }
  | { ok: false; error: E };

async function safeLoad(id: number): Promise<Result<User>> {
  try {
    return { ok: true, data: await fetchUser(id) };
  } catch (e) {
    return { ok: false, error: e instanceof Error ? e : new Error("Unknown") };
  }
}
```

**Interview Q: What is the return type of an async function in TypeScript?** 🔴
**A:** Async functions always return `Promise<T>`, where T is the type of the returned value. TypeScript infers this automatically. `Promise.all` with typed promises returns a tuple — TypeScript knows the specific type at each array position, not just a general array. For functions returning nothing, the return type is `Promise<void>`.

**Aha moment:** `Promise.all([fetchUsers(), fetchProducts()])` returns `Promise<[User[], Product[]]>` — a tuple where TypeScript knows position 0 is `User[]` and position 1 is `Product[]`. Much better than `Promise<any[]>`.

**Common mistake:** Not annotating async function return types for public APIs. `Promise<User | null>` vs `Promise<User>` is an important distinction for callers.

**One-liner:** Async functions return `Promise<T>` — inferred automatically. `Promise.all` returns a typed tuple. Annotate public async functions explicitly.

---

### 🔴 13.3 Error Handling in Async Code

**Explanation:** Modern TypeScript types `catch (e)` as `unknown` (with `useUnknownInCatchVariables`). Must narrow before using. Creates a pattern for handling errors safely that prevents the "cannot read message of undefined" category of crashes.

```typescript
// Modern TypeScript: catch error is 'unknown' (not 'any')
async function loadUser(id: number) {
  try {
    return await fetchUser(id);
  } catch (error: unknown) {
    error.message; // ❌ Error: 'error' is type 'unknown' — must narrow first

    // Narrow with instanceof:
    if (error instanceof Error) {
      console.error(error.message); // ✅ string
      console.error(error.stack);   // ✅ string | undefined
    } else {
      console.error("Unknown error type:", String(error));
    }
  }
}

// Reusable error message extractor
function getErrorMessage(error: unknown): string {
  if (error instanceof Error) return error.message;
  if (typeof error === "string") return error;
  return `An unknown error occurred: ${JSON.stringify(error)}`;
}

// In React components
async function handleSave() {
  try {
    await saveUser(formData);
    toast.success("Saved!");
  } catch (error) {
    toast.error(getErrorMessage(error)); // type-safe error message
  }
}

// Custom error classes for structured error handling
class ApiError extends Error {
  constructor(
    public readonly statusCode: number,
    public readonly code: string,
    message: string
  ) {
    super(message);
    this.name = "ApiError";
  }
}

function handleApiError(error: unknown) {
  if (error instanceof ApiError) {
    // TypeScript knows: error.statusCode (number), error.code (string)
    if (error.statusCode === 401) redirectToLogin();
    if (error.statusCode === 403) showPermissionError(error.code);
  } else if (error instanceof Error) {
    showGenericError(error.message);
  }
}
```

**Interview Q: How does TypeScript handle the error type in catch blocks and why was it changed?** 🔴
**A:** With `useUnknownInCatchVariables` (enabled by `strict`), catch errors are typed as `unknown` instead of `any`. This forces you to narrow before accessing properties — preventing silent runtime errors from assuming every thrown value is an `Error` object. You can throw strings, numbers, or objects — narrowing with `instanceof Error` handles the common case safely.

**Aha moment:** Switching catch errors from `any` to `unknown` was a major TypeScript safety improvement. It forces you to actually think about what you're catching instead of blindly accessing `.message`.

**Common mistake:** `(error as Error).message` — type assertion bypasses the check. If someone `throw "string error"`, the assertion lies and `.message` is undefined at runtime.

**One-liner:** Catch errors are `unknown` in strict mode. Use `instanceof Error` before accessing properties. Extract a `getErrorMessage(e: unknown)` utility.

---

### 🟡 13.4 Typing React Query (TanStack Query)

**Explanation:** React Query is built with TypeScript first. `useQuery<TData, TError>` takes the success type and error type. TypeScript types `data`, `error`, and all states correctly from these generics.

```typescript
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

// useQuery<TData, TError>
// First generic: success data type
// Second generic: error type (defaults to Error)
function useUser(userId: number) {
  return useQuery<User, Error>({
    queryKey: ["user", userId],
    queryFn: () => typedFetch<User>(`/api/users/${userId}`),
  });
}

// Usage — everything is typed
function UserProfile({ userId }: { userId: number }) {
  const { data, isLoading, isError, error } = useUser(userId);
  // data: User | undefined ✅
  // error: Error | null ✅
  // isLoading: boolean ✅

  if (isLoading) return <Spinner />;
  if (isError)   return <p>Error: {error.message}</p>; // error: Error ✅
  if (!data)     return null;

  return <div>{data.name}</div>; // data: User ✅ (narrowed after checks)
}

// useMutation<TData, TError, TVariables, TContext>
function useUpdateUser() {
  const queryClient = useQueryClient();

  return useMutation<User, Error, Partial<User> & { id: number }>({
    mutationFn: (updates) => typedFetch<User>(`/api/users/${updates.id}`, {
      method: "PATCH",
      body: JSON.stringify(updates),
    }),
    onSuccess: (updatedUser) => {   // updatedUser: User ✅
      queryClient.setQueryData(["user", updatedUser.id], updatedUser);
      toast.success("Updated!");
    },
    onError: (error) => {            // error: Error ✅
      toast.error(error.message);
    },
  });
}

const mutation = useUpdateUser();
mutation.mutate({ id: 1, name: "Bob" }); // TypeScript validates the variables ✅
```

**Interview Q: How do you type React Query hooks and what are the key type parameters?** 🟡
**A:** `useQuery<TData, TError>` — first generic is the success data type, second is error type (defaults to `Error`). `useMutation<TData, TError, TVariables>` adds a third generic for mutation input. TypeScript then types `data`, `error`, mutation `variables`, and all callbacks. The `queryFn`'s return type should match `TData` — TypeScript verifies this.

**Aha moment:** React Query infers types from `queryFn`'s return type, so you often don't even need to specify `TData` explicitly — if `queryFn` returns `Promise<User>`, TypeScript knows `data` is `User | undefined`.

**Common mistake:** Not handling the `data: TData | undefined` case. `data` is undefined while loading — always check before use or type-assert after loading check.

**One-liner:** `useQuery<User, Error>({ queryFn })` — first = data type, second = error type. `data` is `T | undefined` until loaded.

---

### 🟡 13.5 Zod for Runtime Validation + Type Inference

**Explanation:** Zod validates data shapes at runtime AND infers TypeScript types from schemas. This solves the fundamental problem of type assertions: the schema is the single source of truth for both validation and types.

```typescript
import { z } from "zod";

// Define schema — this is the single source of truth
const UserSchema = z.object({
  id:        z.number(),
  name:      z.string().min(1),
  email:     z.string().email(),
  role:      z.enum(["user", "admin"]),
  createdAt: z.string().datetime(),
  age:       z.number().min(0).optional(),
});

// Infer TypeScript type FROM the schema — no manual type definition!
type User = z.infer<typeof UserSchema>;
// type User = {
//   id: number;
//   name: string;
//   email: string;
//   role: "user" | "admin";
//   createdAt: string;
//   age?: number | undefined;
// }

// Use in API calls — validates AND types in one step
async function fetchUser(id: number): Promise<User> {
  const res  = await fetch(`/api/users/${id}`);
  const data = await res.json();

  // parse throws ZodError if data doesn't match schema
  // returns correctly typed User if it does
  return UserSchema.parse(data);
}

// Safe parse — doesn't throw, returns result object
const result = UserSchema.safeParse(unknownData);
if (result.success) {
  result.data.name; // User ✅ — TypeScript narrows based on .success flag
} else {
  result.error.issues.forEach(issue => console.error(issue.message));
}

// Form validation with Zod
const LoginSchema = z.object({
  email:    z.string().email("Invalid email"),
  password: z.string().min(8, "Password must be 8+ characters"),
});

type LoginData = z.infer<typeof LoginSchema>;

function LoginForm() {
  const [errors, setErrors] = React.useState<Partial<LoginData>>({});
  // ...
}
```

**Interview Q: Why use Zod instead of TypeScript type assertions for API responses?** 🟡
**A:** TypeScript type assertions (`as User`) are compile-time only — if the API changes its shape, TypeScript thinks it's still `User` and you get silent runtime errors. Zod validates at runtime AND derives TypeScript types from the schema. The schema is the single source of truth: change the schema, both validation and TypeScript types update automatically. Especially valuable for external APIs you don't control.

**Aha moment:** `type User = z.infer<typeof UserSchema>` is one of the most powerful TypeScript patterns. Your runtime validation and your TypeScript types are always in sync — one source of truth.

**Common mistake:** Defining a TypeScript `type User = { ... }` AND a separate Zod schema for the same shape. Use `z.infer` to derive the type from the schema — don't maintain both.

**One-liner:** Zod validates at runtime AND infers TS types. `z.infer<typeof Schema>` = type derived from schema. Single source of truth.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 14: Mapped & Template Types

---

### 🟡 14.1 Mapped Types: { [K in keyof T]: ... }

**Explanation:** Mapped types iterate over the keys of an existing type and transform each property. This is how `Partial`, `Required`, `Readonly`, and `Record` are all implemented internally.

```typescript
// Basic mapped type — iterate over T's keys and transform
type Stringify<T> = {
  [K in keyof T]: string; // every property becomes string
};

interface User { id: number; name: string; active: boolean }
type StringifiedUser = Stringify<User>;
// { id: string; name: string; active: string }

// Preserving the type: T[K] gives the original type at key K
type Identity<T> = {
  [K in keyof T]: T[K]; // identity — same as T
};

// ── How utility types are implemented ─────────────────────
// Partial — add ? modifier
type MyPartial<T>  = { [K in keyof T]?: T[K] };

// Required — remove ? modifier (the - removes it)
type MyRequired<T> = { [K in keyof T]-?: T[K] };

// Readonly — add readonly modifier
type MyReadonly<T> = { readonly [K in keyof T]: T[K] };

// Mutable — remove readonly modifier
type Mutable<T>    = { -readonly [K in keyof T]: T[K] };

// ── Practical custom mapped type ──────────────────────────
// Make all properties nullable
type Nullable<T> = { [K in keyof T]: T[K] | null };

type NullableUser = Nullable<User>;
// { id: number | null; name: string | null; active: boolean | null }

// Validation error map — same keys as the form, all error strings
type FormErrors<T> = { [K in keyof T]?: string };

type UserFormErrors = FormErrors<User>;
// { id?: string; name?: string; active?: string }
// Perfect for form validation error objects!
```

**Interview Q: What are mapped types and how are they used to implement `Partial<T>`?** 🟡
**A:** Mapped types use `[K in keyof T]` to iterate over a type's keys and create a new type with transformed properties. `Partial<T>` is `{ [K in keyof T]?: T[K] }` — the `?` adds optionality to each key, `T[K]` preserves the original value type. `Required<T>` uses `-?` to remove optionality. Understanding mapped types means you can build any of the utility types from scratch.

**Aha moment:** Every utility type in TypeScript (`Partial`, `Required`, `Readonly`, `Record`, `Pick`, `Omit`) is just a mapped type with a different transformation. They're not magic — they're patterns you can write yourself.

**Common mistake:** Forgetting `T[K]` and writing just `string` — this changes all property types to string instead of preserving them.

**One-liner:** `{ [K in keyof T]: transform }` = iterate type keys and transform. `T[K]` = preserve original type. This IS `Partial`, `Readonly`, etc.

---

### 🟡 14.2 keyof and typeof Operators

**Explanation:** `keyof T` produces a union of all property names of a type. `typeof value` (in a type context) gets the TypeScript type of a value. Combined as `keyof typeof obj`, they're fundamental to typed config objects.

```typescript
interface User { id: number; name: string; email: string }

// keyof — get all keys as a union type
type UserKey = keyof User; // "id" | "name" | "email"

// Type-safe property access with keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]; // return type is exactly T[K] — the type at that key
}
const user = { id: 1, name: "Alice", email: "a@test.com" };
const name  = getProperty(user, "name");  // name: string ✅
const id    = getProperty(user, "id");    // id: number ✅
getProperty(user, "phone");               // ❌ "phone" not a key of user

// typeof in TYPE position — get the type of a VALUE
const config = { host: "localhost", port: 8080 };
type Config  = typeof config; // { host: string; port: number }

// keyof + typeof — get union of keys from a RUNTIME object
const ROUTES = { home: "/", about: "/about", contact: "/contact" } as const;
type RouteName = keyof typeof ROUTES; // "home" | "about" | "contact"
type RoutePath = typeof ROUTES[RouteName]; // "/" | "/about" | "/contact"

// React: typed dispatch
type Action = { type: "INCREMENT" } | { type: "DECREMENT" } | { type: "RESET" };
type ActionType = Action["type"]; // "INCREMENT" | "DECREMENT" | "RESET"
// Equivalent to: keyof Pick<Action, "type"> — but the indexed access is simpler
```

**Interview Q: What is the difference between `keyof T` and `typeof value` in TypeScript?** 🟡
**A:** `keyof T` works on types and produces a union of property key types. `typeof` works on values (in a type context) and produces the TypeScript type of that value. They're combined with `keyof typeof obj` — first get the type of the object, then get its keys. This is the key pattern for extracting typed unions from runtime constant objects.

**Aha moment:** `keyof typeof ROUTES` is the idiom for "give me the union of keys of this constant object" — used everywhere for route names, status maps, icon maps, and more.

**Common mistake:** Confusing runtime `typeof x === "string"` (JavaScript) with type-level `typeof x` (TypeScript type system). Same keyword, completely different contexts.

**One-liner:** `keyof T` = union of type's keys. `typeof value` = type of a value. `keyof typeof obj` = union of object's keys (the key idiom).

---

### 🟡 14.3 Template Literal Types

**Explanation:** Template literal types use backtick syntax to create string types with patterns — like JavaScript template literals but at the type level. Combined with union types, they generate entire families of related string types.

```typescript
// Basic template literal type
type Greeting = `Hello, ${string}`; // any string starting with "Hello, "
const g1: Greeting = "Hello, Alice"; // ✅
const g2: Greeting = "Hi, Alice";    // ❌ must start with "Hello, "

// Combining with unions — generates cartesian product of strings
type Side      = "top" | "right" | "bottom" | "left";
type Padding   = `padding-${Side}`; // "padding-top" | "padding-right" | ...
type Margin    = `margin-${Side}`;  // "margin-top" | "margin-right" | ...

// React: typed event handler prop names
type EventName = "click" | "focus" | "blur" | "change";
type HandlerName = `on${Capitalize<EventName>}`;
// = "onClick" | "onFocus" | "onBlur" | "onChange"

// CSS utility class generation (Tailwind-like)
type Breakpoint = "sm" | "md" | "lg" | "xl";
type SpacingSize = 0 | 1 | 2 | 4 | 8 | 16;
type MarginClass = `m-${SpacingSize}`;         // "m-0" | "m-1" | "m-2" | ...
type BreakpointMargin = `${Breakpoint}:m-${SpacingSize}`; // "sm:m-0" | "md:m-4" | ...

// Typed API paths
type ApiVersion = "v1" | "v2";
type Resource = "users" | "products" | "orders";
type ApiEndpoint = `/api/${ApiVersion}/${Resource}`;
// "/api/v1/users" | "/api/v1/products" | "/api/v2/users" | ...

// String manipulation helper types
type UpperGreet = Uppercase<Greeting>;   // `HELLO, ${string}`
type LowerRoute = Lowercase<"/API/USERS">; // "/api/users"
```

**Interview Q: What are template literal types and what can you build with them?** 🟡
**A:** Template literal types use backtick syntax to create string type patterns. When combined with union types, they generate the cartesian product — all possible combinations. Practical uses: typed event handler names (`onClick`, `onFocus`), CSS class utilities, API endpoint paths, and i18n translation keys. TypeScript also provides `Uppercase`, `Lowercase`, `Capitalize`, `Uncapitalize` for string manipulation.

**Aha moment:** Template literal types can auto-generate large sets of related string types from a few base unions. Instead of manually listing 20 event handler names, you derive them from `"click" | "focus" | ...` + a template.

**Common mistake:** Using template literal types where a validation library would be more appropriate. They only validate at compile time — runtime strings still need runtime checking.

**One-liner:** Template literal types `` `${Union}suffix` `` = generate typed string patterns. Combine with unions for cartesian products of string types.

---

### 🟡 14.4 Key Remapping with as in Mapped Types

**Explanation:** The `as` clause in mapped types lets you transform the output key names — rename, filter out keys, or create new key names from existing ones.

```typescript
// Basic key remapping — rename keys
type RenameId<T> = {
  [K in keyof T as K extends "id" ? "identifier" : K]: T[K];
};

interface User { id: number; name: string }
type RenamedUser = RenameId<User>;
// { identifier: number; name: string }

// Filter out keys — returning never in as removes the key
type NonNullableProps<T> = {
  [K in keyof T as T[K] extends null | undefined ? never : K]: T[K];
};

interface MaybeUser { id: number; name: string; bio: string | null; avatar?: string }
type RequiredUser = NonNullableProps<MaybeUser>;
// { id: number; name: string } — bio and avatar filtered out!

// Generate event handler names from property names
type EventHandlers<T> = {
  [K in keyof T as `on${Capitalize<string & K>}Change`]: (value: T[K]) => void;
};

interface FormFields { name: string; email: string; age: number }
type FormHandlers = EventHandlers<FormFields>;
// { onNameChange: (v: string) => void; onEmailChange: (v: string) => void; ... }

// Practical: extract only string-valued keys
type StringKeys<T> = {
  [K in keyof T as T[K] extends string ? K : never]: T[K];
};

interface Mixed { id: number; name: string; active: boolean; email: string }
type StringOnly = StringKeys<Mixed>;
// { name: string; email: string }
```

**Interview Q: What is the `as` clause in mapped types used for?** 🟡
**A:** The `as` clause renames or filters keys during the mapping. If the `as` expression evaluates to `never`, the key is removed from the output type. Common uses: filtering keys by their value type (remove nullable properties), transforming key names (add prefixes/suffixes), and generating event handler names from form field names.

**Aha moment:** `as never` in a mapped type removes the key — this is how you build filter utilities that selectively include/exclude properties based on their types.

**Common mistake:** Forgetting `string &` when using Capitalize with a keyof type. `keyof T` can be `string | number | symbol`, and `Capitalize` only works on `string`.

**One-liner:** `[K in keyof T as newKey]` = remap keys. `as never` removes a key. Combined with template literals = generate typed method names.

---

### 🟢 14.5 Indexed Access Types: T[K]

**Explanation:** `T[K]` accesses the type of property K within type T. Called indexed access types — the type-level equivalent of property access.

```typescript
interface User {
  id: number;
  name: string;
  address: {
    city: string;
    zip: string;
  };
  scores: number[];
}

// Access specific property types
type UserId      = User["id"];           // number
type UserName    = User["name"];         // string
type UserAddress = User["address"];      // { city: string; zip: string }
type UserCity    = User["address"]["city"]; // string (nested access)
type UserScore   = User["scores"][number];  // number (element type of array)

// Useful with union keys
type NameOrId = User["name" | "id"]; // string | number

// Array element type
type ArrayElement<T extends any[]> = T[number]; // number = any array index
type Scores = ArrayElement<number[]>; // number

// Most practical use: pull out a nested type without defining it separately
interface ApiResponse {
  data: {
    users: { id: number; name: string }[];
    total: number;
  };
  meta: { page: number; pageSize: number };
}

// No need to define these separately:
type UserItem = ApiResponse["data"]["users"][number]; // { id: number; name: string }
type PageMeta = ApiResponse["meta"];                  // { page: number; pageSize: number }
```

**Interview Q: What are indexed access types and why are they useful?** 🟢
**A:** `T[K]` extracts the type of a specific property from another type — the type-level equivalent of `obj.property`. They're useful for: extracting nested types without defining them separately, getting array element types (`T[number]`), and accessing union-keyed property types (`T["a" | "b"]`). They prevent duplication by using existing types as sources of truth.

**Aha moment:** `ApiResponse["data"]["users"][number]` gets the element type of a nested array — without creating any separate type aliases. The existing type IS the source of truth.

**Common mistake:** Using `T[K]` outside a generic context. `T["prop"]` where T is a concrete type like `string` gives an error — it works for object types only.

**One-liner:** `T["key"]` = type of that property. `T["a" | "b"]` = union of both property types. `T[number]` = array element type.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Quick Revision — Part 2 One-Liners

```
CATEGORY 8: CLASSES
  Declare class properties with types. Initialize in constructor or =. Use ! for async init
  public = anywhere. private = class only. protected = class + subclasses. All compile-time
  constructor(private name: string) = declare + assign in one shot. Eliminates boilerplate
  implements = compile-time contract check. No code generated, no inheritance
  Abstract class = base with shared concrete methods + required abstract methods
  readonly = per-instance immutable. static = class-level shared. Combine: static readonly
  Structural typing: compatible shape = compatible type. implements not required for compatibility

CATEGORY 9: UTILITY TYPES
  Partial<T> = all optional. Use for updates, form state, config-with-defaults
  Required<T> = all mandatory (removes ?). Use after applying defaults
  Readonly<T> = compile-time immutable (shallow). Not Object.freeze
  Pick<T, K> = keep listed keys. Omit<T, K> = remove listed keys. List fewer = use that
  Record<K, V> = typed dictionary. With literal union K = all keys required
  Exclude<T, U> removes matching. Extract<T, U> keeps matching. Exclude = filter out
  NonNullable<T> = remove null/undefined. Use as return type in generics
  ReturnType<F> = extract return type. Parameters<F> = param tuple. Use for wrappers
  Awaited<Promise<T>> = T. Combine with ReturnType for async function types

CATEGORY 10: TYPING REACT COMPONENTS
  interface ButtonProps + plain function > React.FC in modern React 18+
  ReactNode for children (accepts everything). JSX.Element too restrictive
  Required props: TypeScript errors at call site if missing. Optional with ?: provide defaults
  Generic components function List<T> — TypeScript infers T from items. In .tsx: <T,>
  extends ComponentProps<'div'> + {...rest} = all native HTML attrs pass through
  forwardRef<HTMLInputElement, Props> — ref element type first, props second
  React.memo infers props from component. React.lazy = lazy(() => import('./Comp'))

CATEGORY 11: TYPING HOOKS
  useState<User | null>(null) — always annotate when starting null or empty array
  useRef(null) = DOM ref (RefObject). useRef(value) = mutable value (MutableRefObject)
  useReducer: type State + discriminated union Action. TypeScript narrows per case
  createContext<Type>(null!) + custom hook with null check = typed context pattern
  Custom hook tuples: return [value, fn] as const = typed tuple not union array
  useCallback and useMemo: annotate callback params, let TS infer the rest

CATEGORY 12: EVENT & DOM TYPING
  React.ChangeEvent<HTMLInputElement> for extracted onChange handlers. Inline = inferred
  React.MouseEvent<T>, KeyboardEvent<T>, FocusEvent<T>, FormEvent<T>
  e.preventDefault() always first in form onSubmit handlers
  currentTarget = handler's element (precise type). target = triggering element (broad)
  DOM queries return null. querySelector<T>('.class') for typed results

CATEGORY 13: DATA FETCHING & API TYPING
  typedFetch<T>() utility wrapper. Always check res.ok before .json()
  Async functions return Promise<T> — inferred. Promise.all returns typed tuple
  Catch errors are unknown in strict mode. instanceof Error before accessing .message
  getErrorMessage(error: unknown): string — extract error utility function
  useQuery<User, Error> — first = data type, second = error type
  Zod: z.infer<typeof Schema> = type from schema. Single source of truth

CATEGORY 14: MAPPED & TEMPLATE TYPES
  [K in keyof T]: T[K] = iterate type keys. T[K] preserves original type
  MyPartial = [K in keyof T]?. MyRequired = [K in keyof T]-?. MyReadonly = readonly [K in keyof T]
  keyof T = union of keys. typeof value = type of value. keyof typeof obj = keys of object
  Template literals: `${Union}suffix` generates typed string pattern families
  [K in keyof T as newKey]: remap keys. as never = remove key from output
  T["key"] = type of that property. T[number] = array element type
```

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

*Continue to **Part 3** → Categories 15–20 (Modules, Narrowing, tsconfig, State Management, Testing, Build Tools)*