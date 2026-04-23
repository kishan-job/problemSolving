# Complete Guide to Events — JavaScript & React
### For Frontend Developers — Everything You Need to Know

---

## Table of Contents
1. What is an Event
2. Event Flow — Capturing, Target, Bubbling
3. JavaScript Events (Vanilla)
4. All Event Categories with Levels
5. Complete Event Object Reference
6. React Synthetic Events
7. Event Patterns in React
8. TypeScript Event Types
9. Common Mistakes and Best Practices
10. Interview Questions

---

## Section 1 — What is an Event

### Basic Concept
```
User does something (click, type, scroll)
      ↓
Browser detects it
      ↓
Browser creates an EVENT OBJECT
      ↓
Event object contains all info about what happened
      ↓
Your handler function receives it
```

### How Browser Detects Events
```
Browser constantly watches the DOM
      ↓
User interacts with an element
      ↓
Browser fires the corresponding event
      ↓
Any listeners attached to that event get notified
```

### JS vs React Difference
```
JavaScript (Vanilla)               React
──────────────────────────────     ──────────────────────────────
addEventListener('click', fn)  vs  onClick={fn}
lowercase event names          vs  camelCase event names
native browser Event object    vs  SyntheticEvent wrapper
listener on each element       vs  ONE listener at root (delegation)
must manually removeListener   vs  React handles cleanup
```

---

## Section 2 — Event Flow

### Three Phases of Every Event
```
                    document
                       │
                    <html>
                       │
                    <body>
                       │
                    <div>          ← PHASE 1: CAPTURING (top → down)
                       │
                    <button>       ← PHASE 2: TARGET (event origin)
                       │
                    <div>          ← PHASE 3: BUBBLING (bottom → up)
                       │
                    <body>
                       │
                    <html>
                       │
                    document
```

### Phase 1 — Capturing (top → down)
```javascript
// Capturing phase — fires BEFORE reaching target
element.addEventListener('click', fn, true) // true = capturing phase
// rarely used in practice
```

### Phase 2 — Target
```javascript
// The actual element that was clicked
// Both capturing and bubbling fire here
e.target // this is the target element
```

### Phase 3 — Bubbling (bottom → up)
```javascript
// Default behavior — fires AFTER target
element.addEventListener('click', fn)        // default = bubbling
element.addEventListener('click', fn, false) // explicit bubbling

// Example
<div onClick={handleDiv}>           // fires SECOND (bubbling)
  <button onClick={handleButton}>   // fires FIRST (target)
    Click
  </button>
</div>
```

### stopPropagation — Stop Bubbling
```javascript
// WITHOUT stopPropagation
// click button → button handler → div handler (unwanted!)

// WITH stopPropagation
const handleButton = (e) => {
  e.stopPropagation() // stops here — div never knows
}

// stopImmediatePropagation — stops OTHER listeners on SAME element too
const handleButton = (e) => {
  e.stopImmediatePropagation()
}
```

### preventDefault — Stop Browser Default
```javascript
// Form — default is page refresh
const handleSubmit = (e) => {
  e.preventDefault() // stops page refresh
}

// Link — default is navigation
const handleLink = (e) => {
  e.preventDefault() // stops navigation
}

// Checkbox — default is check/uncheck
const handleCheck = (e) => {
  e.preventDefault() // stops toggle
}

// Right click — default is context menu
const handleContextMenu = (e) => {
  e.preventDefault() // stops context menu
}
```

---

## Section 3 — JavaScript Events (Vanilla)

### addEventListener Syntax
```javascript
// Basic
element.addEventListener(eventType, handler)

// With options
element.addEventListener(eventType, handler, {
  once: true,     // fires only once then removes itself
  capture: true,  // use capturing phase
  passive: true   // never calls preventDefault (better scroll performance)
})
```

### removeEventListener
```javascript
// MUST use same function reference — not inline
function handleClick(e) {
  console.log('clicked')
}

element.addEventListener('click', handleClick)
element.removeEventListener('click', handleClick) // ✅ works

// WRONG — different function reference
element.addEventListener('click', () => console.log('clicked'))
element.removeEventListener('click', () => console.log('clicked')) // ❌ does nothing
```

### Three Ways to Attach Events
```html
<!-- WAY 1 — Inline HTML (avoid this) -->
<button onclick="handleClick()">Click</button>

<!-- WAY 2 — JavaScript addEventListener (vanilla JS way) -->
<script>
  document.getElementById('btn').addEventListener('click', handleClick)
</script>

<!-- WAY 3 — React JSX (React way) -->
<button onClick={handleClick}>Click</button>
```

---

## Section 4 — All Event Categories with Levels

---

### 🔴 Level 1 — Must Know (Daily Use)

#### Mouse Events
```jsx
<button onClick={fn}>           // single click — most used
<div onDoubleClick={fn}>        // double click
<div onContextMenu={fn}>        // right click
<div onMouseDown={fn}>          // mouse button pressed down
<div onMouseUp={fn}>            // mouse button released
```

#### Form / Input Events
```jsx
<input onChange={fn}>           // value changes — MOST USED
<form onSubmit={fn}>            // form submitted
<input onFocus={fn}>            // input activated/selected
<input onBlur={fn}>             // input deactivated/left
<select onChange={fn}>          // dropdown selection changed
<input type="checkbox"
       onChange={fn}>           // checkbox toggled
<textarea onChange={fn}>        // textarea value changed
```

#### Keyboard Events
```jsx
<input onKeyDown={fn}>          // key pressed down — MOST USED
<input onKeyUp={fn}>            // key released
// onKeyPress is DEPRECATED — use onKeyDown instead
```

---

### 🟡 Level 2 — Should Know (Used Regularly)

#### Mouse Position Events
```jsx
<div onMouseEnter={fn}>         // mouse enters element (no bubble)
<div onMouseLeave={fn}>         // mouse leaves element (no bubble)
<div onMouseOver={fn}>          // mouse over element (bubbles)
<div onMouseOut={fn}>           // mouse out of element (bubbles)
<div onMouseMove={fn}>          // mouse moving over element
```

#### Clipboard Events
```jsx
<input onCopy={fn}>             // user copies text
<input onPaste={fn}>            // user pastes text
<input onCut={fn}>              // user cuts text
```

#### Scroll and Wheel Events
```jsx
<div onScroll={fn}>             // element is scrolled
<div onWheel={fn}>              // mouse wheel moved
```

---

### 🟢 Level 3 — Good to Know (Used Sometimes)

#### Drag and Drop Events
```jsx
<div draggable onDragStart={fn}>  // drag started
<div onDrag={fn}>                 // actively dragging
<div onDragEnd={fn}>              // drag finished
<div onDragEnter={fn}>            // dragged item entered element
<div onDragLeave={fn}>            // dragged item left element
<div onDragOver={fn}>             // dragged item over element
<div onDrop={fn}>                 // item dropped on element
```

#### Touch Events (Mobile)
```jsx
<div onTouchStart={fn}>         // finger touches screen
<div onTouchEnd={fn}>           // finger lifts off screen
<div onTouchMove={fn}>          // finger moving on screen
<div onTouchCancel={fn}>        // touch interrupted
```

#### Media Events
```jsx
<video onPlay={fn}>             // video started playing
<video onPause={fn}>            // video paused
<video onEnded={fn}>            // video finished
<video onTimeUpdate={fn}>       // current time changed
<video onVolumeChange={fn}>     // volume changed
<video onLoadedData={fn}>       // video data loaded
<video onError={fn}>            // video error
<audio onPlay={fn}>             // same events work for audio
```

---

### ⚪ Level 4 — Awareness Only (Rare Use)

#### Animation and Transition Events
```jsx
<div onAnimationStart={fn}>     // CSS animation started
<div onAnimationEnd={fn}>       // CSS animation ended
<div onAnimationIteration={fn}> // CSS animation repeated
<div onTransitionEnd={fn}>      // CSS transition finished
```

#### Pointer Events (combines mouse + touch)
```jsx
<div onPointerDown={fn}>        // pointer pressed (mouse/touch/pen)
<div onPointerUp={fn}>          // pointer released
<div onPointerMove={fn}>        // pointer moved
<div onPointerEnter={fn}>       // pointer entered element
<div onPointerLeave={fn}>       // pointer left element
<div onPointerCancel={fn}>      // pointer interrupted
```

#### Window / Document Events (via useEffect)
```javascript
useEffect(() => {
  // Resize
  window.addEventListener('resize', fn)

  // Online / Offline
  window.addEventListener('online', fn)
  window.addEventListener('offline', fn)

  // Tab closing
  window.addEventListener('beforeunload', fn)

  // Visibility change (tab switch)
  document.addEventListener('visibilitychange', fn)

  // Hash change (URL #)
  window.addEventListener('hashchange', fn)

  // History navigation
  window.addEventListener('popstate', fn)

  return () => {
    // cleanup all of them
    window.removeEventListener('resize', fn)
    // ... remove rest
  }
}, [])
```

---

## Section 5 — Complete Event Object Reference

### Understanding What Lives Where
```
e.target is ALWAYS available on EVERY event — it is a BASE property
But what e.target CONTAINS depends on the element involved

TWO types of properties:

1. Properties ON THE EVENT (e.clientX, e.key, e.deltaY)
   → about WHAT HAPPENED (where mouse is, which key pressed)
   → NOT about the element

2. Properties ON THE ELEMENT via e.target (e.target.value, e.target.checked)
   → about the ELEMENT'S DATA
   → only useful when element has that property

Example:
   Mouse click on a div
   → e.clientX works ✅   (position of mouse — on the event)
   → e.target.value = undefined ❌  (div has no value property)

   User types in input
   → e.target.value works ✅   (input has value — on the element)
   → e.clientX exists but irrelevant for typing
```

### Base Event — Properties ALL Events Share
```javascript
const handler = (e) => {

  // ── TARGET & ELEMENT — ALWAYS available on every event ────
  e.target              // element that TRIGGERED the event (what was clicked/typed in)
  e.currentTarget       // element the HANDLER is attached to
  e.relatedTarget       // secondary element (mouseover/mouseout/focus — null otherwise)

  // ── COMMON e.target PROPERTIES — available on any element ─
  e.target.id           // id attribute of the element
  e.target.className    // class attribute of the element
  e.target.tagName      // "INPUT", "BUTTON", "DIV"
  e.target.dataset      // data-* attributes as object
  e.target.dataset.id   // data-id attribute value

  // ── EVENT INFO ────────────────────────────────────────────
  e.type                // "click", "change", "keydown", "submit"
  e.timeStamp           // when event occurred (milliseconds)
  e.isTrusted           // true = real user, false = triggered by code

  // ── PROPAGATION CONTROL ───────────────────────────────────
  e.preventDefault()    // stop browser default behavior
  e.stopPropagation()   // stop event bubbling up
  e.stopImmediatePropagation() // stop bubbling + other listeners on same element

  // ── REACT SPECIFIC ────────────────────────────────────────
  e.nativeEvent         // actual browser event underneath React's SyntheticEvent
                        // use when you need browser-specific behavior
                        // 99% of time you don't need this — use e directly
  e.persist()           // React 16 only — prevent event from being pooled/nullified
  e.cancelable          // can preventDefault be called? true/false
  e.eventPhase          // 1=capturing, 2=target, 3=bubbling
}
```

### Mouse Event Properties
```javascript
const handleMouse = (e) => {

  // ── NOTE ──────────────────────────────────────────────────
  // e.target, e.currentTarget, e.type etc are inherited from base
  // Properties below are MOUSE SPECIFIC — on the event itself
  // e.target here is the element clicked — usually a div/button
  // e.target.value will be undefined on a div (divs have no value)

  // ── POSITION — relative to viewport ───────────────────────
  e.clientX             // X position from left of VIEWPORT
  e.clientY             // Y position from top of VIEWPORT

  // ── POSITION — relative to page ───────────────────────────
  e.pageX               // X position from left of PAGE (includes scroll)
  e.pageY               // Y position from top of PAGE (includes scroll)

  // ── POSITION — relative to element ────────────────────────
  e.offsetX             // X position from left of TARGET element
  e.offsetY             // Y position from top of TARGET element

  // ── POSITION — relative to screen ─────────────────────────
  e.screenX             // X position from left of MONITOR screen
  e.screenY             // Y position from top of MONITOR screen

  // ── MOUSE BUTTON ──────────────────────────────────────────
  e.button              // 0 = left, 1 = middle, 2 = right
  e.buttons             // bitmask — which buttons held (1=left, 2=right, 4=middle)

  // ── MODIFIER KEYS ─────────────────────────────────────────
  e.ctrlKey             // Ctrl held? true/false
  e.shiftKey            // Shift held? true/false
  e.altKey              // Alt held? true/false
  e.metaKey             // Cmd (Mac) / Win (Windows) held? true/false

  // ── MOVEMENT (mousemove only) ─────────────────────────────
  e.movementX           // X movement since last mousemove event
  e.movementY           // Y movement since last mousemove event
}
```

### Keyboard Event Properties
```javascript
const handleKeyboard = (e) => {

  // ── NOTE ──────────────────────────────────────────────────
  // e.target, e.currentTarget etc inherited from base
  // Properties below are KEYBOARD SPECIFIC — on the event itself
  // e.target here = the input/textarea that has focus
  // e.target.value = current text in that input (still works here!)

  // ── KEY IDENTITY ──────────────────────────────────────────
  e.key                 // "a", "A", "Enter", "Escape", "ArrowUp", " " (space)
  e.code                // "KeyA", "Enter", "Space", "ArrowUp" (physical key — layout independent)
  e.keyCode             // numeric code — 65(a), 13(Enter), 27(Escape) — DEPRECATED use e.key
  e.charCode            // character code — DEPRECATED
  e.which               // same as keyCode — DEPRECATED

  // ── MODIFIER KEYS ─────────────────────────────────────────
  e.ctrlKey             // Ctrl held? true/false
  e.shiftKey            // Shift held? true/false
  e.altKey              // Alt held? true/false
  e.metaKey             // Cmd (Mac) / Win key held? true/false

  // ── KEY STATE ─────────────────────────────────────────────
  e.repeat              // key being held down and repeating? true/false
  e.isComposing         // inside IME composition? true/false (Asian keyboards)

  // ── COMMON KEY VALUES ─────────────────────────────────────
  // e.key === 'Enter'
  // e.key === 'Escape'
  // e.key === 'Backspace'
  // e.key === 'Delete'
  // e.key === 'Tab'
  // e.key === 'ArrowUp'
  // e.key === 'ArrowDown'
  // e.key === 'ArrowLeft'
  // e.key === 'ArrowRight'
  // e.key === ' '         (space)
}
```

### Form / Input Event Properties
```javascript
const handleForm = (e) => {

  // ── NOTE ──────────────────────────────────────────────────
  // e.target, e.currentTarget etc inherited from base
  // Properties below are on e.target (the INPUT ELEMENT itself)
  // These work because INPUT elements have these properties
  // A div does NOT have .value or .checked — only form elements do

  // ── INPUT VALUE ───────────────────────────────────────────
  e.target.value        // current value of input/textarea/select
  e.target.name         // name attribute of the input
  e.target.id           // id attribute of the input
  e.target.type         // "text", "checkbox", "radio", "email", "file"
  e.target.placeholder  // placeholder text
  e.target.disabled     // is disabled? true/false
  e.target.readOnly     // is readonly? true/false
  e.target.required     // is required? true/false

  // ── CHECKBOX / RADIO ──────────────────────────────────────
  e.target.checked      // checkbox/radio — true/false
  e.target.indeterminate// checkbox indeterminate state (partial select)

  // ── SELECT ────────────────────────────────────────────────
  e.target.selectedIndex       // index of selected option
  e.target.selectedOptions     // HTMLCollection of selected options
  e.target.options             // all available options
  e.target.multiple            // multiple select? true/false

  // ── FILES (file input) ────────────────────────────────────
  e.target.files        // FileList of selected files
  e.target.files[0]     // first file
  e.target.files[0].name    // file name "photo.png"
  e.target.files[0].size    // file size in bytes
  e.target.files[0].type    // "image/png", "application/pdf"
  e.target.files[0].lastModified // timestamp

  // ── FORM VALIDATION ───────────────────────────────────────
  e.target.validity           // ValidityState object
  e.target.validationMessage  // browser validation message
  e.target.checkValidity()    // returns true/false
}
```

### Focus Event Properties
```javascript
const handleFocus = (e) => {

  // ── NOTE ──────────────────────────────────────────────────
  // Focus events have NO extra properties on the event itself
  // They only use BASE properties + relatedTarget
  // e.target = the element that gained/lost focus (an input usually)
  // e.target.value works here too since it's an input element

  // ── RELATED TARGET ────────────────────────────────────────
  e.relatedTarget       // on focus  → element that LOST focus (where you came FROM)
                        // on blur   → element that GAINED focus (where you are going TO)
                        // null if nothing related (clicked outside browser)

  // ── PRACTICAL USE ─────────────────────────────────────────
  // onFocus — user clicked into an input
  // onBlur  — user clicked out of an input (validate here!)
  const handleBlur = (e) => {
    if (!e.target.value) {
      setError('This field is required')
    }
  }
}
```

### Clipboard Event Properties
```javascript
const handleClipboard = (e) => {

  // ── CLIPBOARD DATA ────────────────────────────────────────
  e.clipboardData                      // ClipboardData object
  e.clipboardData.getData('text')      // get pasted text
  e.clipboardData.getData('text/html') // get pasted HTML
  e.clipboardData.setData('text', val) // set copy data (oncopy/oncut)
  e.clipboardData.files                // pasted files (images etc)
  e.clipboardData.items                // DataTransferItemList

  // ── COMMON PATTERN ────────────────────────────────────────
  const handlePaste = (e) => {
    e.preventDefault()
    const text = e.clipboardData.getData('text')
    // sanitize or transform text before using
  }
}
```

### Drag Event Properties
```javascript
const handleDrag = (e) => {

  // ── DATA TRANSFER ─────────────────────────────────────────
  e.dataTransfer                       // DataTransfer object
  e.dataTransfer.getData('text')       // get dragged data
  e.dataTransfer.setData('text', val)  // set drag data (dragstart)
  e.dataTransfer.files                 // dragged files
  e.dataTransfer.items                 // DataTransferItemList
  e.dataTransfer.types                 // ["text/plain", "Files"]

  // ── DRAG EFFECT ───────────────────────────────────────────
  e.dataTransfer.effectAllowed  // "copy", "move", "link", "none", "all"
  e.dataTransfer.dropEffect     // "copy", "move", "link", "none"

  // ── POSITION (same as mouse) ──────────────────────────────
  e.clientX
  e.clientY
}
```

### Touch Event Properties
```javascript
const handleTouch = (e) => {

  // ── TOUCH LISTS ───────────────────────────────────────────
  e.touches             // ALL fingers currently on screen
  e.targetTouches       // fingers touching THIS element
  e.changedTouches      // fingers that changed in this event

  // ── INDIVIDUAL TOUCH ──────────────────────────────────────
  e.touches[0]          // first finger
  e.touches[0].clientX  // X position of first finger
  e.touches[0].clientY  // Y position of first finger
  e.touches[0].pageX    // X from page start
  e.touches[0].pageY    // Y from page start
  e.touches[0].identifier // unique ID for this touch point
  e.touches.length      // how many fingers on screen

  // ── MODIFIER KEYS ─────────────────────────────────────────
  e.ctrlKey
  e.shiftKey
  e.altKey
  e.metaKey
}
```

### Scroll and Wheel Event Properties
```javascript
const handleScroll = (e) => {

  // ── SCROLL POSITION ───────────────────────────────────────
  e.target.scrollTop    // pixels scrolled from top
  e.target.scrollLeft   // pixels scrolled from left
  e.target.scrollHeight // total scrollable height
  e.target.scrollWidth  // total scrollable width
  e.target.clientHeight // visible height of element
  e.target.clientWidth  // visible width of element

  // ── DETECT BOTTOM (infinite scroll) ───────────────────────
  const isBottom =
    e.target.scrollHeight - e.target.scrollTop === e.target.clientHeight
}

const handleWheel = (e) => {

  // ── WHEEL DELTA ───────────────────────────────────────────
  e.deltaX              // horizontal scroll amount
  e.deltaY              // vertical scroll amount (negative = up)
  e.deltaZ              // Z axis (rare)
  e.deltaMode           // 0=pixels, 1=lines, 2=pages
}
```

### Animation and Transition Event Properties
```javascript
const handleAnimation = (e) => {

  // ── ANIMATION ─────────────────────────────────────────────
  e.animationName       // name of CSS animation
  e.elapsedTime         // seconds since animation started
  e.pseudoElement       // "::before", "::after" or ""
}

const handleTransition = (e) => {

  // ── TRANSITION ────────────────────────────────────────────
  e.propertyName        // CSS property that transitioned ("opacity", "transform")
  e.elapsedTime         // seconds the transition took
  e.pseudoElement       // "::before", "::after" or ""
}
```

### Media Event Properties
```javascript
const handleMedia = (e) => {

  // ── MEDIA ELEMENT ─────────────────────────────────────────
  e.target.currentTime    // current playback position (seconds)
  e.target.duration       // total duration (seconds)
  e.target.paused         // is paused? true/false
  e.target.ended          // has ended? true/false
  e.target.muted          // is muted? true/false
  e.target.volume         // volume 0.0 to 1.0
  e.target.playbackRate   // speed (1 = normal, 2 = 2x)
  e.target.readyState     // 0-4 (how much data is loaded)
  e.target.networkState   // 0-3 (network status)
  e.target.buffered       // TimeRanges of buffered data
  e.target.src            // media source URL
}
```

### Property Inheritance Summary
```
Every event type INHERITS base properties AND adds its own:

BASE (every event)
├── e.target, e.currentTarget, e.type
├── e.preventDefault(), e.stopPropagation()
├── e.bubbles, e.cancelable, e.timeStamp
├── e.nativeEvent         ← React only — actual browser event underneath
└── e.persist()           ← React 16 only — prevent pooling

MOUSE EVENT adds (on the event itself):
├── e.clientX, e.clientY, e.pageX, e.pageY
├── e.button, e.buttons
└── e.ctrlKey, e.shiftKey, e.altKey, e.metaKey

KEYBOARD EVENT adds (on the event itself):
├── e.key, e.code
├── e.ctrlKey, e.shiftKey, e.altKey, e.metaKey
└── e.repeat, e.isComposing

FORM / INPUT EVENT — extra on e.target (the input element):
├── e.target.value, e.target.name, e.target.type
├── e.target.checked (checkbox/radio)
└── e.target.files (file input)

FOCUS EVENT adds:
└── e.relatedTarget (where focus came from / going to)

CLIPBOARD EVENT adds:
└── e.clipboardData (pasted/copied data)

DRAG EVENT adds:
└── e.dataTransfer (dragged data)

TOUCH EVENT adds:
└── e.touches, e.targetTouches, e.changedTouches

WHEEL EVENT adds:
└── e.deltaX, e.deltaY, e.deltaZ, e.deltaMode
```

### What is SyntheticEvent
```
Browser fires native event
      ↓
React intercepts it at ROOT
      ↓
React wraps it in SyntheticEvent object
      ↓
SyntheticEvent has SAME interface as native event
      ↓
Works IDENTICALLY across all browsers ✅
```

### React Event Delegation
```
YOU WRITE
──────────────────────────────────────────
<button onClick={handleClick1}>Button 1</button>
<button onClick={handleClick2}>Button 2</button>
<input  onChange={handleChange} />

REACT ACTUALLY DOES
──────────────────────────────────────────
<div id="root">          ← ONE listener here (React 17+)
  <button>Button 1</button>   // NO listener
  <button>Button 2</button>   // NO listener
  <input />                   // NO listener
</div>

React stores internally:
{
  button1 → handleClick1
  button2 → handleClick2
  input   → handleChange
}

When event bubbles to root:
React checks e.target → finds correct handler → calls it
```

### React 16 vs React 17 Event Pooling
```
REACT 16 — Event Pooling
──────────────────────────────────────────
SyntheticEvent objects are REUSED (pooled)
Handler runs → event properties NULLIFIED
e.target becomes null after handler finishes

REACT 17+ — No More Pooling
──────────────────────────────────────────
Event pooling REMOVED
Events persist normally
e.target works anywhere — even in setTimeout ✅
e.persist() no longer needed
```

### Async Event Access
```javascript
// REACT 16 — PROBLEM
const handleClick = (e) => {
  setTimeout(() => {
    console.log(e.target) // ❌ null — event was pooled
  }, 1000)
}

// REACT 16 — FIX 1 — save value before async
const handleClick = (e) => {
  const target = e.target // save immediately
  setTimeout(() => {
    console.log(target)   // ✅ works
  }, 1000)
}

// REACT 16 — FIX 2 — e.persist()
const handleClick = (e) => {
  e.persist()             // tell React — don't pool this
  setTimeout(() => {
    console.log(e.target) // ✅ works
  }, 1000)
}

// REACT 17+ — just works
const handleClick = (e) => {
  setTimeout(() => {
    console.log(e.target) // ✅ works fine
  }, 1000)
}
```

### React Event Naming
```
JavaScript (Vanilla)    React JSX
──────────────────────  ──────────────────────
onclick              →  onClick
onchange             →  onChange
onsubmit             →  onSubmit
onkeydown            →  onKeyDown
onmouseenter         →  onMouseEnter
ondblclick           →  onDoubleClick
onfocus              →  onFocus
onblur               →  onBlur
```

### nativeEvent
```javascript
const handleClick = (e) => {
  e             // SyntheticEvent — React wrapper ✅ use this 99% of time
  e.nativeEvent // actual browser MouseEvent — use only when needed
}
```

---

## Section 7 — Event Patterns in React

### Basic Event Handler
```jsx
function Button() {
  const handleClick = (e) => {
    e.preventDefault()
    console.log('clicked')
  }

  return <button onClick={handleClick}>Click</button>
}
```

### Passing Arguments to Handlers
```jsx
function List() {
  const handleClick = (id, name, e) => {
    console.log(id, name)
    console.log(e.target)
  }

  return (
    <ul>
      <li onClick={(e) => handleClick(1, 'Alice', e)}>Alice</li>
      <li onClick={(e) => handleClick(2, 'Bob', e)}>Bob</li>
    </ul>
  )
}
```

### useCallback with Events (Performance)
```jsx
function Button({ onClick }) {
  // without useCallback — new function created every render
  const handleClick = (e) => {
    e.preventDefault()
    onClick()
  }

  // with useCallback — same function reference between renders
  const handleClick = useCallback((e) => {
    e.preventDefault()
    onClick()
  }, [onClick]) // only recreates if onClick changes

  return <button onClick={handleClick}>Click</button>
}
```

### Event Handler in List (Efficient Pattern)
```jsx
// INEFFICIENT — new function for each item every render
function List({ items }) {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id} onClick={() => handleClick(item.id)}>
          {item.name}
        </li>
      ))}
    </ul>
  )
}

// EFFICIENT — single handler using data attributes
function List({ items }) {
  const handleClick = (e) => {
    const id = e.currentTarget.dataset.id
    console.log('clicked item:', id)
  }

  return (
    <ul onClick={handleClick}> {/* delegation inside React too */}
      {items.map(item => (
        <li key={item.id} data-id={item.id}>
          {item.name}
        </li>
      ))}
    </ul>
  )
}
```

### Custom Event Hook
```jsx
// reusable hook for any event
function useEventListener(eventType, handler, element = window) {
  const savedHandler = useRef(handler)

  useEffect(() => {
    savedHandler.current = handler
  }, [handler])

  useEffect(() => {
    const eventListener = (e) => savedHandler.current(e)
    element.addEventListener(eventType, eventListener)

    return () => {
      element.removeEventListener(eventType, eventListener)
    }
  }, [eventType, element])
}

// Usage
function App() {
  useEventListener('resize', () => console.log('resized'))
  useEventListener('keydown', (e) => {
    if (e.key === 'Escape') closeModal()
  })
}
```

### Form Handling Pattern
```jsx
function Form() {
  const [values, setValues] = useState({ name: '', email: '' })

  const handleChange = (e) => {
    const { name, value } = e.target
    setValues(prev => ({ ...prev, [name]: value }))
  }

  const handleSubmit = (e) => {
    e.preventDefault()       // ALWAYS — stops page refresh
    console.log(values)
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="name"  value={values.name}  onChange={handleChange} />
      <input name="email" value={values.email} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  )
}
```

### Keyboard Shortcut Pattern
```jsx
function App() {
  useEffect(() => {
    const handleKeyDown = (e) => {
      // Ctrl + S — save
      if (e.ctrlKey && e.key === 's') {
        e.preventDefault()
        save()
      }

      // Ctrl + Z — undo
      if (e.ctrlKey && e.key === 'z') {
        undo()
      }

      // Escape — close modal
      if (e.key === 'Escape') {
        closeModal()
      }
    }

    window.addEventListener('keydown', handleKeyDown)
    return () => window.removeEventListener('keydown', handleKeyDown)
  }, [])
}
```

### Scroll Position Pattern
```jsx
function App() {
  const [scrollY, setScrollY] = useState(0)
  const [isBottom, setIsBottom] = useState(false)

  useEffect(() => {
    const handleScroll = () => {
      setScrollY(window.scrollY)

      // check if bottom
      const bottom =
        window.innerHeight + window.scrollY >= document.body.offsetHeight
      setIsBottom(bottom)
    }

    window.addEventListener('scroll', handleScroll, { passive: true })
    return () => window.removeEventListener('scroll', handleScroll)
  }, [])
}
```

### Drag and Drop Pattern
```jsx
function DragDrop() {
  const handleDragStart = (e, id) => {
    e.dataTransfer.setData('text/plain', id)
    e.dataTransfer.effectAllowed = 'move'
  }

  const handleDragOver = (e) => {
    e.preventDefault()                    // MUST — allows drop
    e.dataTransfer.dropEffect = 'move'
  }

  const handleDrop = (e, targetId) => {
    e.preventDefault()
    const draggedId = e.dataTransfer.getData('text/plain')
    // swap draggedId with targetId
  }

  return (
    <div>
      <div
        draggable
        onDragStart={(e) => handleDragStart(e, 'item-1')}
      >
        Drag me
      </div>

      <div
        onDragOver={handleDragOver}
        onDrop={(e) => handleDrop(e, 'zone-1')}
      >
        Drop here
      </div>
    </div>
  )
}
```

---

## Section 8 — TypeScript Event Types

### Complete Type Reference
```tsx
// ── MOUSE EVENTS ──────────────────────────────────────────
const handleClick      = (e: React.MouseEvent<HTMLButtonElement>) => {}
const handleMouseEnter = (e: React.MouseEvent<HTMLDivElement>) => {}
const handleContextMenu= (e: React.MouseEvent<HTMLElement>) => {}

// ── KEYBOARD EVENTS ───────────────────────────────────────
const handleKeyDown    = (e: React.KeyboardEvent<HTMLInputElement>) => {}
const handleKeyUp      = (e: React.KeyboardEvent<HTMLInputElement>) => {}

// ── FORM / CHANGE EVENTS ──────────────────────────────────
const handleChange     = (e: React.ChangeEvent<HTMLInputElement>) => {}
const handleTextarea   = (e: React.ChangeEvent<HTMLTextAreaElement>) => {}
const handleSelect     = (e: React.ChangeEvent<HTMLSelectElement>) => {}

// ── SUBMIT EVENT ──────────────────────────────────────────
const handleSubmit     = (e: React.FormEvent<HTMLFormElement>) => {}

// ── FOCUS EVENTS ──────────────────────────────────────────
const handleFocus      = (e: React.FocusEvent<HTMLInputElement>) => {}
const handleBlur       = (e: React.FocusEvent<HTMLInputElement>) => {}

// ── CLIPBOARD EVENTS ──────────────────────────────────────
const handlePaste      = (e: React.ClipboardEvent<HTMLInputElement>) => {}
const handleCopy       = (e: React.ClipboardEvent<HTMLInputElement>) => {}

// ── DRAG EVENTS ───────────────────────────────────────────
const handleDragStart  = (e: React.DragEvent<HTMLDivElement>) => {}
const handleDrop       = (e: React.DragEvent<HTMLDivElement>) => {}

// ── TOUCH EVENTS ──────────────────────────────────────────
const handleTouchStart = (e: React.TouchEvent<HTMLDivElement>) => {}
const handleTouchMove  = (e: React.TouchEvent<HTMLDivElement>) => {}

// ── SCROLL / WHEEL ────────────────────────────────────────
const handleScroll     = (e: React.UIEvent<HTMLDivElement>) => {}
const handleWheel      = (e: React.WheelEvent<HTMLDivElement>) => {}

// ── ANIMATION / TRANSITION ────────────────────────────────
const handleAnimation  = (e: React.AnimationEvent<HTMLDivElement>) => {}
const handleTransition = (e: React.TransitionEvent<HTMLDivElement>) => {}

// ── POINTER EVENTS ────────────────────────────────────────
const handlePointer    = (e: React.PointerEvent<HTMLDivElement>) => {}
```

### HTML Element Types Reference
```tsx
// Common element types for event generics
HTMLButtonElement       // <button>
HTMLInputElement        // <input>
HTMLTextAreaElement     // <textarea>
HTMLSelectElement       // <select>
HTMLFormElement         // <form>
HTMLDivElement          // <div>
HTMLSpanElement         // <span>
HTMLAnchorElement       // <a>
HTMLImageElement        // <img>
HTMLVideoElement        // <video>
HTMLAudioElement        // <audio>
HTMLElement             // any element (generic fallback)
```

---

## Section 9 — Common Mistakes and Best Practices

### Mistake 1 — Not Cleaning Up Event Listeners
```javascript
// ❌ WRONG — memory leak
useEffect(() => {
  window.addEventListener('resize', handleResize)
  // no cleanup — listener stays forever after unmount
}, [])

// ✅ CORRECT — cleanup on unmount
useEffect(() => {
  window.addEventListener('resize', handleResize)
  return () => window.removeEventListener('resize', handleResize)
}, [])
```

### Mistake 2 — Inline Arrow Functions in JSX
```jsx
// ❌ BAD — new function created every render
// causes unnecessary re-renders in child components
<button onClick={() => handleClick(id)}>Click</button>

// ✅ BETTER — define handler outside JSX
const handleClick = useCallback(() => {
  doSomething(id)
}, [id])

<button onClick={handleClick}>Click</button>
```

### Mistake 3 — Calling Function Instead of Passing Reference
```jsx
// ❌ WRONG — calls immediately on render, not on click
<button onClick={handleClick()}>Click</button>

// ✅ CORRECT — passes reference, calls on click
<button onClick={handleClick}>Click</button>

// ✅ CORRECT — when you need to pass arguments
<button onClick={() => handleClick(id)}>Click</button>
```

### Mistake 4 — Missing preventDefault on Form Submit
```jsx
// ❌ WRONG — page refreshes on submit
const handleSubmit = (e) => {
  console.log(values)
}

// ✅ CORRECT
const handleSubmit = (e) => {
  e.preventDefault()  // ALWAYS first line
  console.log(values)
}
```

### Mistake 5 — Not Using passive for Scroll/Touch
```javascript
// ❌ BAD — browser waits to see if preventDefault is called
window.addEventListener('scroll', handleScroll)
window.addEventListener('touchmove', handleTouch)

// ✅ GOOD — tells browser "we won't call preventDefault"
// browser can optimize scrolling performance
window.addEventListener('scroll', handleScroll, { passive: true })
window.addEventListener('touchmove', handleTouch, { passive: true })
```

### Mistake 6 — Event in useEffect Dependencies
```javascript
// ❌ WRONG — handleScroll recreated every render
//            causes effect to re-run every render
useEffect(() => {
  window.addEventListener('scroll', handleScroll)
  return () => window.removeEventListener('scroll', handleScroll)
}, [handleScroll]) // handleScroll changes every render

// ✅ CORRECT — stable function reference with useCallback
const handleScroll = useCallback(() => {
  setScrollY(window.scrollY)
}, [])

useEffect(() => {
  window.addEventListener('scroll', handleScroll)
  return () => window.removeEventListener('scroll', handleScroll)
}, [handleScroll]) // now stable
```

### Best Practices Summary
```
✅ Always cleanup addEventListener in useEffect return
✅ Always e.preventDefault() on form submit
✅ Use passive: true for scroll and touch listeners
✅ Use useCallback for event handlers passed as props
✅ Use data attributes for event delegation in lists
✅ Save e.target.value before async operations (React 16)
✅ Use e.key instead of e.keyCode (keyCode is deprecated)
✅ Prefer onKeyDown over onKeyPress (onKeyPress deprecated)

❌ Never forget to removeEventListener
❌ Never call function in JSX — onClick={fn()} not onClick={fn}
❌ Never skip e.preventDefault() on form submit
❌ Never attach heavy listeners without passive flag
❌ Never use inline arrow functions in render-heavy lists
```

---

## Section 10 — Interview Questions

### Q1: What is a SyntheticEvent in React?
```
Answer:
SyntheticEvent is React's cross-browser wrapper around
the native browser event. It has the same interface as
native events (target, preventDefault, stopPropagation etc)
but works identically across ALL browsers.

React pools and reuses these objects for performance in
React 16. React 17+ removed pooling — events persist normally.
```

### Q2: What is Event Delegation? How does React use it?
```
Answer:
Event Delegation = attaching ONE listener to a parent
instead of individual listeners on each child element.

React uses delegation by attaching ONE event listener
at the root div (React 17+) or document (React 16).
When you write onClick anywhere in JSX, React stores it
internally. When user clicks, event bubbles to root,
React checks e.target and calls the correct handler.

Benefit: 1000 buttons = still ONE listener = better performance
```

### Q3: What is the difference between e.target and e.currentTarget?
```
Answer:
e.target        = element that ACTUALLY triggered the event (what was clicked)
e.currentTarget = element that has the EVENT HANDLER attached

They are the SAME when handler is directly on clicked element.
They DIFFER when handler is on parent and child is clicked.
```

### Q4: What is Event Bubbling and how do you stop it?
```
Answer:
Event Bubbling = after an event fires on an element,
it travels UP through all parent elements.

Stop it with: e.stopPropagation()
Stop it + other listeners on same element: e.stopImmediatePropagation()
```

### Q5: What is the difference between stopPropagation and preventDefault?
```
Answer:
e.preventDefault()    = stops the BROWSER DEFAULT behavior
                        (page refresh on submit, navigation on link click)
                        event still bubbles normally

e.stopPropagation()   = stops the event from BUBBLING UP
                        browser default still happens
                        just stops parent handlers from firing
```

### Q6: Why was event pooling removed in React 17?
```
Answer:
React 16 reused SyntheticEvent objects to save memory
(called pooling). After handler finished, properties
were nullified (e.target = null).

This caused bugs when accessing event properties
asynchronously (inside setTimeout, promises etc).

React 17 removed pooling because modern JS engines
handle garbage collection efficiently enough that
the performance benefit was no longer worth the confusion.
```

### Q7: What is the difference between onMouseEnter and onMouseOver?
```
Answer:
onMouseEnter  = fires when mouse enters the element
                does NOT bubble — only fires on that element

onMouseOver   = fires when mouse enters element OR any child
                BUBBLES — fires on parent when entering child too

Use onMouseEnter for tooltips — avoids repeated firing
when moving between child elements.
```

### Q8: How do you handle events on dynamically added elements?
```
Answer:
Use Event Delegation — attach handler to parent that exists,
check e.target to identify which child was interacted with.

const handleClick = (e) => {
  if (e.target.matches('.list-item')) {
    console.log('item clicked:', e.target.dataset.id)
  }
}

<ul onClick={handleClick}>
  {items.map(item => (
    <li className="list-item" data-id={item.id}>{item.name}</li>
  ))}
</ul>
```

---

## Quick Reference Card

### Must Know Events
```
onClick          onDoubleClick    onContextMenu
onChange         onSubmit         onFocus        onBlur
onKeyDown        onKeyUp
```

### Should Know Events
```
onMouseEnter     onMouseLeave     onMouseMove
onPaste          onCopy           onCut
onScroll         onWheel
```

### Good to Know Events
```
onDragStart      onDragOver       onDrop
onTouchStart     onTouchEnd       onTouchMove
onPlay           onPause          onEnded
```

### Always Remember
```
e.preventDefault()    — stop browser default
e.stopPropagation()   — stop bubbling
e.target              — what was interacted with
e.target.value        — input current value
e.key                 — which key was pressed
```
