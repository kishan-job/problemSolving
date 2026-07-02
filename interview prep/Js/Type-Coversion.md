# JavaScript Type Conversion (Cheat Sheet)

## Golden Rule

JavaScript does **not** convert everything to a number.

It converts values to the **type required by the current operation**.

```javascript
Number()   // when a number is needed
String()   // when a string is needed
Boolean()  // when a boolean is needed
```

---

# 1. Number Conversion

When JavaScript needs a number:

```javascript
Number(value)
```

### String → Number

```javascript
Number("123")     // 123
Number("10.5")    // 10.5
Number("abc")     // NaN
Number("")        // 0
```

### Boolean → Number

```javascript
Number(true)      // 1
Number(false)     // 0
```

### Null → Number

```javascript
Number(null)      // 0
```

### Undefined → Number

```javascript
Number(undefined) // NaN
```

### Object → Number

JavaScript tries:

```text
valueOf()
↓
toString()
↓
Number(result)
```

Example:

```javascript
Number([123])

// internally

[123].valueOf()   // [123]
[123].toString()  // "123"
Number("123")     // 123
```

---

# 2. String Conversion

When JavaScript needs a string:

```javascript
String(value)
```

### Number → String

```javascript
String(123) // "123"
```

### Boolean → String

```javascript
String(true) // "true"
```

### Null → String

```javascript
String(null) // "null"
```

### Undefined → String

```javascript
String(undefined) // "undefined"
```

### Object → String

Usually uses:

```javascript
toString()
```

Example:

```javascript
String([1, 2, 3])

// internally

[1, 2, 3].toString()

// result

"1,2,3"
```

---

# 3. Boolean Conversion

When JavaScript needs true/false:

```javascript
Boolean(value)
```

## Falsy Values

Only these become false:

```javascript
false
0
-0
""
null
undefined
NaN
```

Examples:

```javascript
Boolean(false)     // false
Boolean(0)         // false
Boolean("")        // false
Boolean(null)      // false
Boolean(undefined) // false
Boolean(NaN)       // false
```

## Truthy Values

Everything else is true:

```javascript
Boolean("kishan") // true
Boolean([])       // true
Boolean({})       // true
Boolean(1)        // true
```

---

# 4. Object → Primitive Conversion

When JavaScript needs to convert an object into a primitive:

```javascript
object → primitive
```

It generally tries:

```text
1. valueOf()
2. toString()
```

Example:

```javascript
const obj = {
  valueOf() {
    return 100;
  }
};

Number(obj); // 100
```

---

# Common Examples

## isNaN()

```javascript
isNaN("123")
```

Internally:

```javascript
Number("123")
```

Result:

```javascript
false
```

---

```javascript
isNaN("abc")
```

Internally:

```javascript
Number("abc")
```

Result:

```javascript
true
```

---

## String Concatenation

```javascript
"Hello " + 123
```

Internally:

```javascript
"Hello " + String(123)
```

Result:

```javascript
"Hello 123"
```

---

## Boolean Context

```javascript
if ("kishan") {
  console.log("true");
}
```

Internally:

```javascript
Boolean("kishan")
```

Result:

```javascript
true
```

---

# Type Checking

## typeof

```javascript
typeof "abc"      // "string"
typeof 123        // "number"
typeof true       // "boolean"
typeof undefined  // "undefined"
typeof {}         // "object"
typeof []         // "object"
typeof null       // "object" (historical bug)
```

---

## Accurate Type Detection

```javascript
Object.prototype.toString.call(value)
```

Examples:

```javascript
Object.prototype.toString.call({})
// [object Object]

Object.prototype.toString.call([])
// [object Array]

Object.prototype.toString.call(new Date())
// [object Date]

Object.prototype.toString.call(/abc/)
// [object RegExp]

Object.prototype.toString.call(null)
// [object Null]

Object.prototype.toString.call(undefined)
// [object Undefined]
```

---

# Remember

✅ JavaScript converts values to the type required by the operation.

❌ JavaScript does NOT always convert everything to a number.

Mental Model:

```text
Need Number?  → Number()
Need String?  → String()
Need Boolean? → Boolean()

Object?
↓
valueOf()
↓
toString()
↓
Convert to required type
```
