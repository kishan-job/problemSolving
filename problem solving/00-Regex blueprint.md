# The Complete Regex Blueprint
## Everything You Need in One Place

---

# How to Prepare — Step by Step Guide

```
This is not a reading assignment. It is a daily reference map.

BEFORE DAY 1 (today):
  → Read Section A: What is Regex (understand the concept)
  → Read Section B: The 5 Building Blocks (learn the alphabet)
  → Read How to Think About Regex (understand the mental model)
  → Do NOT try to memorize everything. Just get familiar.

DAY 1:
  → Read Level 1, Category 1: Literal Characters
  → Write 3 patterns of your own
  → Test them on: https://regex101.com

DAY 2 onwards:
  → One category per day
  → Read → Write → Test → Done

DAILY ROUTINE (15 minutes max):
  STEP 1 → Read today's category
  STEP 2 → Understand the pattern and what it matches
  STEP 3 → Write 3 of your own patterns from scratch
  STEP 4 → Test on regex101.com
  STEP 5 → Close the map. Done for today.

THE LEVELS:
  Level 1 → Foundation   → learn first → used in 80% of real code
  Level 2 → Intermediate → learn second → used in 15% of real code
  Level 3 → Advanced     → learn third → used in 4% of real code
  Level 4 → Specialist   → learn as needed → remaining 1%

TOOL TO USE ALWAYS:
  https://regex101.com
  → Paste your pattern on top
  → Paste your test string below
  → It highlights matches in real time
  → It explains what each part of your pattern does
```

---

# Table of Contents

- [Section A: What is Regex?](#section-a-what-is-regex)
- [Section B: The 5 Building Blocks](#section-b-the-5-building-blocks)
-  [How to Think About Regex](#how-to-think-about-regex)
- [Level 1 — Foundation (80% of real problems)](#level-1--foundation-80-of-real-problems)
  - [Category 1: Literal Characters](#category-1-literal-characters)
  - [Category 2: Anchors](#category-2-anchors)
  - [Category 3: Character Classes](#category-3-character-classes)
  - [Category 4: Shorthand Classes](#category-4-shorthand-classes)
  - [Category 5: Quantifiers](#category-5-quantifiers)
  - [Category 6: Flags g and i](#category-6-flags-g-and-i)
  - [Level 1 Pattern Picker](#level-1-pattern-picker)
- [Level 2 — Intermediate (15% more)](#level-2--intermediate-15-more)
  - [Category 7: Groups and Capturing](#category-7-groups-and-capturing)
  - [Category 8: Alternation](#category-8-alternation)
  - [Category 9: Non-Capturing Groups](#category-9-non-capturing-groups)
  - [Category 10: Greedy vs Lazy](#category-10-greedy-vs-lazy)
  - [Category 11: Flags m and s](#category-11-flags-m-and-s)
  - [Category 12: Backreferences](#category-12-backreferences)
  - [Level 2 Pattern Picker](#level-2-pattern-picker)
- [Level 3 — Advanced (4% more)](#level-3--advanced-4-more)
  - [Category 13: Lookahead](#category-13-lookahead)
  - [Category 14: Lookbehind](#category-14-lookbehind)
  - [Category 15: Named Capture Groups](#category-15-named-capture-groups)
  - [Level 3 Pattern Picker](#level-3-pattern-picker)
- [Level 4 — Specialist (Remaining 1%)](#level-4--specialist-remaining-1)
- [Section C: Regex in JavaScript](#section-c-regex-in-javascript)
- [Section D: Real World Patterns](#section-d-real-world-patterns)
- [Section E: Master Pattern Picker](#section-e-master-pattern-picker)
- [Section F: Common Mistakes](#section-f-common-mistakes)
- [Section G: 28-Day Practice Roadmap](#section-g-28-day-practice-roadmap)
- [Section H: Golden Rules](#section-h-golden-rules)
- [Quick Revision — One-Liners](#quick-revision--one-liners)

---

# Section A: What is Regex?

```
REGEX = Regular Expression

It is a structure that describes text.
Every email, phone, date — they are all BUILT a certain way.
Regex lets you describe that structure formally.

TWO USES:
  SEARCH   → you have a big text
             find everything built that structure
             
  VALIDATE → you have one specific input
             check if it is built that structure → yes or no

ANALOGY:
  Think of regex like a search bar but smarter.

  Normal search: finds exact word "cat"
  Regex search:  finds any 3-letter word ending in "at"
                 → cat, bat, rat, hat, mat, sat

WHY LEARN IT:
  → Validate emails, phone numbers, passwords
  → Find and replace text in files
  → Extract data from strings
  → Solve LeetCode string problems faster
  → Used in every professional codebase

HOW IT LOOKS:
  /pattern/flags

  /hello/          → finds the word "hello"
  /\d+/            → finds one or more digits
  /^[A-Z]/         → finds strings starting with a capital letter
  /\w+@\w+\.\w+/   → finds something that looks like an email
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section B: The 5 Building Blocks

```
Everything in regex is built from these 5 things.
Learn these and you can understand any pattern.

BUILDING BLOCK 1 → LITERAL
  Exact character. Matches itself.
  a → matches "a"
  3 → matches "3"

BUILDING BLOCK 2 → METACHARACTER
  Special character with a meaning.
  .  → matches ANY character
  \d → matches any digit
  \w → matches any word character

BUILDING BLOCK 3 → QUANTIFIER
  Says HOW MANY times something appears.
  *   → zero or more
  +   → one or more
  ?   → zero or one
  {3} → exactly 3

BUILDING BLOCK 4 → ANCHOR
  Says WHERE in the string to match.
  ^ → start of string
  $ → end of string

BUILDING BLOCK 5 → GROUP
  Wraps things together.
  (abc)     → capturing group
  [abc]     → character class
  (cat|dog) → alternation
```

[↑ Back to Table of Contents](#table-of-contents)

---

# How to Think About Regex

```
STEP 1 → What problem do I have?
          search   → I have big text, find things inside it
          validate → I have one input, check if it is correct

STEP 2 → What structure does that text follow?
          every email is built → something @ something . something
          every phone is built → 10 digits starting with 6-9
          every date is built  → 4 digits - 2 digits - 2 digits

STEP 3 → Describe that structure using tools
          TOOL 1 → exact characters     → literals
          TOOL 2 → types of characters  → \d \w \s []
          TOOL 3 → how many times       → quantifiers + * ? {n}
          TOOL 4 → where in string      → anchors ^ $
          TOOL 5 → grouping choices     → groups () []

STEP 4 → Use the right method
          test()    → validate  → yes or no
          match()   → search    → find matches
          replace() → clean     → swap matches
          split()   → cut       → split at matches

WHEN YOU SEE ANY REGEX — DO NOT memorize it
READ it as a sentence — a description of structure

  /^[6-9]\d{9}$/
  ^       → must START here
  [6-9]   → first character must be 6, 7, 8, or 9
  \d{9}   → followed by exactly 9 more digits
  $       → must END here
  → "10 digit number starting with 6 to 9"
  → that is an Indian phone number structure

REMEMBER FOREVER:
  See a structure in real life
        ↓
  Describe it using regex tools
        ↓
  Let computer search or validate it
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Level 1 — Foundation (80% of real problems)

```
These are the most common AND easiest regex concepts.
Master these first. They solve 80% of everything you will face.
Days 1-6 of your roadmap.
```

---

## Category 1: Literal Characters

```
WHAT:   Match the exact character you type.
        No tricks. No special meaning.
        a matches "a". 3 matches "3". hello matches "hello".

USED IN: Every single regex pattern. You cannot avoid this.
```

```javascript
// LITERAL MATCH
/cat/      → matches "cat" in "the cat sat"
/hello/    → matches "hello" in "say hello world"
/123/      → matches "123" in "order 123 placed"

// HOW TO USE IN JAVASCRIPT
const str = "the cat sat on the mat";
str.match(/cat/);     // ["cat"]
str.match(/cat/g);    // ["cat"] — g flag finds ALL matches

// SPECIAL CHARACTERS THAT NEED ESCAPING
// These have special meaning in regex: . * + ? ^ $ { } [ ] | ( ) \
// To match them literally, put \ before them

/\./    → matches a real dot "."
/\*/    → matches a real star "*"
/\$/    → matches a real dollar sign "$"
/\(/    → matches a real open bracket "("

// EXAMPLE
"price is $5.00".match(/\$\d+\.\d+/)   // ["$5.00"] ✓
```

```
REMEMBER:
  Normal letter or number → just type it
  Special character (. * + ? ^ $ { } [ ] | ( ) \) → put \ before it
```

```
Practice:
□ Match the word "error" in a log string
□ Match the price "$9.99" (escape $ and .)
□ Match the version "v1.0.0" (escape the dots)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 2: Anchors

```
WHAT:   Anchors do NOT match characters.
        They match POSITIONS in the string.
        ^ = start position
        $ = end position
        \b = word boundary position

USED IN: Validation. Every time you validate a whole string,
         you need ^ and $. Used constantly.
```

```javascript
// ^ — match at START of string
/^Hello/     → "Hello world"   ✓    "Say Hello"   ✗

// $ — match at END of string
/world$/     → "Hello world"   ✓    "world peace" ✗

// ^ AND $ together — match ENTIRE string
/^hello$/    → "hello"         ✓    "say hello"   ✗
              → useful for validation (is the WHOLE input exactly this?)

// \b — word BOUNDARY
/\bcat\b/    → "the cat sat"   ✓    "concatenate" ✗

// REAL EXAMPLES
// Validate: does string START with a capital letter?
/^[A-Z]/.test("Hello")         // true  ✓
/^[A-Z]/.test("hello")         // false ✗

// Validate: is the WHOLE string a 4-digit number?
/^\d{4}$/.test("1234")         // true  ✓
/^\d{4}$/.test("12345")        // false ✗
/^\d{4}$/.test("ab12")         // false ✗
```

```
KEY INSIGHT:
  Without anchors → pattern matches ANYWHERE in string
  With ^ and $    → pattern must match the WHOLE string
  This is why ^ and $ are essential for VALIDATION
```

```
Practice:
□ Match strings that START with "Error:"
□ Match strings that END with ".js"
□ Validate that ENTIRE string is exactly 5 letters
□ Match word "end" but not "ending" (use \b)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 3: Character Classes

```
WHAT:   Match ONE character from a SET of allowed characters.
        [abc] means "match a OR b OR c"
        Think of it as a menu: pick one item from the list.

USED IN: Very common. Anywhere you need "one of these characters."
```

```javascript
// BASIC CHARACTER CLASS
/[aeiou]/      → matches any ONE vowel
/[0-9]/        → matches any ONE digit
/[a-z]/        → matches any ONE lowercase letter
/[A-Z]/        → matches any ONE uppercase letter
/[a-zA-Z]/     → matches any ONE letter (upper or lower)
/[a-zA-Z0-9]/  → matches any ONE letter or digit

// RANGES use - inside []
/[a-f]/        → matches a, b, c, d, e, or f
/[1-5]/        → matches 1, 2, 3, 4, or 5

// NEGATED CLASS — ^ inside [] means NOT
/[^aeiou]/     → matches any character that is NOT a vowel
/[^0-9]/       → matches any character that is NOT a digit

// EXAMPLES
"hello123".match(/[a-z]+/g)    // ["hello"]
"hello123".match(/[0-9]+/g)    // ["123"]
"h3ll0".match(/[^a-z0-9]/g)   // null (all are letters or digits)

// REAL USE — validate hex color code
/^#[0-9A-Fa-f]{6}$/.test("#FF5733")   // true  ✓
/^#[0-9A-Fa-f]{6}$/.test("#ZZZZZZ")   // false ✗
```

```
REMEMBER:
  [abc]  → match a OR b OR c
  [a-z]  → range: any lowercase letter
  [^abc] → NOT a, b, or c
  - inside [] means range ONLY when between two characters
  ^ inside [] means NOT (only when first character)
```

```
Practice:
□ Match any single vowel
□ Match any character that is NOT a digit
□ Match a single hex digit [0-9A-Fa-f]
□ Match any uppercase letter
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 4: Shorthand Classes

```
WHAT:   Built-in shortcuts for common character classes.
        Instead of writing [0-9] every time, just write \d.

USED IN: Extremely common. Used in almost every real pattern.
```

```javascript
// THE 6 SHORTHANDS
\d    → [0-9]              any digit
\D    → [^0-9]             any NON-digit
\w    → [a-zA-Z0-9_]      any word character
\W    → [^a-zA-Z0-9_]     any NON-word character
\s    → [ \t\r\n\f]        any whitespace
\S    → [^ \t\r\n\f]       any NON-whitespace
.     → any character EXCEPT newline

// EXAMPLES
"hello 123".match(/\d+/g)    // ["123"]
"hello 123".match(/\w+/g)    // ["hello","123"]
"hello 123".match(/\s/g)     // [" "]
"hello 123".match(/\D+/g)    // ["hello "]

// REAL USE
// Remove all digits
"h3ll0 w0rld".replace(/\d/g, "")      // "hll wrld"

// Remove all whitespace
"hello   world".replace(/\s+/g, " ")  // "hello world"

// Find all words
"one two three".match(/\w+/g)         // ["one","two","three"]

// Check if string has only digits
/^\d+$/.test("12345")     // true  ✓
/^\d+$/.test("123ab")     // false ✗
```

```
QUICK REFERENCE:
  \d  digit         \D  NOT digit
  \w  word char     \W  NOT word char
  \s  whitespace    \S  NOT whitespace
  .   any char (except newline)

  RULE: Capital letter = opposite of lowercase
```

```
Practice:
□ Find all numbers in a string: match(/\d+/g)
□ Remove all non-word characters: replace(/\W/g, "")
□ Split on any whitespace: split(/\s+/)
□ Check if string has any whitespace: /\s/.test(str)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 5: Quantifiers

```
WHAT:   Say HOW MANY times the previous character or group repeats.
        Without quantifiers → match exactly ONE.
        With quantifiers → match zero, one, many, or a specific count.

USED IN: Almost every pattern. You cannot write useful regex without these.
```

```javascript
// THE QUANTIFIERS
*      → 0 or more
+      → 1 or more (at least one required)
?      → 0 or 1   (optional)
{n}    → exactly n
{n,}   → n or more
{n,m}  → between n and m

// EXAMPLES
/\d*/     → matches "" or "1" or "123" or "99999"
/\d+/     → matches "1" or "123" — NOT "" (needs at least 1)
/\d?/     → matches "" or "5"  — only 0 or 1 digit
/\d{3}/   → matches exactly "123" — NOT "12" or "1234"
/\d{2,4}/ → matches "12" or "123" or "1234"
/\d{3,}/  → matches "123" or "12345678" — at least 3

// REAL EXAMPLES
// Phone: exactly 10 digits
/^\d{10}$/.test("9876543210")   // true  ✓
/^\d{10}$/.test("98765")        // false ✗

// Password: 8 or more characters
/^.{8,}$/.test("mypassword")    // true  ✓
/^.{8,}$/.test("short")         // false ✗

// Optional "s" at end of word
/cats?/.test("cat")             // true  ✓
/cats?/.test("cats")            // true  ✓
```

```
QUANTIFIER SUMMARY:
  *      → 0 or more   (optional, many)
  +      → 1 or more   (required, many)
  ?      → 0 or 1      (optional, once)
  {3}    → exactly 3
  {3,}   → 3 or more
  {3,6}  → 3 to 6
```

```
Practice:
□ Match one or more digits
□ Match an optional "s": /cats?/
□ Match exactly 3 letters: /[a-z]{3}/
□ Match 4 to 8 word characters: /\w{4,8}/
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 6: Flags g and i

```
WHAT:   Flags change HOW the pattern searches.
        g and i are the two most used flags by far.
        Written after the closing slash: /pattern/gi

USED IN: Almost every real code pattern uses at least g or i.
```

```javascript
// g — global: find ALL matches, not just the first
"cat bat rat".match(/[a-z]at/)     // ["cat"]           — first only
"cat bat rat".match(/[a-z]at/g)    // ["cat","bat","rat"] — all ✓

// i — case insensitive: A = a
/hello/i.test("HELLO")    // true  ✓
/hello/i.test("Hello")    // true  ✓
/hello/.test("HELLO")     // false ✗ (without i)

// COMBINING g and i
"The the THE".match(/the/gi)   // ["The","the","THE"] ✓

// REAL EXAMPLES

// Count how many times a word appears
"cat and cat and cat".match(/cat/g).length   // 3

// Replace all (not just first)
"a.b.c.d".replace(/\./g, "-")    // "a-b-c-d" ✓
"a.b.c.d".replace(/\./, "-")     // "a-b.c.d" ✗ first only

// Find all emails (case insensitive domain)
"User@Gmail.COM user@yahoo.com".match(/\w+@\w+\.\w+/gi)
// ["User@Gmail.COM","user@yahoo.com"]
```

```
FLAG SUMMARY (g and i):
  g → find ALL matches (without g → only first match)
  i → ignore case (A = a = A)

  MOST COMMON MISTAKE: forgetting g flag when you need all matches
```

```
Practice:
□ Find all vowels in a string: match(/[aeiou]/gi)
□ Replace all spaces with dashes: replace(/\s/g, "-")
□ Check if string contains "hello" regardless of case: /hello/i
□ Count occurrences of a word
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 1 Pattern Picker

```
"What do I need?"

  Match exact text              → Literal: /hello/
  Validate start of string      → Anchor: /^pattern/
  Validate end of string        → Anchor: /pattern$/
  Validate whole string         → Both: /^pattern$/
  One character from a set      → Class: [abc] or [a-z]
  Any digit                     → \d
  Any letter or digit           → \w
  Any whitespace                → \s
  Any character at all          → .
  Repeated characters           → Quantifier + * ? {n}
  Optional character            → ?
  Find all matches              → g flag
  Case insensitive              → i flag
```

---

# Level 2 — Intermediate (15% more)

```
You know Level 1 well. Now add these.
They are used regularly in real code but need more thought.
Days 7-12 of your roadmap.
```

---

## Category 7: Groups and Capturing

```
WHAT:   Wrap a pattern in () to group it AND remember the match.
        The remembered part is called a "capture group."
        Access it with match()[1], match()[2], etc.

USED IN: Extracting specific parts from a match. Date parsing,
         email parsing, reformatting strings.
```

```javascript
// BASIC CAPTURING GROUP
/(cat)/        → matches "cat", captures "cat" in group 1
/(ha)+/        → matches "ha" "haha" "hahaha"
/(\d{4})/      → matches 4 digits, captures them

// ACCESSING CAPTURE GROUPS
const result = "2024-03-15".match(/(\d{4})-(\d{2})-(\d{2})/);
result[0]   // "2024-03-15"  full match
result[1]   // "2024"        group 1 — year
result[2]   // "03"          group 2 — month
result[3]   // "15"          group 3 — day

// USE IN replace() — $1 $2 refer to captured groups
"2024-03-15".replace(/(\d{4})-(\d{2})-(\d{2})/, "$3/$2/$1")
// "15/03/2024" — reformatted date ✓

// REAL EXAMPLES
// Extract domain from email
"user@gmail.com".match(/\w+@(\w+\.\w+)/)[1]   // "gmail.com"

// Extract area code from phone
"(123) 456-7890".match(/\((\d{3})\)/)[1]      // "123"
```

```
REMEMBER:
  ()       → captures the match
  result[0] → full match
  result[1] → first group
  result[2] → second group
  $1 $2    → refer to groups inside replace()
```

```
Practice:
□ Match a date and capture year, month, day separately
□ Extract domain from email address
□ Reformat date from YYYY-MM-DD to DD/MM/YYYY using replace()
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 8: Alternation

```
WHAT:   The OR operator in regex.
        (cat|dog) means "match cat OR dog."

USED IN: Matching multiple valid options. File extensions,
         keywords, country codes.
```

```javascript
// BASIC ALTERNATION
/cat|dog/         → matches "cat" OR "dog"
/yes|no|maybe/    → matches "yes" OR "no" OR "maybe"
/(cat|dog)s/      → matches "cats" OR "dogs"

// ALTERNATION IN CHARACTER CLASS vs GROUP
/[abc]/           → match ONE character: a OR b OR c
/(abc|def)/       → match WHOLE WORD: "abc" OR "def"

// REAL EXAMPLES
// Match image file extension
/\.(jpg|jpeg|png|gif|webp)$/i
"photo.JPG".match(/\.(jpg|jpeg|png|gif|webp)$/i)   // [".JPG","JPG"] ✓
"doc.pdf".match(/\.(jpg|jpeg|png|gif|webp)$/i)     // null ✗

// Match "color" or "colour" (British vs American)
/colou?r/         → u is optional → matches both ✓
/color|colour/    → explicit OR → also works ✓

// Match multiple keywords
const log = "ERROR: file not found";
/error|warning|info/i.test(log)   // true ✓
```

```
REMEMBER:
  | means OR
  Always use () around alternation to control scope
  /cat|dogs/ → "cat" OR "dogs"  (not "cat" OR "dog" + s)
  /(cat|dog)s/ → "cats" OR "dogs" (s applies to both)
```

```
Practice:
□ Match "color" or "colour"
□ Match any image extension: jpg, png, gif, webp
□ Match "Mr" or "Mrs" or "Ms" or "Dr"
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 9: Non-Capturing Groups

```
WHAT:   (?:) groups a pattern WITHOUT remembering the match.
        Use when you need grouping but do not need to capture.

USED IN: When you need alternation or repetition but do not
         need to extract the group. Keeps match() array clean.
```

```javascript
// CAPTURING vs NON-CAPTURING
/(\d{4})-(\d{2})/      → captures both groups → result[1] result[2]
/(?:\d{4})-(?:\d{2})/  → groups but NO capture → result[1] is undefined

// WHEN TO USE (?:)
// You need the group for structure but not for extraction

// Example: match "ha" repeated
/(ha)+/.exec("hahaha")    // ["hahaha", "ha"] — "ha" captured (last repeat)
/(?:ha)+/.exec("hahaha")  // ["hahaha"]        — nothing captured

// Example: alternation without capture
/(?:cat|dog)s/.exec("cats")    // ["cats"] — no group captured
/(cat|dog)s/.exec("cats")      // ["cats","cat"] — "cat" captured

// REAL USE
// Match protocol but don't capture it
/(?:https?:\/\/)?([\w.-]+)/
"https://google.com".match(/(?:https?:\/\/)?([\w.-]+)/)[1]
// "google.com" — only domain captured, not protocol
```

```
REMEMBER:
  ()    → group AND capture   → shows up in result array
  (?:)  → group but NO capture → does NOT show up in result array
  Use (?:) when you want grouping for structure only
```

```
Practice:
□ Match "http://" or "https://" without capturing it
□ Match repeated "ha" without capturing
□ Use alternation without polluting the capture groups
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 10: Greedy vs Lazy

```
WHAT:   By default quantifiers are GREEDY — match as MUCH as possible.
        Add ? after quantifier to make LAZY — match as LITTLE as possible.

USED IN: Matching HTML tags, quoted strings, anything between delimiters.
         Essential for avoiding over-matching.
```

```javascript
// GREEDY (default) — matches as MUCH as possible
/<.+>/    → on "<b>bold</b>" matches the WHOLE thing "<b>bold</b>"

// LAZY — matches as LITTLE as possible
/<.+?>/   → on "<b>bold</b>" matches "<b>" then "</b>" separately ✓

// ALL LAZY VERSIONS
*?    → 0 or more (lazy)
+?    → 1 or more (lazy)
??    → 0 or 1    (lazy)
{n,m}?→ between n and m (lazy)

// REAL EXAMPLES
const html = "<b>bold</b> and <i>italic</i>";

// GREEDY — wrong
html.match(/<.+>/)[0]    // "<b>bold</b> and <i>italic</i>" — too much!

// LAZY — correct
html.match(/<.+?>/g)     // ["<b>","</b>","<i>","</i>"] ✓

// Extract text between quotes
'"hello" and "world"'.match(/".*?"/g)   // ['"hello"','"world"'] ✓
'"hello" and "world"'.match(/".*"/g)    // ['"hello" and "world"'] ✗ greedy

// Extract text between parentheses
"(first) and (second)".match(/\(.*?\)/g)  // ["(first)","(second)"] ✓
```

```
REMEMBER:
  Greedy = as MUCH as possible (default)
  Lazy   = as LITTLE as possible (add ? after quantifier)

  USE LAZY WHEN:
  → Matching HTML tags: <.+?>
  → Matching quoted strings: ".*?"
  → Matching anything between two delimiters
```

```
Practice:
□ Match all HTML tags in a string
□ Extract all quoted strings
□ Match text inside parentheses
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 11: Flags m and s

```
WHAT:   Two less common but useful flags.
        m changes what ^ and $ mean.
        s makes . match newlines too.

USED IN: Multi-line text processing, log parsing, HTML parsing.
```

```javascript
// m — multiline: ^ and $ match start/end of EACH LINE
const text = "line1\nline2\nline3";

text.match(/^\w+/g)    // ["line1"]              — only string start
text.match(/^\w+/gm)   // ["line1","line2","line3"] — each line start ✓

text.match(/\w+$/g)    // ["line3"]              — only string end
text.match(/\w+$/gm)   // ["line1","line2","line3"] — each line end ✓

// s — dotAll: . matches newline too
/hello.world/.test("hello\nworld")    // false ✗ (. skips newline)
/hello.world/s.test("hello\nworld")   // true  ✓ (s flag)

// REAL EXAMPLES
// Find all lines that start with ERROR
const logs = "INFO: ok\nERROR: fail\nERROR: crash";
logs.match(/^ERROR.+/gm)   // ["ERROR: fail","ERROR: crash"] ✓

// Match multi-line HTML content
/<div>.*?<\/div>/s.exec("<div>\nhello\n</div>")   // matches ✓
```

```
FLAG SUMMARY (m and s):
  m → ^ and $ match each LINE (not just whole string)
  s → . matches newline too (normally . skips \n)
```

```
Practice:
□ Find all lines starting with "ERROR:" in a log
□ Match content between <div> tags across multiple lines
□ Count lines in a multiline string
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 12: Backreferences

```
WHAT:   \1 \2 refer back to what was captured in group 1, group 2.
        Match the SAME text that the group matched.

USED IN: Finding repeated words, detecting duplicates,
         matching opening and closing tags.
```

```javascript
// BASIC BACKREFERENCE
/(\w+) \1/    → matches a word followed by the SAME word
"hello hello".match(/(\w+) \1/)    // ["hello hello","hello"] ✓
"hello world".match(/(\w+) \1/)    // null ✗ (different words)

// FIND REPEATED WORDS IN TEXT
"the the quick brown fox".match(/\b(\w+) \1\b/g)
// ["the the"] ✓ — finds the duplicate

// MATCH SAME OPENING AND CLOSING TAG
/<(\w+)>.*?<\/\1>/s
"<b>hello</b>".match(/<(\w+)>.*?<\/\1>/s)    // ✓ — b matches b
"<b>hello</i>".match(/<(\w+)>.*?<\/\1>/s)    // null ✗ — b ≠ i

// FIND DUPLICATE CHARACTERS
/(.)\1/
"aabbcc".match(/(.)\1/g)   // ["aa","bb","cc"]
"hello".match(/(.)\1/g)    // ["ll"]
```

```
REMEMBER:
  \1 → same text as group 1
  \2 → same text as group 2
  Different from $1 $2 which are used inside replace()
  \1 is used inside the PATTERN itself
  $1 is used inside the REPLACEMENT string
```

```
Practice:
□ Find repeated words: /\b(\w+) \1\b/
□ Find doubled letters: /(.)\1/
□ Match valid HTML opening and closing tag pairs
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 2 Pattern Picker

```
"What do I need?"

  Extract part of a match       → Capturing group: (\d+)
  Match this OR that            → Alternation: (a|b|c)
  Group without extracting      → Non-capturing: (?:abc)
  Match between delimiters      → Lazy quantifier: .*?
  Find repeated words           → Backreference: (\w+) \1
  Match start of each line      → m flag + ^
  Make . match newlines         → s flag
  Find all + ignore case        → gi flags combined
```

---

# Level 3 — Advanced (4% more)

```
You are comfortable with Levels 1 and 2.
These are powerful but used less often.
Days 13-18 of your roadmap.
```

---

## Category 13: Lookahead

```
WHAT:   Match X only IF X is followed (or not followed) by Y.
        The lookahead part is a CONDITION — it never gets included
        in the match result.

USED IN: Password validation, extracting values before a unit,
         complex string conditions.
```

```javascript
// POSITIVE LOOKAHEAD (?=...)
// Match X only if FOLLOWED BY Y
/\d+(?= dollars)/
"100 dollars".match(/\d+(?= dollars)/)    // ["100"] ✓
"100 euros".match(/\d+(?= dollars)/)      // null    ✗

// NEGATIVE LOOKAHEAD (?!...)
// Match X only if NOT FOLLOWED BY Y
/\d+(?! dollars)/
"100 euros".match(/\d+(?! dollars)/)      // ["100"] ✓
"100 dollars".match(/\d+(?! dollars)/)    // null    ✗

// REAL EXAMPLES
// Extract filename without extension
"photo.jpg".match(/\w+(?=\.)/)            // ["photo"] ✓

// Check password has at least one uppercase
/(?=.*[A-Z])/.test("Hello123")            // true  ✓
/(?=.*[A-Z])/.test("hello123")            // false ✗

// Strong password: uppercase + digit + min 8 chars
/^(?=.*[A-Z])(?=.*\d).{8,}$/
// Chain multiple lookaheads for multiple conditions
"MyPass1word".match(/^(?=.*[A-Z])(?=.*\d).{8,}$/)   // ✓
"mypassword".match(/^(?=.*[A-Z])(?=.*\d).{8,}$/)    // null ✗
```

```
REMEMBER:
  (?=Y)  → must be FOLLOWED BY Y     (positive)
  (?!Y)  → must NOT be followed by Y (negative)
  Lookahead is a CONDITION not a match — never included in result
```

```
Practice:
□ Match number only if followed by "kg": /\d+(?=kg)/
□ Match word not followed by "!": /\w+(?!!)/
□ Validate password has uppercase AND digit
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 14: Lookbehind

```
WHAT:   Match X only IF X is preceded (or not preceded) by Y.
        Same idea as lookahead but looks BEHIND instead of ahead.

USED IN: Extracting values after a symbol, currency parsing,
         conditional matching based on what came before.
```

```javascript
// POSITIVE LOOKBEHIND (?<=...)
// Match X only if PRECEDED BY Y
/(?<=\$)\d+/
"$100".match(/(?<=\$)\d+/)    // ["100"] ✓  — digits after $
"100".match(/(?<=\$)\d+/)     // null    ✗  — no $ before

// NEGATIVE LOOKBEHIND (?<!...)
// Match X only if NOT PRECEDED BY Y
/(?<!\$)\d+/
"100".match(/(?<!\$)\d+/)     // ["100"] ✓
"$100".match(/(?<!\$)\d+/)    // null    ✗

// REAL EXAMPLES
// Extract price value without the $ symbol
"price: $49.99".match(/(?<=\$)\d+\.?\d*/)[0]    // "49.99" ✓

// Extract number after # (like issue numbers)
"issue #142".match(/(?<=#)\d+/)[0]              // "142" ✓

// Match word only if it comes after "Mr." or "Ms."
/(?<=Mr\.|Ms\.)\s*\w+/
"Mr. Smith".match(/(?<=Mr\.|Ms\.)\s*\w+/)[0]    // " Smith" ✓
```

```
REMEMBER:
  (?<=Y)  → must be PRECEDED BY Y     (positive)
  (?<!Y)  → must NOT be preceded by Y (negative)

  LOOKAHEAD vs LOOKBEHIND:
  Lookahead  → condition is AFTER  the match: X(?=Y)
  Lookbehind → condition is BEFORE the match: (?<=Y)X
```

```
Practice:
□ Extract digits after "$": /(?<=\$)\d+/
□ Match digits after "#": /(?<=#)\d+/
□ Match word only if NOT preceded by "no ": /(?<!no )\w+/
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 15: Named Capture Groups

```
WHAT:   Give your capture groups a name instead of a number.
        Access by name instead of index.
        Makes complex patterns readable and self-documenting.

USED IN: Complex parsing where you extract multiple parts.
         Professional production code.
```

```javascript
// BASIC NAMED GROUP
/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/

const m = "2024-03-15".match(/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/);
m.groups.year    // "2024"
m.groups.month   // "03"
m.groups.day     // "15"

// COMPARE: numbered vs named
// Numbered — hard to read
result[1]   result[2]   result[3]

// Named — self-documenting ✓
m.groups.year   m.groups.month   m.groups.day

// USE IN replace()
"2024-03-15".replace(
  /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/,
  "$<day>/$<month>/$<year>"
)
// "15/03/2024" ✓

// REAL EXAMPLE — parse URL parts
const urlPattern = /(?<protocol>https?):\/\/(?<domain>[\w.]+)(?<path>\/.*)?/;
const url = "https://www.example.com/about";
const { protocol, domain, path } = url.match(urlPattern).groups;
// protocol = "https"
// domain   = "www.example.com"
// path     = "/about"
```

```
REMEMBER:
  (?<name>pattern) → named capture group
  result.groups.name → access by name
  $<name> → use in replace()
  Always use named groups when pattern has 3+ capture groups
```

```
Practice:
□ Parse a date and access parts by name
□ Extract URL parts (protocol, domain, path)
□ Parse "firstName lastName" with named groups
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 3 Pattern Picker

```
"What do I need?"

  Match only if followed by X        → Positive lookahead:  (?=X)
  Match only if NOT followed by X    → Negative lookahead:  (?!X)
  Match only if preceded by X        → Positive lookbehind: (?<=X)
  Match only if NOT preceded by X    → Negative lookbehind: (?<!X)
  Multiple conditions on one match   → Chain lookaheads: (?=.*X)(?=.*Y)
  Name your capture groups           → Named group: (?<name>)
  Access captured parts by name      → result.groups.name
  Complex password validation        → Lookahead chain + anchors
```

---

# Level 4 — Specialist (Remaining 1%)

```
Learn these as you encounter them.
Not needed for everyday work but useful in specific situations.
```

| # | Feature | Use When | Example |
|---|---------|----------|---------|
| 1 | Unicode \p{L} | Match letters from any language | /\p{L}+/u |
| 2 | Unicode \p{N} | Match numbers from any script | /\p{N}+/u |
| 3 | Unicode \p{Emoji} | Match emoji characters | /\p{Emoji}/u |
| 4 | \p{Sc} Currency | Match any currency symbol | /\p{Sc}\d+/u |
| 5 | Atomic Groups (?>)| Prevent backtracking | Advanced parsers |
| 6 | Possessive + | No backtrack version of + | Performance critical |
| 7 | Conditional (?if) | If group matched then X else Y | Complex parsers |
| 8 | Recursive patterns | Match nested structures | JSON, HTML parsers |

```javascript
// UNICODE EXAMPLES (require u flag)
/\p{L}+/u.test("héllo")         // true  ✓ — matches accented letters
/\p{L}+/u.test("こんにちは")      // true  ✓ — matches Japanese
/\p{Emoji}/u.test("hello 😀")    // true  ✓ — matches emoji
/\p{Sc}\d+/u.test("€50")        // true  ✓ — matches currency + amount
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section C: Regex in JavaScript

```
HOW TO USE REGEX IN JAVASCRIPT CODE
There are 6 methods. Each does something different.
```

```javascript
// METHOD 1: test() → returns true or false
// Use for: VALIDATION
/^\d{5}$/.test("12345")    // true  ✓
/^\d{5}$/.test("1234")     // false ✗

// METHOD 2: match() → returns array of matches or null
// Use for: FINDING and EXTRACTING
"hello world".match(/\w+/)      // ["hello"] — first match only
"hello world".match(/\w+/g)     // ["hello","world"] — all (g flag)
"abc".match(/\d/)               // null — no match

// match() result structure
const result = "2024-03-15".match(/(\d{4})-(\d{2})-(\d{2})/);
result[0]   // "2024-03-15" — full match
result[1]   // "2024"       — group 1
result[2]   // "03"         — group 2
result[3]   // "15"         — group 3

// METHOD 3: replace() → returns new string with replacements
// Use for: CLEANING and FORMATTING
"hello world".replace(/\s/, "-")     // "hello-world"  — first only
"hello world".replace(/\s/g, "-")    // "hello-world"  — all (g flag)

// replace() with capture groups
"2024-03-15".replace(/(\d{4})-(\d{2})-(\d{2})/, "$3/$2/$1")
// "15/03/2024" — $1 $2 $3 refer to captured groups

// replace() with function
"hello world".replace(/\w+/g, w => w.toUpperCase())
// "HELLO WORLD"

// METHOD 4: replaceAll() → replaces all without needing g flag
"a.b.c".replaceAll(".", "-")    // "a-b-c"

// METHOD 5: split() → split string into array
"one two  three".split(/\s+/)   // ["one","two","three"]
"2024-03-15".split(/-/)         // ["2024","03","15"]

// METHOD 6: search() → returns index of first match or -1
"hello world".search(/world/)   // 6
"hello world".search(/xyz/)     // -1
```

```
METHOD PICKER:
  "Does it match?"          → test()        → true/false
  "Find the match"          → match()       → array or null
  "Find all matches"        → match(/g)     → array or null
  "Replace something"       → replace()     → new string
  "Split into pieces"       → split()       → array
  "Where is the match?"     → search()      → index or -1
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section D: Real World Patterns

```
These are the patterns used most in professional code.
Each one is tested, explained, and ready to copy.
```

---

## Email

```javascript
// SIMPLE (good enough for most cases)
/^[^\s@]+@[^\s@]+\.[^\s@]+$/

"user@gmail.com".match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)     // ✓
"usergmail.com".match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)      // null ✗
"user @gmail.com".match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)    // null ✗
```

---

## Phone Number

```javascript
// INDIAN (10 digits, starts with 6-9)
/^[6-9]\d{9}$/
"9876543210".match(/^[6-9]\d{9}$/)    // ✓

// WITH OPTIONAL +91 OR 0
/^(?:\+91|0)?[6-9]\d{9}$/
"+919876543210".match(/^(?:\+91|0)?[6-9]\d{9}$/)   // ✓

// US PHONE
/^\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})$/
"(123) 456-7890".match(...)   // ✓
"123.456.7890".match(...)     // ✓
```

---

## Password Strength

```javascript
// STRONG: min 8 chars, one uppercase, one lowercase, one digit, one special
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/

"MyPass@1".match(...)      // ✓
"mypassword".match(...)    // null ✗
```

---

## Date Formats

```javascript
// YYYY-MM-DD
/^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$/
"2024-03-15".match(...)    // ✓
"2024-13-01".match(...)    // null ✗

// DD/MM/YYYY
/^(0[1-9]|[12]\d|3[01])\/(0[1-9]|1[0-2])\/\d{4}$/
"15/03/2024".match(...)    // ✓
```

---

## URL

```javascript
/^(https?:\/\/)?([\da-z.-]+)\.([a-z.]{2,6})([/\w .-]*)*\/?$/i

"https://www.google.com".match(...)         // ✓
"http://example.com/path/page".match(...)   // ✓
```

---

## Username

```javascript
// 3-20 chars, letters digits underscore, starts with letter
/^[a-zA-Z][a-zA-Z0-9_]{2,19}$/

"john_doe".match(...)   // ✓
"_john".match(...)      // null ✗ (starts with _)
"jo".match(...)         // null ✗ (too short)
```

---

## Hex Color

```javascript
// #RGB or #RRGGBB
/^#([0-9A-Fa-f]{3}|[0-9A-Fa-f]{6})$/

"#FF5733".match(...)    // ✓
"#FFF".match(...)       // ✓
"#GGGGGG".match(...)    // null ✗
```

---

## Strip HTML Tags

```javascript
"<b>hello</b> <i>world</i>".replace(/<[^>]*>/g, "")
// "hello world" ✓
```

---

## IP Address

```javascript
// Basic IPv4
/^(\d{1,3}\.){3}\d{1,3}$/
"192.168.1.1".match(...)    // ✓
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section E: Master Pattern Picker

```
"What do I need to match?"

  LEVEL 1
  Exact word or phrase          → Literal: /hello/
  Start of string               → /^pattern/
  End of string                 → /pattern$/
  Whole string validation       → /^pattern$/
  One character from a set      → [abc] or [a-z]
  Any digit                     → \d
  Any letter or digit           → \w
  Any whitespace                → \s
  Any character                 → .
  Repeated characters           → + * ? {n}
  Optional character            → ?
  Find all matches              → g flag
  Case insensitive              → i flag

  LEVEL 2
  Extract part of match         → Capturing group ()
  Match this OR that            → Alternation (a|b)
  Group without extracting      → Non-capturing (?:)
  Match between delimiters      → Lazy quantifier .*?
  Find repeated words           → Backreference \1
  Match each line start/end     → m flag with ^ $
  Make . match newlines         → s flag

  LEVEL 3
  Match only if followed by X   → Lookahead (?=X)
  Match only if NOT followed    → Negative lookahead (?!X)
  Match only if preceded by X   → Lookbehind (?<=X)
  Match only if NOT preceded    → Negative lookbehind (?<!X)
  Multiple conditions           → Chain lookaheads
  Name capture groups           → (?<name>pattern)

  LEVEL 4
  International characters      → Unicode \p{L} with u flag
  Emoji matching                → \p{Emoji} with u flag
```

```
"Which JavaScript method do I use?"

  Just check if it matches      → .test()
  Get the matched text          → .match()
  Get all matched texts         → .match() with g flag
  Replace matched text          → .replace()
  Split string at pattern       → .split()
  Find position of match        → .search()
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section F: Common Mistakes

```
MISTAKE 1: Forgetting to escape special characters
  ✗ /3.14/    → . matches ANY character
  ✓ /3\.14/   → \. matches only a real dot

MISTAKE 2: Forgetting g flag when you need ALL matches
  ✗ "aaa".match(/a/)    → ["a"]         — only first
  ✓ "aaa".match(/a/g)   → ["a","a","a"] — all

MISTAKE 3: Not using ^ and $ for validation
  ✗ /\d{5}/      → passes "abc12345xyz" (has 5 digits somewhere)
  ✓ /^\d{5}$/    → only passes "12345" (whole string)

MISTAKE 4: Greedy matching too much
  ✗ /<.+>/    → matches entire "<b>text</b>" as one match
  ✓ /<.+?>/   → matches "<b>" and "</b>" separately

MISTAKE 5: Using . when you want \S or \w
  ✗ /.+/      → also matches spaces and newlines
  ✓ /\S+/     → only non-whitespace
  ✓ /\w+/     → only word characters

MISTAKE 6: Using match() without g and expecting all results
  ✗ "one two".match(/\w+/)    → ["one"]
  ✓ "one two".match(/\w+/g)   → ["one","two"]

MISTAKE 7: Not testing on regex101.com first
  Always test before putting regex in code.

MISTAKE 8: Overcomplicating the pattern
  If regex is longer than 50 characters, split into two steps.
  Readable code beats clever regex.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section G: 28-Day Practice Roadmap

## Week 1-2: Level 1 — Foundation

| Day | Topic | Practice Task |
|-----|-------|---------------|
| 1 | Literal Characters | Match your name, match "$9.99" |
| 2 | Anchors ^ $ | Validate 5-digit zip code |
| 3 | Character Classes [] | Match any vowel, any hex digit |
| 4 | Negated Classes [^] | Match anything that is NOT a digit |
| 5 | Shorthand \d \w \s | Find all words, find all numbers |
| 6 | Quantifiers * + ? | Match optional "s" at end of word |
| 7 | Exact Quantifiers {n} | Validate phone: exactly 10 digits |
| 8 | Flag g | Find ALL matches in a string |
| 9 | Flag i | Case insensitive search |
| 10 | Combine Level 1 | Validate an Indian phone number |
| 11 | Combine Level 1 | Validate a username (3-20 chars) |
| 12 | Combine Level 1 | Validate a hex color code |
| 13 | Combine Level 1 | Extract all numbers from a sentence |
| 14 | Review Level 1 | Write 5 patterns from scratch without help |

## Week 3: Level 2 — Intermediate

| Day | Topic | Practice Task |
|-----|-------|---------------|
| 15 | Capturing Groups | Extract year month day from date |
| 16 | replace() with groups | Reformat YYYY-MM-DD to DD/MM/YYYY |
| 17 | Alternation (a\|b) | Match jpg, png, gif extensions |
| 18 | Non-Capturing (?:) | Match protocol without capturing it |
| 19 | Greedy vs Lazy | Match HTML tags correctly |
| 20 | Flag m | Find all lines starting with ERROR |
| 21 | Backreferences \1 | Find repeated words in text |

## Week 4: Level 3 — Advanced

| Day | Topic | Practice Task |
|-----|-------|---------------|
| 22 | Positive Lookahead | Match number only before "kg" |
| 23 | Negative Lookahead | Match word not followed by "!" |
| 24 | Positive Lookbehind | Match digits only after "$" |
| 25 | Negative Lookbehind | Match digits NOT after "$" |
| 26 | Named Groups | Parse URL into protocol domain path |
| 27 | Strong Password | Validate with multiple lookaheads |
| 28 | Real World | Write regex for a problem you face at work |

**Day 29+:** Every time you write if/else for string validation, ask: "Can I use regex here?" Try it. Test on regex101.com.

[↑ Back to Table of Contents](#table-of-contents)

---

# Section H: Golden Rules

```
1.  "Test every pattern on regex101.com before using it in code."
2.  "Simple and readable beats clever and short."
3.  "Always use ^ and $ when validating the whole string."
4.  "Add g flag when you need all matches, not just the first."
5.  "Escape special characters: . * + ? ^ $ { } [ ] | ( ) \"
6.  "Use lazy quantifier +? or *? when matching between delimiters."
7.  "Use \d \w \s instead of long character classes."
8.  "Named capture groups make your code self-documenting."
9.  "If regex is getting complex, split it into two simpler ones."
10. "Capital shorthand = opposite: \D \W \S"
11. "Lookahead is a condition, not a match — never in the result."
12. "Level 1 solves 80%. Master it before moving to Level 2."
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Revision — One-Liners

```
LEVEL 1 — FOUNDATION
  Literal    = exact character. /hello/ matches "hello"
  ^          = start of string
  $          = end of string
  ^pattern$  = validate ENTIRE string
  \b         = word boundary
  [abc]      = match a OR b OR c (one character)
  [a-z]      = any lowercase letter
  [^abc]     = NOT a, b, or c
  \d         = digit [0-9]
  \w         = word char [a-zA-Z0-9_]
  \s         = whitespace
  .          = any char except newline
  Capital    = opposite: \D \W \S
  *          = 0 or more
  +          = 1 or more
  ?          = 0 or 1 (optional)
  {3}        = exactly 3
  {3,6}      = between 3 and 6
  g flag     = find ALL matches
  i flag     = case insensitive

LEVEL 2 — INTERMEDIATE
  ()         = capture group (remembered)
  result[1]  = first captured group
  $1 $2      = groups in replace()
  (a|b)      = alternation — match a OR b
  (?:abc)    = non-capturing group
  +?  *?     = lazy — match as little as possible
  m flag     = ^ and $ match each line
  s flag     = . matches newline too
  \1  \2     = backreference — same text as group 1, 2

LEVEL 3 — ADVANCED
  (?=Y)      = positive lookahead  — followed by Y
  (?!Y)      = negative lookahead  — NOT followed by Y
  (?<=Y)     = positive lookbehind — preceded by Y
  (?<!Y)     = negative lookbehind — NOT preceded by Y
  (?<name>)  = named capture group
  .groups.n  = access named group

LEVEL 4 — SPECIALIST
  \p{L}      = any unicode letter (u flag required)
  \p{Emoji}  = any emoji (u flag required)

JAVASCRIPT METHODS
  test()     = true/false  — "does it match?"
  match()    = array       — "what matched?"
  replace()  = new string  — "swap it"
  split()    = array       — "cut here"
  search()   = index       — "where is it?"
```

[↑ Back to Table of Contents](#table-of-contents)
