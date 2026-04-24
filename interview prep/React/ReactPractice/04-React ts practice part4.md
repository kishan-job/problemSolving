# React + TypeScript Practice Guide — Part 4
## Level 4: Specialist (1% — Production Incidents)

> These are the bugs that do not show up in development.
> They show up at 2am when your app is live and users are affected.
> Every senior developer has been burned by at least one of these.

---

# Table of Contents

- [Level 4 Overview](#level-4-overview)
- [Pattern 15: 🕳️ Memory Leaks](#pattern-15-️-memory-leaks)
  - [Scenario 1 — The Timer That Runs Forever](#-pattern-15--scenario-1--the-timer-that-runs-forever)
  - [Scenario 2 — The WebSocket That Never Closes](#-pattern-15--scenario-2--the-websocket-that-never-closes)
- [Pattern 16: 👻 Stale Closures](#pattern-16--stale-closures)
  - [Scenario 1 — The Counter That Always Shows Wrong](#-pattern-16--scenario-1--the-counter-that-always-shows-wrong)
  - [Scenario 2 — The Search That Fetches Old Results](#-pattern-16--scenario-2--the-search-that-fetches-old-results)
- [Pattern 17: 🔁 Infinite Render Loops](#pattern-17--infinite-render-loops)
  - [Scenario 1 — The useEffect That Runs Forever](#-pattern-17--scenario-1--the-useeffect-that-runs-forever)
  - [Scenario 2 — The Object Dependency That Never Stabilizes](#-pattern-17--scenario-2--the-object-dependency-that-never-stabilizes)
- [Pattern 18: 🔑 Token Expiry + Silent Refresh](#pattern-18--token-expiry--silent-refresh)
  - [Scenario 1 — The User Who Gets Logged Out Mid-Work](#-pattern-18--scenario-1--the-user-who-gets-logged-out-mid-work)
  - [Scenario 2 — The Five Requests That All Refresh at Once](#-pattern-18--scenario-2--the-five-requests-that-all-refresh-at-once)
- [Pattern 19: 📦 Context Performance at Scale](#pattern-19--context-performance-at-scale)
  - [Scenario 1 — The App That Re-renders Everything on Every Click](#-pattern-19--scenario-1--the-app-that-re-renders-everything-on-every-click)
  - [Scenario 2 — The Context That Makes the App Unusable](#-pattern-19--scenario-2--the-context-that-makes-the-app-unusable)
- [Interview Pattern Picker — Level 4](#interview-pattern-picker--level-4)
- [Production Readiness Checklist — Level 4](#production-readiness-checklist--level-4)

---

# Level 4 Overview

```
These 5 patterns cover the bugs that:
  - Never appear in your local dev environment
  - Only surface under real usage conditions
  - Can take hours or days to debug without knowing the pattern
  - Are the difference between junior and senior React developers

Pattern 15: Memory Leaks
  → Component unmounts, but something keeps running
  → Memory grows over time → app slows down → browser crashes

Pattern 16: Stale Closures
  → Function captures old state value
  → Logic runs with wrong data silently
  → No errors, just wrong behavior

Pattern 17: Infinite Render Loops
  → useEffect triggers state change → re-render → useEffect again
  → Browser freezes → white screen → calls to support

Pattern 18: Token Expiry + Silent Refresh
  → JWT expires mid-session
  → User loses work, gets logged out
  → Multiple requests fail simultaneously

Pattern 19: Context Performance at Scale
  → Small app works fine → grows to 50 components
  → Every state change re-renders the entire tree
  → App becomes unusably slow
```

---

# Pattern 15: 🕳️ Memory Leaks

> **Level 4 — Specialist**

🎯 **What you will fix in this pattern:**
- A timer or interval that keeps running after the component unmounts
- A WebSocket or subscription that is never closed
- An async fetch that tries to update state on an unmounted component

📋 **Interview asks this as:**
- "What is a memory leak in React and how do you prevent it?"
- "Why do you return a cleanup function from useEffect?"
- "How do you fix the warning: Can't perform a React state update on an unmounted component?"

🔷 **TypeScript concepts used in this pattern:**
- `ReturnType<typeof setInterval>` for typing timer IDs
- `AbortController` with typed signal for cancelling fetch
- Generic cleanup patterns with typed refs

🚨 **Most common mistake in interviews:**
Setting up `setInterval`, `addEventListener`, or WebSocket in `useEffect`
without returning a cleanup function. The resource keeps running after
the component is gone — consuming memory and firing errors silently.

---

### 🔴 Pattern 15 · Scenario 1 — The Timer That Runs Forever

🏢 **Situation:**
You are on call at a food delivery app. Users report the app gets
slower and slower the longer they use it. Eventually it crashes
the browser tab. Your colleague opens the Chrome memory profiler
and sees memory growing by 2MB every minute. You trace it to
the live order tracker component that users open and close repeatedly.
Every time they open it, a new interval starts. None ever stop.

🔷 **Types for this scenario:**

```typescript
interface Order {
  id: string;
  status: 'placed' | 'preparing' | 'on_the_way' | 'delivered';
  estimatedArrival: string;
  driverName: string;
  driverLocation: { lat: number; lng: number };
}

interface OrderTrackerProps {
  orderId: string;
  onDelivered: () => void;
}
```

**Here is the current code:**

```typescript
import { useState, useEffect } from 'react';

function OrderTracker({ orderId, onDelivered }: OrderTrackerProps) {
  const [order, setOrder] = useState<Order | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(true);

  useEffect(() => {
    // Fetch immediately on mount
    fetchOrder();

    // Then fetch every 5 seconds
    // BUG: this interval is NEVER cleared
    setInterval(() => {
      fetchOrder();
    }, 5000);

    async function fetchOrder(): Promise<void> {
      try {
        const res = await fetch(`/api/orders/${orderId}`);
        const data = await res.json() as Order;
        setOrder(data);
        setIsLoading(false);

        if (data.status === 'delivered') {
          onDelivered();
        }
      } catch (err) {
        console.error('Failed to fetch order:', err);
      }
    }
  }, [orderId]);
  // Missing: cleanup, setLoading on orderId change,
  // cancel fetch on unmount

  if (isLoading) return <p>Loading order...</p>;
  if (!order) return <p>Order not found.</p>;

  return (
    <div>
      <h2>Order #{order.id}</h2>
      <p>Status: {order.status}</p>
      <p>Driver: {order.driverName}</p>
      <p>ETA: {order.estimatedArrival}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Store the interval ID and clear it when the component unmounts
- ✅ Stop the interval when order status is "delivered"
- ✅ Cancel the fetch request if the component unmounts mid-request
- ✅ Reset loading state when `orderId` changes

<details>
<summary>💡 Hint</summary>

The interval ID returned by `setInterval` must be stored somewhere
and passed to `clearInterval` in the cleanup function.
For cancelling fetch: use `AbortController` — create it before the fetch,
pass `signal` to the fetch options, and call `controller.abort()` in cleanup.
Both cleanup actions go in the `return () => {}` of the same useEffect.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import { useState, useEffect, useCallback } from 'react';

function OrderTracker({ orderId, onDelivered }: OrderTrackerProps) {
  const [order, setOrder] = useState<Order | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    // AbortController — lets us cancel in-flight fetch on unmount
    const controller = new AbortController();

    // Type the interval ID correctly
    let intervalId: ReturnType<typeof setInterval> | null = null;

    // Track if component is still mounted
    // Prevents state updates after unmount
    let isMounted = true;

    const fetchOrder = async (): Promise<void> => {
      try {
        const res = await fetch(`/api/orders/${orderId}`, {
          signal: controller.signal, // attach abort signal
        });

        if (!res.ok) throw new Error(`HTTP ${res.status}`);

        const data = await res.json() as Order;

        // Only update state if still mounted
        if (!isMounted) return;

        setOrder(data);
        setIsLoading(false);
        setError(null);

        // Stop polling once delivered
        if (data.status === 'delivered') {
          if (intervalId !== null) {
            clearInterval(intervalId);
            intervalId = null;
          }
          onDelivered();
        }
      } catch (err) {
        // AbortError is expected on cleanup — not a real error
        if (err instanceof Error && err.name === 'AbortError') return;

        if (isMounted) {
          setError('Failed to load order status.');
          setIsLoading(false);
        }
      }
    };

    // Reset state when orderId changes
    setIsLoading(true);
    setOrder(null);
    setError(null);

    // Initial fetch
    fetchOrder();

    // Poll every 5 seconds
    intervalId = setInterval(fetchOrder, 5000);

    // CLEANUP — runs on unmount OR when orderId changes
    return () => {
      isMounted = false;

      // Stop the interval
      if (intervalId !== null) {
        clearInterval(intervalId);
      }

      // Cancel any in-flight fetch
      controller.abort();
    };

  }, [orderId, onDelivered]);

  if (isLoading) return <p>Tracking your order...</p>;
  if (error) return <p style={{ color: 'red' }}>{error}</p>;
  if (!order) return <p>Order not found.</p>;

  return (
    <div>
      <h2>Order #{order.id}</h2>
      <p>Status: {order.status}</p>
      <p>Driver: {order.driverName}</p>
      <p>ETA: {order.estimatedArrival}</p>
      {order.status === 'delivered' && (
        <p style={{ color: 'green' }}>Your order has been delivered!</p>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Three cleanup actions in one useEffect: `clearInterval`, `controller.abort()`,
and the `isMounted` flag. They also check that `AbortError` is caught
separately and not shown as a real error to the user.

🔷 **TypeScript lesson:**
`ReturnType<typeof setInterval>` is the correct way to type a timer ID
in TypeScript — it resolves to `number` in browsers and `NodeJS.Timeout`
in Node, so your code works in both environments without manual type casting.

⚡ **Why this matters in production:**
Without cleanup, opening and closing this component 10 times creates
10 simultaneous intervals all making API calls. Memory grows continuously
and the app eventually crashes. Users lose their session data.

🔍 **What a senior dev spots in 10 seconds:**
`setInterval(() => {})` inside useEffect with no corresponding
`clearInterval` in a return function. The interval ID is not stored.

✅ **Before you ship this, verify:**
```
□ Does every setInterval have a matching clearInterval in cleanup?
□ Does every fetch have an AbortController signal attached?
□ Does the cleanup handle the case where interval is already null?
□ Is AbortError caught separately and silently ignored?
□ Does state reset when the key prop (orderId) changes?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

### 🔴 Pattern 15 · Scenario 2 — The WebSocket That Never Closes

🏢 **Situation:**
You are at a social platform. The notification bell opens a panel
showing live notifications via WebSocket. Users open and close the
panel dozens of times per session. Your backend team alerts you:
one user has 47 active WebSocket connections open simultaneously.
The server is being hammered. Each open-close cycle creates a new
connection that is never closed.

🔷 **Types for this scenario:**

```typescript
type NotificationType = 'like' | 'comment' | 'follow' | 'mention';

interface Notification {
  id: string;
  type: NotificationType;
  message: string;
  fromUser: string;
  createdAt: string;
  isRead: boolean;
}

interface NotificationPanelProps {
  userId: string;
  isOpen: boolean;
}

interface WebSocketMessage {
  type: 'NEW_NOTIFICATION' | 'MARK_READ' | 'PING';
  payload?: Notification;
}
```

**Here is the current code:**

```typescript
import { useState, useEffect } from 'react';

function NotificationPanel({ userId, isOpen }: NotificationPanelProps) {
  const [notifications, setNotifications] = useState<Notification[]>([]);

  useEffect(() => {
    if (!isOpen) return; // only connect when panel is open

    // New WebSocket every time panel opens
    // BUG: never closed when panel closes or component unmounts
    const ws = new WebSocket(`wss://api.example.com/ws/notifications/${userId}`);

    ws.onopen = () => {
      console.log('Connected');
    };

    ws.onmessage = (event: MessageEvent) => {
      const message = JSON.parse(event.data) as WebSocketMessage;
      if (message.type === 'NEW_NOTIFICATION' && message.payload) {
        setNotifications(prev => [message.payload!, ...prev]);
      }
    };

    ws.onerror = (error: Event) => {
      console.error('WebSocket error:', error);
    };

    // Missing: no cleanup — ws.close() never called
  }, [isOpen, userId]);

  if (!isOpen) return null;

  return (
    <div>
      <h3>Notifications</h3>
      {notifications.length === 0 ? (
        <p>No notifications</p>
      ) : (
        notifications.map(n => (
          <div key={n.id}>
            <strong>{n.fromUser}</strong>
            <span>{n.message}</span>
          </div>
        ))
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Close the WebSocket when the panel closes or component unmounts
- ✅ Handle reconnection if the connection drops unexpectedly
- ✅ Show a connection status indicator (connected / reconnecting)
- ✅ Load initial notifications via REST API, use WebSocket only for updates

<details>
<summary>💡 Hint</summary>

The WebSocket cleanup goes in the return function of useEffect.
`ws.close()` is the cleanup call — same as `clearInterval` for timers.
For reconnection: check `ws.onclose` — if it was not a clean close
(code 1000) and the panel is still open, reconnect after a delay.
Use a `useRef` to track whether the close was intentional.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import { useState, useEffect, useRef } from 'react';

function NotificationPanel({ userId, isOpen }: NotificationPanelProps) {
  const [notifications, setNotifications] = useState<Notification[]>([]);
  const [connectionStatus, setConnectionStatus] =
    useState<'connected' | 'disconnected' | 'reconnecting'>('disconnected');
  const [isLoadingInitial, setIsLoadingInitial] = useState<boolean>(true);

  // Track if close was intentional (cleanup) vs unexpected (need reconnect)
  const isIntentionalClose = useRef<boolean>(false);

  // Load initial notifications via REST (not WebSocket)
  useEffect(() => {
    if (!isOpen) return;

    const loadInitial = async (): Promise<void> => {
      try {
        const res = await fetch(`/api/notifications/${userId}`);
        const data = await res.json() as Notification[];
        setNotifications(data);
      } catch (err) {
        console.error('Failed to load notifications:', err);
      } finally {
        setIsLoadingInitial(false);
      }
    };

    loadInitial();
  }, [isOpen, userId]);

  // WebSocket for live updates only
  useEffect(() => {
    if (!isOpen) return;

    let reconnectTimer: ReturnType<typeof setTimeout> | null = null;

    const connect = (): WebSocket => {
      isIntentionalClose.current = false;
      setConnectionStatus('connected');

      const ws = new WebSocket(
        `wss://api.example.com/ws/notifications/${userId}`
      );

      ws.onopen = (): void => {
        setConnectionStatus('connected');
        if (reconnectTimer) {
          clearTimeout(reconnectTimer);
          reconnectTimer = null;
        }
      };

      ws.onmessage = (event: MessageEvent<string>): void => {
        try {
          const message = JSON.parse(event.data) as WebSocketMessage;
          if (message.type === 'NEW_NOTIFICATION' && message.payload) {
            setNotifications(prev => [message.payload!, ...prev]);
          }
        } catch {
          console.error('Failed to parse WebSocket message');
        }
      };

      ws.onclose = (event: CloseEvent): void => {
        // Was this our intentional cleanup close?
        if (isIntentionalClose.current) return;

        // Unexpected close — attempt reconnect
        setConnectionStatus('reconnecting');
        reconnectTimer = setTimeout(() => {
          if (isOpen) connect(); // reconnect if still open
        }, 3000);
      };

      ws.onerror = (): void => {
        setConnectionStatus('reconnecting');
      };

      return ws;
    };

    const ws = connect();

    // CLEANUP — runs when isOpen becomes false or component unmounts
    return () => {
      isIntentionalClose.current = true; // mark as intentional

      // Clear any pending reconnect timer
      if (reconnectTimer) clearTimeout(reconnectTimer);

      // Close the WebSocket cleanly (code 1000 = normal closure)
      if (ws.readyState === WebSocket.OPEN ||
          ws.readyState === WebSocket.CONNECTING) {
        ws.close(1000, 'Panel closed by user');
      }

      setConnectionStatus('disconnected');
    };

  }, [isOpen, userId]);

  if (!isOpen) return null;

  return (
    <div>
      <div style={{ display: 'flex', justifyContent: 'space-between' }}>
        <h3>Notifications</h3>
        <span style={{
          color: connectionStatus === 'connected' ? 'green' :
                 connectionStatus === 'reconnecting' ? 'orange' : 'gray',
          fontSize: 12,
        }}>
          {connectionStatus === 'connected' && '● Live'}
          {connectionStatus === 'reconnecting' && '● Reconnecting...'}
          {connectionStatus === 'disconnected' && '● Offline'}
        </span>
      </div>

      {isLoadingInitial ? (
        <p>Loading...</p>
      ) : notifications.length === 0 ? (
        <p>No notifications yet.</p>
      ) : (
        notifications.map(n => (
          <div key={n.id} style={{ opacity: n.isRead ? 0.6 : 1 }}>
            <strong>{n.fromUser}</strong>
            <span> {n.message}</span>
            <span style={{ fontSize: 11, color: '#999' }}>
              {new Date(n.createdAt).toLocaleTimeString()}
            </span>
          </div>
        ))
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`ws.close(1000, 'reason')` in the cleanup return — this is the exact
pattern for closing WebSocket. They also check the `isIntentionalClose`
ref to distinguish cleanup from unexpected disconnect, and the
reconnect logic with a timer that is itself cleaned up.

🔷 **TypeScript lesson:**
`MessageEvent<string>` properly types the WebSocket message event,
and `CloseEvent` types the close event. Without these, TypeScript
cannot help you catch incorrect message parsing. Typed events
prevent an entire class of runtime errors.

⚡ **Why this matters in production:**
Each leaked WebSocket holds a TCP connection on the server. With
enough users opening and closing the panel, the server runs out
of connections. This is an outage, not just a slowdown.

🔍 **What a senior dev spots in 10 seconds:**
`new WebSocket(...)` inside useEffect with no `ws.close()` in
a return function. The cleanup is simply missing.

✅ **Before you ship this, verify:**
```
□ Does ws.close() run in every path that exits the useEffect?
□ Is the reconnect timer cleared in cleanup?
□ Does the reconnect check if the panel is still open?
□ Is ws.readyState checked before closing?
□ Is isIntentionalClose reset before each new connection?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 16: 👻 Stale Closures

> **Level 4 — Specialist**

🎯 **What you will fix in this pattern:**
- A timer that always uses the initial state value, never the updated one
- A search function that fetches results for old query values
- An event handler that references state from a previous render

📋 **Interview asks this as:**
- "What is a stale closure in React?"
- "Why does my useEffect always see the initial value of state?"
- "My setInterval callback always logs the same value — why?"

🔷 **TypeScript concepts used in this pattern:**
- `useCallback` with proper typed dependencies
- `useRef` typed as `React.MutableRefObject<T>` for mutable ref values
- Understanding that TypeScript does not catch stale closure bugs —
  they are a runtime logic error, not a type error

🚨 **Most common mistake in interviews:**
Not understanding WHY the stale value appears. The answer is:
the function was created during a specific render and "closed over"
the values from that render. Those values never update inside that
function because a new function is never created.

---

### 🔴 Pattern 16 · Scenario 1 — The Counter That Always Shows Wrong

🏢 **Situation:**
You are at a SaaS platform building a session activity tracker.
The feature records how many actions a user takes every 10 seconds
and sends it to the analytics API. Your analytics team reports all
sessions are showing "1 action" even for highly active users.
You add a console.log inside the interval and see it always prints 0.
The counter on screen shows the correct number. Something is wrong.

🔷 **Types for this scenario:**

```typescript
interface ActivityReport {
  sessionId: string;
  actionCount: number;
  timestamp: string;
}

interface ActivityTrackerProps {
  sessionId: string;
}
```

**Here is the current code:**

```typescript
import { useState, useEffect } from 'react';

function ActivityTracker({ sessionId }: ActivityTrackerProps) {
  const [actionCount, setActionCount] = useState<number>(0);

  // Records a user action
  const recordAction = (): void => {
    setActionCount(prev => prev + 1);
  };

  useEffect(() => {
    // Sends report every 10 seconds
    const intervalId = setInterval(async () => {
      // BUG: actionCount is ALWAYS 0 here
      // This function closed over actionCount from the FIRST render
      // It never sees updated values
      console.log('Sending report, count:', actionCount); // always 0

      if (actionCount === 0) return; // never sends!

      const report: ActivityReport = {
        sessionId,
        actionCount, // always 0 — stale closure
        timestamp: new Date().toISOString(),
      };

      await fetch('/api/analytics/activity', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(report),
      });

      setActionCount(0); // reset after report — but this never runs
    }, 10000);

    return () => clearInterval(intervalId);
  }, []); // empty deps — interval created once, closed over initial actionCount

  return (
    <div>
      <p>Actions this session: {actionCount}</p>
      <button onClick={recordAction}>Record Action</button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the stale closure so the interval reads the latest `actionCount`
- ✅ Use a `ref` to hold the current value for the interval to read
- ✅ Keep the display updating correctly via state
- ✅ Reset the count after each report is sent

<details>
<summary>💡 Hint</summary>

State in a stale closure never updates because the closure was
created during a specific render. The fix: use a `ref` alongside
the state. State drives the UI (re-renders on change). The ref
stores the same value and is always current — refs are not part
of the closure snapshot. The interval reads from the ref, not state.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import { useState, useEffect, useRef } from 'react';

function ActivityTracker({ sessionId }: ActivityTrackerProps) {
  const [actionCount, setActionCount] = useState<number>(0);

  // Ref mirrors state — always holds the latest value
  // Ref is mutable — not captured in closure snapshot
  const actionCountRef = useRef<number>(0);

  const recordAction = (): void => {
    setActionCount(prev => {
      const next = prev + 1;
      actionCountRef.current = next; // keep ref in sync with state
      return next;
    });
  };

  useEffect(() => {
    const intervalId = setInterval(async () => {
      // FIX: read from ref — always current, never stale
      const currentCount = actionCountRef.current;

      console.log('Sending report, count:', currentCount); // now correct!

      if (currentCount === 0) return; // correctly skips when no actions

      const report: ActivityReport = {
        sessionId,
        actionCount: currentCount, // correct value
        timestamp: new Date().toISOString(),
      };

      try {
        await fetch('/api/analytics/activity', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(report),
        });

        // Reset both state and ref after successful report
        setActionCount(0);
        actionCountRef.current = 0;

      } catch (err) {
        console.error('Failed to send activity report:', err);
        // Do not reset — keep the count for next attempt
      }
    }, 10000);

    return () => clearInterval(intervalId);
  }, [sessionId]); // sessionId is stable — interval created once per session

  return (
    <div>
      <p>Actions this session: {actionCount}</p>
      <button onClick={recordAction}>Record Action</button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `useRef` + `useState` dual pattern — state for UI updates,
ref for the interval to read from. They specifically want to hear
you explain why the closure is stale: the interval was created once
and closed over the initial value of `actionCount` (which was 0).

🔷 **TypeScript lesson:**
`useRef<number>(0)` properly types the ref. TypeScript will enforce
that `actionCountRef.current` is always a number and prevent
accidental assignment of incompatible types. But TypeScript cannot
warn you about stale closures — that is a runtime logic error.
This is one case where TypeScript's type safety does not protect you.

⚡ **Why this matters in production:**
Stale closures in intervals silently produce wrong data. No errors
are thrown. The UI looks correct. Only the data being sent to your
backend is wrong — and you may not notice until you look at analytics.

🔍 **What a senior dev spots in 10 seconds:**
`setInterval` or `setTimeout` callback using state variables,
combined with an empty dependency array `[]`. That combination
almost always means stale closure.

✅ **Before you ship this, verify:**
```
□ Does every interval/timeout callback that uses state also use a ref?
□ Is the ref updated every time state is updated?
□ Are both state and ref reset together after the operation?
□ Is the interval dependency array accurate — not just []?
□ Is the interval cleanup in the useEffect return?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

### 🔴 Pattern 16 · Scenario 2 — The Search That Fetches Old Results

🏢 **Situation:**
You are at a job portal. The candidate search has a debounced
input — it waits 500ms after the user stops typing before fetching.
Users report that fast typing sometimes shows wrong results.
They type "React developer" quickly, then the results show
"developer" results instead of "React developer" results.
Slower typists never see the bug.

🔷 **Types for this scenario:**

```typescript
interface Candidate {
  id: string;
  name: string;
  skills: string[];
  experience: number;
  location: string;
}

interface SearchState {
  results: Candidate[];
  isLoading: boolean;
  error: string | null;
  query: string;
}
```

**Here is the current code:**

```typescript
import { useState, useEffect, useCallback } from 'react';

function CandidateSearch() {
  const [inputValue, setInputValue] = useState<string>('');
  const [results, setResults] = useState<Candidate[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  // Search function — this is the stale closure
  const searchCandidates = useCallback(async (query: string): Promise<void> => {
    if (!query.trim()) {
      setResults([]);
      return;
    }

    setIsLoading(true);
    try {
      const res = await fetch(
        `/api/candidates/search?q=${encodeURIComponent(query)}`
      );
      const data = await res.json() as Candidate[];
      // BUG: by the time this runs, inputValue may have changed
      // But we still set results for this possibly-outdated query
      setResults(data);
    } catch (err) {
      console.error('Search failed:', err);
    } finally {
      setIsLoading(false);
    }
  }, []); // empty deps — stale reference to old query

  useEffect(() => {
    const timer = setTimeout(() => {
      searchCandidates(inputValue);
    }, 500);

    return () => clearTimeout(timer);
  }, [inputValue, searchCandidates]);

  return (
    <div>
      <input
        value={inputValue}
        onChange={e => setInputValue(e.target.value)}
        placeholder="Search candidates..."
      />
      {isLoading && <p>Searching...</p>}
      <div>
        {results.map(candidate => (
          <div key={candidate.id}>
            <strong>{candidate.name}</strong>
            <p>{candidate.skills.join(', ')}</p>
            <p>{candidate.experience} years · {candidate.location}</p>
          </div>
        ))}
      </div>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the race condition where old results overwrite new ones
- ✅ Cancel the previous fetch when a new search starts
- ✅ Show which query the results are for
- ✅ Handle the empty results case with a clear message

<details>
<summary>💡 Hint</summary>

This is a race condition, not a classic stale closure — but same root cause.
The issue: two fetches start, the slower one finishes last and overwrites
the faster one's results. Fix: use `AbortController` to cancel the previous
fetch when a new one starts. Also check if the query that triggered
the fetch still matches the current input before setting results.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import { useState, useEffect, useRef } from 'react';

function CandidateSearch() {
  const [inputValue, setInputValue] = useState<string>('');
  const [results, setResults] = useState<Candidate[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(false);
  const [error, setError] = useState<string | null>(null);
  const [searchedQuery, setSearchedQuery] = useState<string>('');

  // Ref to the latest AbortController
  // Lets us cancel the previous fetch when a new one starts
  const abortControllerRef = useRef<AbortController | null>(null);

  useEffect(() => {
    const query = inputValue.trim();

    // Clear results immediately when input is cleared
    if (!query) {
      setResults([]);
      setSearchedQuery('');
      setIsLoading(false);
      return;
    }

    const timer = setTimeout(async () => {
      // Cancel any in-flight request from previous keystroke
      if (abortControllerRef.current) {
        abortControllerRef.current.abort();
      }

      // Create fresh controller for this request
      const controller = new AbortController();
      abortControllerRef.current = controller;

      setIsLoading(true);
      setError(null);

      try {
        const res = await fetch(
          `/api/candidates/search?q=${encodeURIComponent(query)}`,
          { signal: controller.signal }
        );

        if (!res.ok) throw new Error(`Search failed: ${res.status}`);

        const data = await res.json() as Candidate[];

        // FIX: only update if this request was NOT cancelled
        // (i.e. we are still waiting for THIS query's results)
        setResults(data);
        setSearchedQuery(query); // record what we searched for

      } catch (err) {
        // AbortError = intentional cancel — not a real error
        if (err instanceof Error && err.name === 'AbortError') return;
        setError('Search failed. Please try again.');
        setResults([]);
      } finally {
        setIsLoading(false);
      }
    }, 500);

    return () => {
      clearTimeout(timer);
      // Also cancel fetch if input changes before timer fires
      if (abortControllerRef.current) {
        abortControllerRef.current.abort();
      }
    };
  }, [inputValue]);

  const showEmptyState =
    !isLoading &&
    !error &&
    searchedQuery !== '' &&
    results.length === 0;

  return (
    <div>
      <input
        value={inputValue}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setInputValue(e.target.value)
        }
        placeholder="Search candidates by skill, name, or location..."
      />

      {isLoading && <p>Searching for "{inputValue.trim()}"...</p>}

      {error && <p style={{ color: 'red' }}>{error}</p>}

      {showEmptyState && (
        <p>No candidates found for "{searchedQuery}".</p>
      )}

      {!isLoading && results.length > 0 && (
        <>
          <p>{results.length} candidates found for "{searchedQuery}"</p>
          <div>
            {results.map(candidate => (
              <div key={candidate.id}>
                <strong>{candidate.name}</strong>
                <p>{candidate.skills.join(', ')}</p>
                <p>
                  {candidate.experience} years · {candidate.location}
                </p>
              </div>
            ))}
          </div>
        </>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`AbortController` to cancel the previous fetch is the exact answer.
They also look for `AbortError` being caught and silently ignored,
and the `searchedQuery` state that shows what results belong to.

🔷 **TypeScript lesson:**
`useRef<AbortController | null>(null)` correctly types the ref
as possibly null (before first search) or an AbortController.
The null check `if (abortControllerRef.current)` before `.abort()`
is required by TypeScript — it forces you to handle the null case
that would otherwise cause a runtime error.

⚡ **Why this matters in production:**
Without cancellation, a slow network causes users to see results for
a query they already changed. This looks like random wrong results
and is extremely hard to reproduce in fast local environments.

🔍 **What a senior dev spots in 10 seconds:**
Multiple `fetch` calls that can overlap, with no cancellation mechanism.
The signal: debounced input triggering async fetch with no AbortController.

✅ **Before you ship this, verify:**
```
□ Is the previous AbortController cancelled before each new fetch?
□ Is AbortError caught and silently ignored?
□ Is the controller ref cleared or replaced, not leaked?
□ Does the cleanup cancel both the timer AND the in-flight fetch?
□ Does empty input clear results immediately?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 17: 🔁 Infinite Render Loops

> **Level 4 — Specialist**

🎯 **What you will fix in this pattern:**
- A `useEffect` that triggers a state update which triggers the same `useEffect`
- An object or array in the dependency array that is recreated every render
- A parent passing a new function reference on every render causing child loops

📋 **Interview asks this as:**
- "Why is my component re-rendering infinitely?"
- "What causes an infinite loop in useEffect?"
- "Why does adding an object to the useEffect dependency array cause infinite renders?"

🔷 **TypeScript concepts used in this pattern:**
- `useMemo` with typed return values to stabilize object references
- `useCallback` with typed parameters to stabilize function references
- Understanding that TypeScript cannot prevent infinite loops —
  it is a runtime behavior, not a type error

🚨 **Most common mistake in interviews:**
Adding an object literal or array literal directly to the dependency array.
In JavaScript, `{} === {}` is `false` — every render creates a new object
reference even if the contents are identical. React sees a new dependency
every render and re-runs the effect.

---

### 🔴 Pattern 17 · Scenario 1 — The useEffect That Runs Forever

🏢 **Situation:**
You are at a SaaS dashboard. A developer on your team added a
"sync user preferences" feature that calls an API whenever preferences
change. They push it to production on Friday afternoon. By Friday
evening, your API gateway is showing 50,000 requests per minute
from a single user. The browser tab is completely frozen.
Your on-call phone goes off. You open the code.

🔷 **Types for this scenario:**

```typescript
interface UserPreferences {
  theme: 'light' | 'dark';
  language: string;
  notificationsEnabled: boolean;
  itemsPerPage: number;
}

interface SyncResult {
  success: boolean;
  syncedAt: string;
}
```

**Here is the current code:**

```typescript
import { useState, useEffect } from 'react';

function PreferencesSync() {
  const [preferences, setPreferences] = useState<UserPreferences>({
    theme: 'light',
    language: 'en',
    notificationsEnabled: true,
    itemsPerPage: 10,
  });
  const [lastSync, setLastSync] = useState<string | null>(null);

  useEffect(() => {
    // BUG: this creates an infinite loop
    // 1. Effect runs
    // 2. fetch completes → setLastSync updates state
    // 3. Re-render happens
    // 4. `preferences` object is recreated with same values
    // 5. React sees new object reference → runs effect again
    // 6. GOTO 1

    const syncPreferences = async (): Promise<void> => {
      const res = await fetch('/api/user/preferences', {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(preferences),
      });
      const result = await res.json() as SyncResult;
      setLastSync(result.syncedAt); // triggers re-render → loop
    };

    syncPreferences();
  }, [preferences]); // preferences is a new object on EVERY render

  const updateTheme = (theme: UserPreferences['theme']): void => {
    setPreferences(prev => ({ ...prev, theme }));
  };

  return (
    <div>
      <select
        value={preferences.theme}
        onChange={e =>
          updateTheme(e.target.value as UserPreferences['theme'])
        }
      >
        <option value="light">Light</option>
        <option value="dark">Dark</option>
      </select>
      {lastSync && <p>Last synced: {lastSync}</p>}
    </div>
  );
}
```

**Your tasks:**
- ✅ Identify exactly why the infinite loop happens
- ✅ Fix it so sync only happens when preferences actually change
- ✅ Prevent sync from firing on the initial mount
- ✅ Debounce sync so rapid preference changes only trigger one API call

<details>
<summary>💡 Hint</summary>

The problem is `preferences` in the dependency array. Every render
creates a new `preferences` object reference — even with identical
contents — so React always sees a changed dependency.
Fix 1: use individual primitive values in the dep array instead
of the whole object. Fix 2: use a `useRef` to track the previous
value and only sync when something actually changed.
Also: `setLastSync` inside the effect triggers a re-render which
runs the effect again — separate concerns.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import { useState, useEffect, useRef } from 'react';

function PreferencesSync() {
  const [preferences, setPreferences] = useState<UserPreferences>({
    theme: 'light',
    language: 'en',
    notificationsEnabled: true,
    itemsPerPage: 10,
  });
  const [lastSync, setLastSync] = useState<string | null>(null);
  const [isSyncing, setIsSyncing] = useState<boolean>(false);

  // Track if this is the initial mount
  const isFirstRender = useRef<boolean>(true);

  // Track previous preferences to detect real changes
  const prevPreferencesRef = useRef<UserPreferences>(preferences);

  useEffect(() => {
    // Skip sync on initial mount — preferences have not changed yet
    if (isFirstRender.current) {
      isFirstRender.current = false;
      return;
    }

    // Check if preferences actually changed (deep compare key values)
    const prev = prevPreferencesRef.current;
    const hasChanged =
      prev.theme !== preferences.theme ||
      prev.language !== preferences.language ||
      prev.notificationsEnabled !== preferences.notificationsEnabled ||
      prev.itemsPerPage !== preferences.itemsPerPage;

    if (!hasChanged) return; // no real change — skip sync

    // Update the ref to current values
    prevPreferencesRef.current = preferences;

    // Debounce — wait 1 second after last change before syncing
    const debounceTimer = setTimeout(async () => {
      try {
        setIsSyncing(true);
        const res = await fetch('/api/user/preferences', {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(preferences),
        });

        if (!res.ok) throw new Error('Sync failed');

        const result = await res.json() as SyncResult;

        // FIX: setLastSync does cause re-render, but isFirstRender
        // is now false AND prevPreferencesRef equals current preferences
        // so the effect will run but exit early at the hasChanged check
        setLastSync(result.syncedAt);

      } catch (err) {
        console.error('Preferences sync failed:', err);
      } finally {
        setIsSyncing(false);
      }
    }, 1000);

    return () => clearTimeout(debounceTimer);

  // FIX: use individual primitive values, not the whole object
  // Primitives are compared by VALUE — no false positives
  }, [
    preferences.theme,
    preferences.language,
    preferences.notificationsEnabled,
    preferences.itemsPerPage,
  ]);

  const updatePreference = <K extends keyof UserPreferences>(
    key: K,
    value: UserPreferences[K]
  ): void => {
    setPreferences(prev => ({ ...prev, [key]: value }));
  };

  return (
    <div>
      <select
        value={preferences.theme}
        onChange={e =>
          updatePreference('theme', e.target.value as UserPreferences['theme'])
        }
      >
        <option value="light">Light</option>
        <option value="dark">Dark</option>
      </select>

      <select
        value={preferences.itemsPerPage}
        onChange={e =>
          updatePreference('itemsPerPage', Number(e.target.value))
        }
      >
        <option value={10}>10 per page</option>
        <option value={25}>25 per page</option>
        <option value={50}>50 per page</option>
      </select>

      <div style={{ fontSize: 12, color: '#666' }}>
        {isSyncing && <span>Syncing...</span>}
        {!isSyncing && lastSync && <span>Synced at {lastSync}</span>}
      </div>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to hear you explain: objects in dependency arrays are compared
by reference, not value. Every render creates a new object reference.
The fix is either primitive values in the dep array or a ref-based
comparison. They also look for skipping the initial mount.

🔷 **TypeScript lesson:**
The generic `updatePreference<K extends keyof UserPreferences>` function
is a production pattern that gives you fully typed preference updates.
TypeScript ensures you can only pass valid keys and matching value types —
`updatePreference('theme', 'dark')` works but `updatePreference('theme', 123)`
is a type error. This replaces a dozen individual handlers with one safe function.

⚡ **Why this matters in production:**
A single infinite loop user can generate tens of thousands of API calls
per minute, overwhelming your backend for everyone. This has caused
real production outages at real companies.

🔍 **What a senior dev spots in 10 seconds:**
An object or array variable in a `useEffect` dependency array,
combined with a `setState` call inside the same effect.
That is the infinite loop pattern.

✅ **Before you ship this, verify:**
```
□ Are all dependency array items primitives, not objects or arrays?
□ Does the effect skip the initial render if needed?
□ Does any setState inside the effect trigger a re-run?
□ If yes — does the re-run have an early exit condition?
□ Are objects stabilized with useMemo before going into dep arrays?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

### 🔴 Pattern 17 · Scenario 2 — The Object Dependency That Never Stabilizes

🏢 **Situation:**
You are at an e-commerce company. A developer adds a product
recommendations widget. It works perfectly in isolation.
But when it is placed on the product detail page, the entire
page re-renders 60 times per second. The browser shows 100% CPU.
The bug was deployed on a Monday morning. You get a Slack message
at 9:03am: "the site is completely broken for anyone viewing
a product page."

🔷 **Types for this scenario:**

```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  categoryId: string;
}

interface RecommendationFilters {
  categoryId: string;
  maxPrice: number;
  excludeIds: string[];
}

interface RecommendationsWidgetProps {
  product: Product;
}
```

**Here is the current code:**

```typescript
import { useState, useEffect } from 'react';

function RecommendationsWidget({ product }: RecommendationsWidgetProps) {
  const [recommendations, setRecommendations] = useState<Product[]>([]);

  // BUG: this object is created on EVERY render
  // It is a new reference each time — even with identical values
  const filters: RecommendationFilters = {
    categoryId: product.categoryId,
    maxPrice: product.price * 1.5,
    excludeIds: [product.id], // new array every render
  };

  useEffect(() => {
    const loadRecommendations = async (): Promise<void> => {
      const params = new URLSearchParams({
        categoryId: filters.categoryId,
        maxPrice: filters.maxPrice.toString(),
        excludeIds: filters.excludeIds.join(','),
      });

      const res = await fetch(`/api/products/recommendations?${params}`);
      const data = await res.json() as Product[];
      setRecommendations(data); // triggers re-render → filters recreated → loop
    };

    loadRecommendations();
  }, [filters]); // new object every render → effect runs every render → infinite

  return (
    <div>
      <h3>You might also like</h3>
      {recommendations.map(rec => (
        <div key={rec.id}>
          <span>{rec.name}</span>
          <span>₹{rec.price}</span>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the infinite loop caused by the unstable object reference
- ✅ Use primitive values in the dependency array instead of the object
- ✅ Add an AbortController to cancel the previous fetch
- ✅ Add loading and error states

<details>
<summary>💡 Hint</summary>

Remove `filters` from the dependency array entirely.
Instead, put the primitive values that make up the filters directly
in the dependency array: `product.categoryId`, `product.price`,
`product.id`. These are strings and numbers — compared by value,
not reference. They only trigger the effect when they actually change.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import { useState, useEffect } from 'react';

function RecommendationsWidget({ product }: RecommendationsWidgetProps) {
  const [recommendations, setRecommendations] = useState<Product[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const controller = new AbortController();

    const loadRecommendations = async (): Promise<void> => {
      try {
        setIsLoading(true);
        setError(null);

        // Build filters INSIDE the effect using the primitive dep values
        // These are computed fresh each time the effect runs — correctly
        const params = new URLSearchParams({
          categoryId: product.categoryId,
          maxPrice: (product.price * 1.5).toString(),
          excludeIds: product.id, // exclude current product
        });

        const res = await fetch(
          `/api/products/recommendations?${params}`,
          { signal: controller.signal }
        );

        if (!res.ok) throw new Error('Failed to load recommendations');

        const data = await res.json() as Product[];
        setRecommendations(data);

      } catch (err) {
        if (err instanceof Error && err.name === 'AbortError') return;
        setError('Could not load recommendations.');
      } finally {
        setIsLoading(false);
      }
    };

    loadRecommendations();

    return () => controller.abort();

  }, [
    // FIX: use PRIMITIVES not the filters object
    // Strings and numbers are compared by VALUE
    // Only re-runs when product actually changes — not every render
    product.categoryId,
    product.price,
    product.id,
  ]);

  if (isLoading) return <p>Loading recommendations...</p>;
  if (error) return null; // fail silently for recommendations widget
  if (recommendations.length === 0) return null;

  return (
    <div>
      <h3>You might also like</h3>
      {recommendations.map(rec => (
        <div key={rec.id}>
          <span>{rec.name}</span>
          <span>₹{rec.price.toLocaleString()}</span>
        </div>
      ))}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The root cause explanation: objects and arrays in dep arrays are
compared by reference, not content. The fix: extract primitive values
from the object and use those in the dependency array. This is the
most important useEffect knowledge a React developer needs.

🔷 **TypeScript lesson:**
When you use `product.categoryId` in the dependency array instead
of `product`, TypeScript's type system helps you here — if you later
rename a field on the Product interface, TypeScript will show a type
error at the dependency array access, reminding you to update
the dependencies. Object spreading hides this safety net.

⚡ **Why this matters in production:**
Infinite render loops max out CPU on the user's device. The browser
tab becomes unresponsive. Users close the tab and do not come back.
For an e-commerce site this directly means lost sales.

🔍 **What a senior dev spots in 10 seconds:**
An object or array variable (not primitive) sitting in a useEffect
dependency array. Especially when that object is defined in the
component body above the useEffect — it is recreated every render.

✅ **Before you ship this, verify:**
```
□ Are there any non-primitive values in any dependency array?
□ If yes — are they wrapped in useMemo to stabilize the reference?
□ OR — are the primitives inside them used instead?
□ Does the effect produce a setState that could trigger it again?
□ If yes — is there a stable exit condition that stops the loop?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 18: 🔑 Token Expiry + Silent Refresh

> **Level 4 — Specialist**

🎯 **What you will build in this pattern:**
- An Axios interceptor that catches 401 responses and silently refreshes the token
- A queue that holds failed requests while token refresh is in progress
- Multi-tab logout sync using BroadcastChannel

📋 **Interview asks this as:**
- "How do you handle JWT token expiry in React?"
- "How do you implement silent token refresh?"
- "What happens if 5 requests fail with 401 simultaneously?"

🔷 **TypeScript concepts used in this pattern:**
- Typed Axios interceptors
- Generic Promise queue with typed resolution
- Typed BroadcastChannel messages with discriminated unions

🚨 **Most common mistake in interviews:**
Not handling the case where multiple requests fail with 401 at the
same time. Without a queue, each 401 triggers its own refresh call —
the server sees 5 simultaneous refresh requests, some fail, tokens
get out of sync, and the user gets logged out anyway.

---

### 🔴 Pattern 18 · Scenario 1 — The User Who Gets Logged Out Mid-Work

🏢 **Situation:**
You are at a banking app. Users are complaining they get logged out
randomly while actively using the app. Your logs show they are getting
401 responses. The access token expires after 15 minutes. The app
has no refresh logic — when the token expires, every API call fails
and the app redirects to login. A user filling out a transfer form
for 20 minutes loses all their work. This is a critical UX failure
for a banking product.

🔷 **Types for this scenario:**

```typescript
interface AuthTokens {
  accessToken: string;
  refreshToken: string;
  expiresAt: number; // Unix timestamp in ms
}

interface RefreshResponse {
  accessToken: string;
  expiresAt: number;
}

// In-memory token store (not localStorage — more secure)
let tokenStore: AuthTokens | null = null;
```

**Here is the current code:**

```typescript
// api.ts — current broken version

import axios, { AxiosInstance } from 'axios';

const api: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
});

// Only adds the token — no refresh logic
api.interceptors.request.use(config => {
  const token = localStorage.getItem('accessToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// No response interceptor — 401 goes straight to the component
// Component has no idea how to handle it
// User gets redirected to login losing all work

export default api;
```

**Your tasks:**
- ✅ Add a response interceptor that catches 401 errors
- ✅ Automatically call the refresh endpoint to get a new token
- ✅ Retry the original failed request with the new token
- ✅ Redirect to login only if the refresh itself fails

<details>
<summary>💡 Hint</summary>

Add an Axios response interceptor. When status is 401:
1. Call your refresh endpoint
2. Store the new token
3. Update the failed request's Authorization header
4. Retry the original request with `axios(originalRequest)`
The tricky part: if refresh fails (refresh token also expired),
clear everything and redirect to login. Use `_retry` flag on the
config to prevent infinite retry loops.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
// api.ts

import axios, {
  AxiosInstance,
  AxiosError,
  InternalAxiosRequestConfig,
} from 'axios';

// Extend Axios config type to add our custom _retry flag
interface RetryableAxiosRequestConfig
  extends InternalAxiosRequestConfig {
  _retry?: boolean;
}

// In-memory token store — never in localStorage for sensitive apps
let accessToken: string | null = null;

export const setAccessToken = (token: string): void => {
  accessToken = token;
};

export const clearAccessToken = (): void => {
  accessToken = null;
};

export const getAccessToken = (): string | null => accessToken;

const api: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  withCredentials: true, // send httpOnly cookie for refresh token
});

// REQUEST INTERCEPTOR — attach current access token to every request
api.interceptors.request.use(
  (config: InternalAxiosRequestConfig): InternalAxiosRequestConfig => {
    if (accessToken) {
      config.headers.Authorization = `Bearer ${accessToken}`;
    }
    return config;
  },
  (error: AxiosError) => Promise.reject(error)
);

// RESPONSE INTERCEPTOR — catch 401, refresh token, retry
api.interceptors.response.use(
  // Success — pass through unchanged
  response => response,

  // Error handler
  async (error: AxiosError) => {
    const originalRequest = error.config as RetryableAxiosRequestConfig;

    // Only handle 401 errors that have not been retried yet
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true; // prevent infinite retry loop

      try {
        // Call refresh endpoint — refresh token is in httpOnly cookie
        // withCredentials: true sends the cookie automatically
        const res = await axios.post<RefreshResponse>(
          `${import.meta.env.VITE_API_URL}/auth/refresh`,
          {},
          { withCredentials: true }
        );

        const newToken = res.data.accessToken;

        // Store the new access token in memory
        setAccessToken(newToken);

        // Update the failed request's Authorization header
        originalRequest.headers.Authorization = `Bearer ${newToken}`;

        // Retry the original request with the new token
        return api(originalRequest);

      } catch (refreshError) {
        // Refresh failed — token completely expired or invalid
        // Clear auth state and redirect to login
        clearAccessToken();

        // Use BroadcastChannel to log out all tabs
        const channel = new BroadcastChannel('auth');
        channel.postMessage({ type: 'LOGOUT' });
        channel.close();

        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }

    return Promise.reject(error);
  }
);

export default api;
```

**✅ What the interviewer is checking:**
The `_retry` flag — this prevents the interceptor from retrying
infinitely if the refresh endpoint itself returns 401.
They also check `withCredentials: true` for the refresh call
(to send the httpOnly cookie) and the logout redirect when
refresh fails.

🔷 **TypeScript lesson:**
Extending `InternalAxiosRequestConfig` with a custom interface
to add `_retry` is the correct TypeScript approach — it avoids
casting to `any` and keeps type safety throughout the interceptor.
`RetryableAxiosRequestConfig` documents the intent and prevents
accidental misuse.

⚡ **Why this matters in production:**
Without silent refresh, users in banking or productivity apps
lose work every 15 minutes. With it, a session can transparently
last hours without interruption — matching user expectations
from apps like Gmail and Google Docs.

🔍 **What a senior dev spots in 10 seconds:**
A response interceptor that handles 401 without a `_retry` flag.
That is an infinite loop waiting to happen. Also: the refresh
call being made with the same `api` instance — which would also
trigger the interceptor — instead of a base `axios` call.

✅ **Before you ship this, verify:**
```
□ Does the _retry flag prevent infinite retry loops?
□ Is the refresh call made with base axios, not the intercepted instance?
□ Is withCredentials: true on the refresh call for httpOnly cookies?
□ Does failed refresh clear tokens AND redirect?
□ Does the retry use the newly obtained token, not the old one?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

### 🔴 Pattern 18 · Scenario 2 — The Five Requests That All Refresh at Once

🏢 **Situation:**
Your team improved the auth interceptor from Scenario 1.
It works great for single requests. But your QA engineer finds
a new bug: when the token expires, if the dashboard loads 5
widgets simultaneously, all 5 get 401, all 5 try to refresh,
the server receives 5 simultaneous refresh requests, 4 of them
fail because the refresh token was already consumed by the first,
and the user gets logged out. Fix the race condition.

🔷 **Types for this scenario:**

```typescript
interface QueuedRequest {
  resolve: (token: string) => void;
  reject: (error: unknown) => void;
}

type TokenRefreshState = 'idle' | 'refreshing';
```

**Here is the current code:**

```typescript
// The interceptor from Scenario 1 — works for one request
// but breaks with simultaneous 401s

api.interceptors.response.use(
  response => response,
  async (error: AxiosError) => {
    const originalRequest = error.config as RetryableAxiosRequestConfig;

    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;

      // BUG: if 5 requests get 401 simultaneously,
      // all 5 reach this code before any refresh completes
      // All 5 call the refresh endpoint
      // The refresh token can only be used once
      // 4 of the 5 refresh calls fail
      // User gets logged out

      try {
        const res = await axios.post<RefreshResponse>(
          `${import.meta.env.VITE_API_URL}/auth/refresh`,
          {},
          { withCredentials: true }
        );

        setAccessToken(res.data.accessToken);
        originalRequest.headers.Authorization =
          `Bearer ${res.data.accessToken}`;
        return api(originalRequest);

      } catch {
        clearAccessToken();
        window.location.href = '/login';
        return Promise.reject(error);
      }
    }

    return Promise.reject(error);
  }
);
```

**Your tasks:**
- ✅ Fix the race condition — only ONE refresh call should happen at a time
- ✅ Queue all other 401 requests while refresh is in progress
- ✅ After refresh succeeds, retry ALL queued requests with the new token
- ✅ If refresh fails, reject ALL queued requests and log out

<details>
<summary>💡 Hint</summary>

Use a module-level queue array and a boolean flag.
The first 401 sets the flag to "refreshing" and starts the refresh.
All subsequent 401s check the flag — if already refreshing, they
add themselves to the queue and wait.
When refresh completes, loop through the queue and resolve each
promise with the new token. Each queued request then retries itself.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
// api.ts — with race condition fix

import axios, {
  AxiosInstance,
  AxiosError,
  InternalAxiosRequestConfig,
} from 'axios';

interface RetryableAxiosRequestConfig
  extends InternalAxiosRequestConfig {
  _retry?: boolean;
}

// Queue for requests waiting on token refresh
let failedQueue: QueuedRequest[] = [];
let refreshState: TokenRefreshState = 'idle';

// Process all queued requests after refresh attempt
const processQueue = (
  error: unknown,
  token: string | null = null
): void => {
  failedQueue.forEach(({ resolve, reject }) => {
    if (error) {
      reject(error); // refresh failed — reject all queued requests
    } else if (token) {
      resolve(token); // refresh succeeded — give all requests the new token
    }
  });

  // Clear the queue
  failedQueue = [];
};

let accessToken: string | null = null;
export const setAccessToken = (token: string): void => { accessToken = token; };
export const clearAccessToken = (): void => { accessToken = null; };

const api: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  withCredentials: true,
});

api.interceptors.request.use(config => {
  if (accessToken) {
    config.headers.Authorization = `Bearer ${accessToken}`;
  }
  return config;
});

api.interceptors.response.use(
  response => response,
  (error: AxiosError): Promise<unknown> => {
    const originalRequest = error.config as RetryableAxiosRequestConfig;

    if (error.response?.status !== 401 || originalRequest._retry) {
      return Promise.reject(error);
    }

    // Is a refresh already in progress?
    if (refreshState === 'refreshing') {
      // Queue this request — it will retry when refresh completes
      return new Promise<string>((resolve, reject) => {
        failedQueue.push({ resolve, reject });
      }).then(token => {
        // Refresh completed — retry with new token
        originalRequest.headers.Authorization = `Bearer ${token}`;
        originalRequest._retry = true;
        return api(originalRequest);
      }).catch(err => {
        return Promise.reject(err);
      });
    }

    // First 401 — start the refresh process
    originalRequest._retry = true;
    refreshState = 'refreshing';

    return new Promise(async (resolve, reject) => {
      try {
        const res = await axios.post<RefreshResponse>(
          `${import.meta.env.VITE_API_URL}/auth/refresh`,
          {},
          { withCredentials: true }
        );

        const newToken = res.data.accessToken;
        setAccessToken(newToken);

        // Retry original request
        originalRequest.headers.Authorization = `Bearer ${newToken}`;

        // Resolve all queued requests with new token
        processQueue(null, newToken);

        resolve(api(originalRequest));

      } catch (refreshError) {
        // Refresh failed — reject everything and logout
        processQueue(refreshError, null);
        clearAccessToken();

        const channel = new BroadcastChannel('auth');
        channel.postMessage({ type: 'LOGOUT' } satisfies { type: string });
        channel.close();

        window.location.href = '/login';
        reject(refreshError);

      } finally {
        refreshState = 'idle'; // always reset state
      }
    });
  }
);

export default api;
```

**✅ What the interviewer is checking:**
The `failedQueue` array and `refreshState` flag — these are the exact
mechanism that prevents multiple simultaneous refresh calls.
They also check `processQueue` which must handle both success and
failure cases, and that `refreshState` is reset in `finally`.

🔷 **TypeScript lesson:**
The `QueuedRequest` interface with `resolve` and `reject` typed
as functions gives you type-safe promise control. The `satisfies`
operator on the BroadcastChannel message ensures the message
matches the expected type without widening it to `any` — a modern
TypeScript pattern that is cleaner than type assertions.

⚡ **Why this matters in production:**
Without the queue, a dashboard with 5 API calls will always fail
when the token expires because the refresh token is single-use.
The user gets logged out despite actively using the app. This is
a trust-destroying experience in any application, especially fintech.

🔍 **What a senior dev spots in 10 seconds:**
A refresh call inside a 401 interceptor with no flag or queue
to prevent concurrent refreshes. The question they ask: "what
happens if this interceptor fires for 3 requests at once?"

✅ **Before you ship this, verify:**
```
□ Is refreshState reset to 'idle' in a finally block?
□ Does processQueue handle both success and failure?
□ Is failedQueue cleared after processing?
□ Are queued requests retried with the new token, not the old one?
□ Does the BroadcastChannel close after sending the logout message?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 19: 📦 Context Performance at Scale

> **Level 4 — Specialist**

🎯 **What you will fix in this pattern:**
- A single Context that holds everything — causes the entire app to re-render on any change
- Context consumers re-rendering when they only use a small slice of the value
- An app that works fine with 10 components but becomes unusable at 100

📋 **Interview asks this as:**
- "What is the Context re-render problem?"
- "How do you optimize Context to prevent unnecessary re-renders?"
- "When would you choose Zustand over Context API?"

🔷 **TypeScript concepts used in this pattern:**
- Generic context factory functions
- Typed selectors with useMemo
- Typed context splitting with separate interfaces per domain

🚨 **Most common mistake in interviews:**
Putting everything in one context object. When any value in the object
changes, ALL consumers re-render — even the ones that only use a
different part of the object. Split contexts by update frequency,
not by convenience.

---

### 🔴 Pattern 19 · Scenario 1 — The App That Re-renders Everything on Every Click

🏢 **Situation:**
You are at an HR tool company. The app has one global `AppContext`
holding the current user, theme, language, notifications, and cart.
Your React DevTools Profiler shows every single component in the app
re-renders whenever anyone clicks anything. Even changing the language
causes the entire sidebar to re-render. With 80 components in the tree
the app visibly lags on every interaction. Your senior dev says:
"The context architecture is wrong. Fix it."

🔷 **Types for this scenario:**

```typescript
interface User {
  id: string;
  name: string;
  role: 'admin' | 'manager' | 'employee';
  email: string;
}

interface Theme {
  mode: 'light' | 'dark';
  primaryColor: string;
}

interface AppNotification {
  id: string;
  message: string;
  type: 'info' | 'success' | 'warning' | 'error';
  isRead: boolean;
}

// BAD: Everything in one context — changes to ANY field
// cause ALL consumers to re-render
interface AppContextType {
  user: User | null;
  theme: Theme;
  language: string;
  notifications: AppNotification[];
  setTheme: (theme: Theme) => void;
  setLanguage: (lang: string) => void;
  markNotificationRead: (id: string) => void;
  logout: () => void;
}
```

**Here is the current code:**

```typescript
import {
  createContext, useContext, useState,
  ReactNode, useCallback
} from 'react';

const AppContext = createContext<AppContextType | null>(null);

function AppProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [theme, setThemeState] = useState<Theme>({
    mode: 'light',
    primaryColor: '#0066cc',
  });
  const [language, setLanguage] = useState<string>('en');
  const [notifications, setNotifications] = useState<AppNotification[]>([]);

  const setTheme = (newTheme: Theme): void => setThemeState(newTheme);

  const markNotificationRead = (id: string): void => {
    setNotifications(prev =>
      prev.map(n => n.id === id ? { ...n, isRead: true } : n)
    );
  };

  const logout = (): void => {
    setUser(null);
    localStorage.removeItem('token');
  };

  // BUG: entire value object is recreated on every render
  // Every consumer re-renders on any state change — even unrelated ones
  return (
    <AppContext.Provider value={{
      user, theme, language, notifications,
      setTheme, setLanguage, markNotificationRead, logout,
    }}>
      {children}
    </AppContext.Provider>
  );
}
```

**Your tasks:**
- ✅ Split the single context into multiple focused contexts
- ✅ Separate by update frequency: user (rarely), theme (sometimes), notifications (often)
- ✅ Use `useMemo` to stabilize context values and prevent unnecessary re-renders
- ✅ Create typed custom hooks for each context

<details>
<summary>💡 Hint</summary>

The rule: split contexts by HOW OFTEN they change, not by what
they are related to. User data changes rarely (login/logout only).
Theme changes occasionally (user toggles). Notifications change
frequently (new ones arrive). When you separate these, changing
a notification only re-renders notification consumers — not
the entire app including the sidebar and user profile.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import {
  createContext, useContext, useState,
  useMemo, useCallback, ReactNode
} from 'react';

// ─────────────────────────────────────────────
// CONTEXT 1: Auth/User — changes rarely (login/logout only)
// ─────────────────────────────────────────────

interface AuthContextType {
  user: User | null;
  login: (user: User) => void;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);

  const login = useCallback((newUser: User): void => {
    setUser(newUser);
  }, []);

  const logout = useCallback((): void => {
    setUser(null);
    localStorage.removeItem('token');
  }, []);

  // useMemo: only new object when user changes — not every render
  const value = useMemo<AuthContextType>(() => ({
    user,
    login,
    logout,
    isAuthenticated: user !== null,
  }), [user, login, logout]);

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth(): AuthContextType {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error('useAuth must be inside AuthProvider');
  return ctx;
}

// ─────────────────────────────────────────────
// CONTEXT 2: Theme — changes occasionally
// ─────────────────────────────────────────────

interface ThemeContextType {
  theme: Theme;
  setTheme: (theme: Theme) => void;
  toggleMode: () => void;
}

const ThemeContext = createContext<ThemeContextType | null>(null);

function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setThemeState] = useState<Theme>({
    mode: 'light',
    primaryColor: '#0066cc',
  });

  const setTheme = useCallback((newTheme: Theme): void => {
    setThemeState(newTheme);
  }, []);

  const toggleMode = useCallback((): void => {
    setThemeState(prev => ({
      ...prev,
      mode: prev.mode === 'light' ? 'dark' : 'light',
    }));
  }, []);

  const value = useMemo<ThemeContextType>(() => ({
    theme,
    setTheme,
    toggleMode,
  }), [theme, setTheme, toggleMode]);

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme(): ThemeContextType {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be inside ThemeProvider');
  return ctx;
}

// ─────────────────────────────────────────────
// CONTEXT 3: Notifications — changes frequently
// ─────────────────────────────────────────────

interface NotificationsContextType {
  notifications: AppNotification[];
  unreadCount: number;
  addNotification: (notification: Omit<AppNotification, 'id' | 'isRead'>) => void;
  markRead: (id: string) => void;
  markAllRead: () => void;
}

const NotificationsContext = createContext<NotificationsContextType | null>(null);

function NotificationsProvider({ children }: { children: ReactNode }) {
  const [notifications, setNotifications] = useState<AppNotification[]>([]);

  const addNotification = useCallback(
    (n: Omit<AppNotification, 'id' | 'isRead'>): void => {
      const newNotification: AppNotification = {
        ...n,
        id: crypto.randomUUID(),
        isRead: false,
      };
      setNotifications(prev => [newNotification, ...prev]);
    },
    []
  );

  const markRead = useCallback((id: string): void => {
    setNotifications(prev =>
      prev.map(n => n.id === id ? { ...n, isRead: true } : n)
    );
  }, []);

  const markAllRead = useCallback((): void => {
    setNotifications(prev => prev.map(n => ({ ...n, isRead: true })));
  }, []);

  const unreadCount = useMemo(
    () => notifications.filter(n => !n.isRead).length,
    [notifications]
  );

  const value = useMemo<NotificationsContextType>(() => ({
    notifications,
    unreadCount,
    addNotification,
    markRead,
    markAllRead,
  }), [notifications, unreadCount, addNotification, markRead, markAllRead]);

  return (
    <NotificationsContext.Provider value={value}>
      {children}
    </NotificationsContext.Provider>
  );
}

export function useNotifications(): NotificationsContextType {
  const ctx = useContext(NotificationsContext);
  if (!ctx) throw new Error('useNotifications must be inside NotificationsProvider');
  return ctx;
}

// ─────────────────────────────────────────────
// Root provider — composes all providers
// ─────────────────────────────────────────────

function AppProviders({ children }: { children: ReactNode }) {
  return (
    <AuthProvider>
      <ThemeProvider>
        <NotificationsProvider>
          {children}
        </NotificationsProvider>
      </ThemeProvider>
    </AuthProvider>
  );
}

export { AppProviders };
```

**✅ What the interviewer is checking:**
Three separate contexts instead of one. `useMemo` on the provider
value. `useCallback` on all functions passed in context (prevents
new references). Typed custom hooks that throw if used outside
their provider. They want to hear: split by change frequency,
not by feature similarity.

🔷 **TypeScript lesson:**
`Omit<AppNotification, 'id' | 'isRead'>` for `addNotification`
is a production pattern — callers should not provide the `id`
(that is generated internally) or `isRead` (always starts false).
Utility types like `Omit` and `Pick` in function signatures
make APIs self-documenting and enforce correct usage at compile time.

⚡ **Why this matters in production:**
One context with 10 pieces of state in an app with 80 components
means every notification re-renders the entire component tree.
Split contexts mean only the 3 components actually showing
notifications re-render. 27x less work for the same action.

🔍 **What a senior dev spots in 10 seconds:**
One giant context value object with user + theme + notifications
+ actions all together. The instant signal: if changing a notification
mark re-renders the top navbar showing the user's name — the contexts
are not split correctly.

✅ **Before you ship this, verify:**
```
□ Is each context split by change frequency (not feature)?
□ Is every context value wrapped in useMemo?
□ Are all functions in context value wrapped in useCallback?
□ Does every custom hook throw if used outside its provider?
□ Does the root AppProviders compose cleanly without prop drilling?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

### 🔴 Pattern 19 · Scenario 2 — The Context That Makes the App Unusable

🏢 **Situation:**
You are at a project management SaaS. The app has a `ProjectContext`
holding all projects and tasks — thousands of items. Every time
the user marks a single task as complete, the entire app re-renders
including the sidebar, header, and all other project views. The React
DevTools Profiler shows 1200ms render time for a single checkbox click.
Users on older laptops are reporting the app "freezes for a second"
every time they complete a task. This is your most-used feature.

🔷 **Types for this scenario:**

```typescript
interface Task {
  id: string;
  title: string;
  isComplete: boolean;
  projectId: string;
  assigneeId: string | null;
  dueDate: string | null;
}

interface Project {
  id: string;
  name: string;
  tasks: Task[];
  memberIds: string[];
}

interface ProjectContextType {
  projects: Project[];
  activeProjectId: string | null;
  setActiveProject: (id: string) => void;
  toggleTask: (taskId: string, projectId: string) => void;
  addTask: (projectId: string, task: Omit<Task, 'id'>) => void;
}
```

**Here is the current code:**

```typescript
import {
  createContext, useContext, useState,
  ReactNode
} from 'react';

const ProjectContext = createContext<ProjectContextType | null>(null);

function ProjectProvider({ children }: { children: ReactNode }) {
  const [projects, setProjects] = useState<Project[]>([]);
  const [activeProjectId, setActiveProjectId] = useState<string | null>(null);

  const toggleTask = (taskId: string, projectId: string): void => {
    setProjects(prev => prev.map(project =>
      project.id !== projectId ? project : {
        ...project,
        tasks: project.tasks.map(task =>
          task.id !== taskId ? task : { ...task, isComplete: !task.isComplete }
        ),
      }
    ));
  };

  const addTask = (projectId: string, task: Omit<Task, 'id'>): void => {
    setProjects(prev => prev.map(project =>
      project.id !== projectId ? project : {
        ...project,
        tasks: [...project.tasks, { ...task, id: crypto.randomUUID() }],
      }
    ));
  };

  // BUG: new value object every render
  // ALL consumers re-render when ANY project or task changes
  // Including header, sidebar, other project views
  return (
    <ProjectContext.Provider value={{
      projects,
      activeProjectId,
      setActiveProject: setActiveProjectId,
      toggleTask,
      addTask,
    }}>
      {children}
    </ProjectContext.Provider>
  );
}
```

**Your tasks:**
- ✅ Fix the context so toggling one task only re-renders relevant components
- ✅ Separate stable data (project list) from frequently changing data (active task states)
- ✅ Use `useReducer` instead of `useState` for the projects state
- ✅ Memoize derived data: active project's tasks, completion percentage

<details>
<summary>💡 Hint</summary>

Split into two contexts: one for the data (projects, active project)
and one for actions (toggleTask, addTask, setActiveProject).
Actions never change — they are stable function references.
Data changes on every update. Components that only dispatch actions
(like a "complete task" button) subscribe only to the actions context
and never re-render when data changes.

</details>

<details>
<summary>✅ Full Solution</summary>

```typescript
import {
  createContext, useContext, useReducer,
  useMemo, useCallback, ReactNode
} from 'react';

// ─────────────────────────────────────────────
// Reducer — centralized state logic
// ─────────────────────────────────────────────

type ProjectAction =
  | { type: 'SET_PROJECTS'; payload: Project[] }
  | { type: 'TOGGLE_TASK'; payload: { taskId: string; projectId: string } }
  | { type: 'ADD_TASK'; payload: { projectId: string; task: Omit<Task, 'id'> } }
  | { type: 'SET_ACTIVE_PROJECT'; payload: string | null };

interface ProjectState {
  projects: Project[];
  activeProjectId: string | null;
}

function projectReducer(
  state: ProjectState,
  action: ProjectAction
): ProjectState {
  switch (action.type) {
    case 'SET_PROJECTS':
      return { ...state, projects: action.payload };

    case 'TOGGLE_TASK':
      return {
        ...state,
        projects: state.projects.map(project =>
          project.id !== action.payload.projectId ? project : {
            ...project,
            tasks: project.tasks.map(task =>
              task.id !== action.payload.taskId ? task : {
                ...task,
                isComplete: !task.isComplete,
              }
            ),
          }
        ),
      };

    case 'ADD_TASK':
      return {
        ...state,
        projects: state.projects.map(project =>
          project.id !== action.payload.projectId ? project : {
            ...project,
            tasks: [
              ...project.tasks,
              { ...action.payload.task, id: crypto.randomUUID() },
            ],
          }
        ),
      };

    case 'SET_ACTIVE_PROJECT':
      return { ...state, activeProjectId: action.payload };

    default:
      return state;
  }
}

// ─────────────────────────────────────────────
// DATA CONTEXT — re-renders when state changes
// ─────────────────────────────────────────────

interface ProjectDataContextType {
  projects: Project[];
  activeProjectId: string | null;
  activeProject: Project | null;
}

const ProjectDataContext =
  createContext<ProjectDataContextType | null>(null);

// ─────────────────────────────────────────────
// ACTIONS CONTEXT — never re-renders (functions are stable)
// ─────────────────────────────────────────────

interface ProjectActionsContextType {
  setActiveProject: (id: string | null) => void;
  toggleTask: (taskId: string, projectId: string) => void;
  addTask: (projectId: string, task: Omit<Task, 'id'>) => void;
  setProjects: (projects: Project[]) => void;
}

const ProjectActionsContext =
  createContext<ProjectActionsContextType | null>(null);

// ─────────────────────────────────────────────
// PROVIDER
// ─────────────────────────────────────────────

function ProjectProvider({ children }: { children: ReactNode }) {
  const [state, dispatch] = useReducer(projectReducer, {
    projects: [],
    activeProjectId: null,
  });

  // STABLE ACTIONS — useCallback with dispatch as dep
  // dispatch is stable — never changes — so these functions never change
  const setActiveProject = useCallback(
    (id: string | null): void =>
      dispatch({ type: 'SET_ACTIVE_PROJECT', payload: id }),
    []
  );

  const toggleTask = useCallback(
    (taskId: string, projectId: string): void =>
      dispatch({ type: 'TOGGLE_TASK', payload: { taskId, projectId } }),
    []
  );

  const addTask = useCallback(
    (projectId: string, task: Omit<Task, 'id'>): void =>
      dispatch({ type: 'ADD_TASK', payload: { projectId, task } }),
    []
  );

  const setProjects = useCallback(
    (projects: Project[]): void =>
      dispatch({ type: 'SET_PROJECTS', payload: projects }),
    []
  );

  // DERIVED DATA — computed from state, memoized
  const activeProject = useMemo<Project | null>(
    () => state.projects.find(p => p.id === state.activeProjectId) ?? null,
    [state.projects, state.activeProjectId]
  );

  // DATA VALUE — new object when state changes
  const dataValue = useMemo<ProjectDataContextType>(() => ({
    projects: state.projects,
    activeProjectId: state.activeProjectId,
    activeProject,
  }), [state.projects, state.activeProjectId, activeProject]);

  // ACTIONS VALUE — stable, only created once
  const actionsValue = useMemo<ProjectActionsContextType>(() => ({
    setActiveProject,
    toggleTask,
    addTask,
    setProjects,
  }), [setActiveProject, toggleTask, addTask, setProjects]);

  return (
    <ProjectActionsContext.Provider value={actionsValue}>
      <ProjectDataContext.Provider value={dataValue}>
        {children}
      </ProjectDataContext.Provider>
    </ProjectActionsContext.Provider>
  );
}

// ─────────────────────────────────────────────
// TYPED CUSTOM HOOKS
// ─────────────────────────────────────────────

export function useProjectData(): ProjectDataContextType {
  const ctx = useContext(ProjectDataContext);
  if (!ctx) throw new Error('useProjectData must be inside ProjectProvider');
  return ctx;
}

export function useProjectActions(): ProjectActionsContextType {
  const ctx = useContext(ProjectActionsContext);
  if (!ctx) throw new Error('useProjectActions must be inside ProjectProvider');
  return ctx;
}

// Convenience hook for components that need both
export function useProject(): ProjectDataContextType & ProjectActionsContextType {
  return { ...useProjectData(), ...useProjectActions() };
}

// Selector hook — only re-renders when the selected value changes
export function useActiveProjectTasks(): {
  tasks: Task[];
  completionPercentage: number;
} {
  const { activeProject } = useProjectData();

  return useMemo(() => {
    const tasks = activeProject?.tasks ?? [];
    const completed = tasks.filter(t => t.isComplete).length;
    const completionPercentage = tasks.length > 0
      ? Math.round((completed / tasks.length) * 100)
      : 0;
    return { tasks, completionPercentage };
  }, [activeProject]);
}

export { ProjectProvider };
```

**✅ What the interviewer is checking:**
The data/actions context split is the key answer. They also look
for `useReducer` for complex state, `useCallback` with empty deps
for stable action functions, and the typed `useProjectActions`
hook that never causes re-renders in action-only components.

🔷 **TypeScript lesson:**
The discriminated union `ProjectAction` type means TypeScript
enforces correct payloads for each action. `dispatch({ type: 'TOGGLE_TASK', payload: { taskId: 'x' } })`
is a TypeScript error because `projectId` is missing from the payload.
This is the key benefit of typed reducers — the compiler catches
dispatch mistakes at compile time, not runtime.

⚡ **Why this matters in production:**
A "complete task" button subscribed only to `useProjectActions`
never re-renders when tasks change. Without the split, every checkbox
click re-renders 80+ components. The difference is 1200ms vs 12ms
for the same user action.

🔍 **What a senior dev spots in 10 seconds:**
A context value that mixes state (changes frequently) with action
functions (should be stable) in one object without `useCallback`
or `useMemo`. That combination guarantees maximum unnecessary
re-renders as the app grows.

✅ **Before you ship this, verify:**
```
□ Are data and actions in separate contexts?
□ Are all action functions wrapped in useCallback?
□ Is the data context value wrapped in useMemo?
□ Does the actions context value never change after first render?
□ Do components subscribe only to what they actually use?
```

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Interview Pattern Picker — Level 4

> Use this when you recognize a symptom but are not sure which pattern applies.

| You See This | Go To |
|---|---|
| App gets slower the longer it runs | Pattern 15 — Memory Leaks |
| Browser tab crashes after extended use | Pattern 15 — Memory Leaks |
| Console shows: "Can't update state on unmounted component" | Pattern 15 — Memory Leaks |
| setInterval inside useEffect with no clearInterval | Pattern 15 — Scenario 1 |
| WebSocket or EventSource with no close() in cleanup | Pattern 15 — Scenario 2 |
| A value inside setInterval always shows the initial value | Pattern 16 — Stale Closures |
| useEffect with empty deps `[]` reading from state | Pattern 16 — Stale Closures |
| Search shows results for a query the user already cleared | Pattern 16 — Scenario 2 |
| Component renders forever / browser freezes on mount | Pattern 17 — Infinite Loops |
| useEffect fires on every render with no clear reason | Pattern 17 — Infinite Loops |
| Object or array variable sitting in a dep array | Pattern 17 — Scenario 2 |
| setState inside useEffect with that state in the dep array | Pattern 17 — Scenario 1 |
| User gets logged out randomly while actively using app | Pattern 18 — Token Expiry |
| All API calls fail at the same time after a period | Pattern 18 — Token Expiry |
| Multiple simultaneous 401 errors cause full logout | Pattern 18 — Scenario 2 |
| Auth token expires but user should stay logged in | Pattern 18 — Scenario 1 |
| Changing one setting re-renders unrelated components | Pattern 19 — Context Scale |
| React DevTools shows entire tree rendering on tiny changes | Pattern 19 — Context Scale |
| App works fine with 10 components, lags with 100 | Pattern 19 — Context Scale |
| One giant context object holding user + theme + everything | Pattern 19 — Scenario 1 |
| Marking a task complete causes 1 second freeze | Pattern 19 — Scenario 2 |
| "When should I use Zustand instead of Context?" | Pattern 19 — when updates are frequent |

---

# Production Readiness Checklist — Level 4

---

**Pattern 15 — Memory Leaks**
```
□ Does every setInterval have a clearInterval in the useEffect return?
□ Does every setTimeout have a clearTimeout in the useEffect return?
□ Does every addEventListener have a removeEventListener in cleanup?
□ Does every WebSocket / EventSource have a .close() in cleanup?
□ Does every fetch have an AbortController signal?
□ Is AbortError caught and silently ignored (not shown to user)?
```

---

**Pattern 16 — Stale Closures**
```
□ Does every interval/timeout callback that reads state also use a ref?
□ Is the ref updated every time the state it mirrors is updated?
□ Is the dependency array accurate — not just [] to avoid issues?
□ Are race conditions in async functions handled with AbortController?
□ Is the "last query wins" pattern used for search/filter operations?
```

---

**Pattern 17 — Infinite Render Loops**
```
□ Are all dependency array values primitives (string, number, boolean)?
□ If objects or arrays are in deps — are they wrapped in useMemo?
□ Does any setState inside useEffect risk triggering the same effect?
□ If yes — is there a guaranteed exit condition (ref check, value check)?
□ Are functions from parents that go in deps wrapped in useCallback?
```

---

**Pattern 18 — Token Expiry + Silent Refresh**
```
□ Is there a _retry flag to prevent infinite 401 retry loops?
□ Is the refresh call made with base axios, not the intercepted instance?
□ Does the interceptor queue concurrent 401s instead of all refreshing?
□ Is refreshState reset to 'idle' in a finally block?
□ Does failed refresh clear tokens AND redirect to login?
□ Does BroadcastChannel log out all open tabs on logout?
```

---

**Pattern 19 — Context Performance at Scale**
```
□ Are contexts split by change frequency, not by feature?
□ Is every context value wrapped in useMemo?
□ Are all functions in context values wrapped in useCallback?
□ Are data and actions in separate contexts?
□ Do components subscribe only to the context slice they need?
□ Is there a selector pattern for derived data (useMemo per hook)?
```

---

> **Part 4 complete.**
> You have finished all 4 levels of the React + TypeScript Practice Guide.
>
> **Full guide summary:**
> - Part 1 — Level 1 Foundation (Patterns 1–6) — 24 scenarios
> - Part 2 — Level 2 Intermediate (Patterns 7–11) — 20 scenarios
> - Part 3 — Level 3 Advanced (Patterns 12–14) — 12 scenarios
> - Part 4 — Level 4 Specialist (Patterns 15–19) — 10 scenarios
> - **Total: 66 scenarios covering 80% → 99% of real React work**