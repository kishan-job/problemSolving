# JavaScript Equality (`==` vs `===`) Cheat Sheet

## Strict Equality (`===`)

Checks:

1. Type
2. Value

No type conversion happens.

```javascript
5 === 5
// true

"5" === 5
// false

null === undefined
// false
```

### Mental Model

```text
Same Type?
    ↓
Compare Values
```

---

# Loose Equality (`==`)

If types are different, JavaScript applies equality rules (often type conversion) before comparing.

```javascript
"5" == 5
// true
```

Internally:

```javascript
Number("5") == 5

5 == 5
```

Result:

```javascript
true
```

---

## Examples

### String and Number

```javascript
"5" == 5
// true
```

---

### Boolean and Number

```javascript
false == 0
// true

true == 1
// true
```

Because:

```javascript
Number(false) // 0
Number(true)  // 1
```

---

# Special Rule: `null` and `undefined`

JavaScript treats them as equal to each other.

```javascript
null == undefined
// true
```

But:

```javascript
null === undefined
// false
```

because their types are different.

---

# Important Gotcha

Many developers think:

```javascript
Boolean(null) // false
Boolean(undefined) // false
```

Therefore:

```javascript
null == false
undefined == false
```

should be true.

❌ Wrong!

JavaScript does NOT compare by converting everything to boolean.

---

## Actual Results

```javascript
null == undefined
// true

null == false
// false

undefined == false
// false

null == 0
// false

undefined == 0
// false

null == ""
// false

undefined == ""
// false
```

---

# Easy Memory Trick

```text
null only equals undefined

undefined only equals null
```

```javascript
null == undefined
// true
```

Everything else:

```javascript
null == false
// false

undefined == false
// false

null == 0
// false

undefined == 0
// false
```

---

# Common Interview Questions

```javascript
console.log(false == 0);
// true

console.log(true == 1);
// true

console.log("5" == 5);
// true

console.log("5" === 5);
// false

console.log(null == undefined);
// true

console.log(null === undefined);
// false

console.log(null == 0);
// false

console.log(undefined == 0);
// false
```

---

# Final Mental Model

```text
===

Check Type
    ↓
Check Value

No Conversion


==

Different Types?
    ↓
Apply JavaScript Equality Rules
    ↓
(May Convert Types)
    ↓
Compare Values


Special Case:

null == undefined
// true
```

## Golden Rule

Prefer:

```javascript
===
```

because it is predictable and avoids JavaScript's confusing coercion rules.
