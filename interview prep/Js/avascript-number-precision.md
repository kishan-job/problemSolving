# JavaScript Number Precision, Exponent Bits, MAX_SAFE_INTEGER, and BigInt

## JavaScript Number Structure

JavaScript stores all numbers using the IEEE-754 double-precision floating-point format.

```text
64 bits total

┌──────┬──────────┬────────────────────────────────────┐
│ Sign │ Exponent │ Fraction (Mantissa)               │
│ 1 bit│ 11 bits  │ 52 bits                           │
└──────┴──────────┴────────────────────────────────────┘
```

Because IEEE-754 assumes a hidden leading `1`, JavaScript effectively gets:

```text
52 stored bits + 1 hidden bit
=
53 bits of precision
```

---

## What Each Part Does

### 1. Sign Bit (1 Bit)

Determines whether the number is positive or negative.

```text
0 = Positive
1 = Negative
```

Example:

```javascript
100
-100
```

The sign bit distinguishes between them.

---

### 2. Exponent Bits (11 Bits)

The exponent stores the scale (magnitude) of the number.

Think of scientific notation:

```text
1.23 × 10^5
```

Here:

```text
1.23 = Significant digits
5    = Exponent
```

The exponent answers:

> "Where should the decimal point go?"

Example:

```text
1.2345 × 10^3
=
1234.5
```

```text
1.2345 × 10^-3
=
0.0012345
```

The exponent allows JavaScript to represent:

```javascript
1e20
1e100
1e-20
1e-100
```

without storing all the zeros.

---

### 3. Fraction/Mantissa (52 Bits)

Stores the significant digits of the number.

This is the part responsible for accuracy.

Because of the hidden leading bit:

```text
52 bits stored
+
1 implicit bit
=
53 bits precision
```

---

## What Happened to the Remaining Bits?

Many developers think:

```text
64 bits total
53 bits for integers
11 bits for decimal digits
```

❌ Incorrect

Actual layout:

```text
64 bits total

1 bit   -> Sign
11 bits -> Exponent
52 bits -> Fraction
```

The exponent bits are NOT used to store:

```text
Digits after the decimal point
Extra digits beyond 16 digits
```

Instead, they are used only for:

```text
Range (how large or small a number can be)
```

---

## Exponent Gives Range, Not Precision

Exponent bits allow JavaScript to represent:

```javascript
1e308
0.000000000000000000000001
```

But they do NOT increase accuracy.

Think of it like:

```text
Exponent  -> How BIG or SMALL a number can be.
Precision -> How ACCURATELY digits are stored.
```

---

## What Is Precision?

Precision means:

> How accurately a number can be represented without losing information.

Simple definition:

```text
Precision = Accuracy of stored digits.
```

When precision is lost:

```javascript
console.log(9007199254740992 === 9007199254740993);
```

Output:

```javascript
true
```

JavaScript can no longer distinguish between the two values.

---

## Why Does JavaScript Lose Precision?

JavaScript only has:

```text
53 bits of precision
```

for storing significant digits.

This results in a maximum safe integer:

```javascript
Number.MAX_SAFE_INTEGER
```

Value:

```javascript
9007199254740991
```

Which equals:

```text
2^53 - 1
```

---

## Why Is 9007199254740991 Special?

```javascript
Number.MAX_SAFE_INTEGER
// 9007199254740991
```

This means:

> Every integer up to this value can be represented exactly.

Examples:

```javascript
9007199254740989 ✅
9007199254740990 ✅
9007199254740991 ✅
```

Beyond this:

```javascript
9007199254740992
9007199254740993
```

precision problems begin.

Example:

```javascript
console.log(9007199254740992 === 9007199254740993);
```

Output:

```javascript
true
```

---

## Important Doubt

### If

```text
9007199254740980.84747744
```

is smaller than

```text
9007199254740991
```

why isn't it stored exactly?

Because:

```text
MAX_SAFE_INTEGER applies ONLY to integers.
```

---

### What MAX_SAFE_INTEGER Guarantees

```javascript
Number.MAX_SAFE_INTEGER
// 9007199254740991
```

Means:

✅ Every integer up to this value is exact.

It does NOT mean:

✅ Every smaller number is exact.

---

## Integer Example

```javascript
9007199254740991
```

This is an integer.

All available precision can be used to store the integer value.

Result:

```text
Exact ✅
```

---

## Decimal Example

Consider:

```text
9007199254740980.84747744
```

JavaScript must store:

```text
9007199254740980
```

and

```text
0.84747744
```

using the same precision budget.

The number contains roughly:

```text
9007199254740980 -> 16 digits
84747744         -> 8 digits

Total ≈ 24 significant digits
```

JavaScript can accurately remember only about:

```text
15–16 significant decimal digits
```

Therefore:

```text
Precision is lost.
```

---

## Think of Precision as a Fixed Budget

