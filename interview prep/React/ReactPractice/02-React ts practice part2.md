# React + TypeScript Practice Guide — Part 2
## Level 2: Intermediate (15% of Interview Questions)

---

# Table of Contents

- [Pattern 7: ♾️ Pagination / Infinite Scroll](#pattern-7--pagination--infinite-scroll)
  - [Scenario 1 — The Table With No Pages](#-pattern-7--scenario-1--the-table-with-no-pages)
  - [Scenario 2 — The Pagination That Loses Filters](#-pattern-7--scenario-2--the-pagination-that-loses-filters)
  - [Scenario 3 — The Load More Button That Duplicates Data](#-pattern-7--scenario-3--the-load-more-button-that-duplicates-data)
  - [Scenario 4 — The Infinite Scroll That Never Stops](#-pattern-7--scenario-4--the-infinite-scroll-that-never-stops)
- [Pattern 8: 🔔 Toast / Notifications](#pattern-8--toast--notifications)
  - [Scenario 1 — The Action With No Feedback](#-pattern-8--scenario-1--the-action-with-no-feedback)
  - [Scenario 2 — The Toast That Stacks Forever](#-pattern-8--scenario-2--the-toast-that-stacks-forever)
  - [Scenario 3 — The Notification System With No Context](#-pattern-8--scenario-3--the-notification-system-with-no-context)
  - [Scenario 4 — The Toast That Ignores Errors](#-pattern-8--scenario-4--the-toast-that-ignores-errors)
- [Pattern 9: 🛒 Shopping Cart](#pattern-9--shopping-cart)
  - [Scenario 1 — The Cart That Cannot Update Quantity](#-pattern-9--scenario-1--the-cart-that-cannot-update-quantity)
  - [Scenario 2 — The Cart With Wrong Totals](#-pattern-9--scenario-2--the-cart-with-wrong-totals)
  - [Scenario 3 — The Cart That Does Not Persist](#-pattern-9--scenario-3--the-cart-that-does-not-persist)
  - [Scenario 4 — The Cart With No Stock Check](#-pattern-9--scenario-4--the-cart-with-no-stock-check)
- [Pattern 10: 🌙 Dark Mode / User Preferences](#pattern-10--dark-mode--user-preferences)
  - [Scenario 1 — The Dark Mode That Flashes on Load](#-pattern-10--scenario-1--the-dark-mode-that-flashes-on-load)
  - [Scenario 2 — The Theme That Does Not Reach Deep Components](#-pattern-10--scenario-2--the-theme-that-does-not-reach-deep-components)
  - [Scenario 3 — The Preferences That Do Not Sync](#-pattern-10--scenario-3--the-preferences-that-do-not-sync)
  - [Scenario 4 — The Theme That Ignores System Preference](#-pattern-10--scenario-4--the-theme-that-ignores-system-preference)
- [Pattern 11: 🔐 Auth Flow](#pattern-11--auth-flow)
  - [Scenario 1 — The Token Stored in the Wrong Place](#-pattern-11--scenario-1--the-token-stored-in-the-wrong-place)
  - [Scenario 2 — The Auth State That Disappears on Refresh](#-pattern-11--scenario-2--the-auth-state-that-disappears-on-refresh)
  - [Scenario 3 — The Role-Based UI That Anyone Can See](#-pattern-11--scenario-3--the-role-based-ui-that-anyone-can-see)
  - [Scenario 4 — The Token That Expires Mid-Session](#-pattern-11--scenario-4--the-token-that-expires-mid-session)
- [Interview Pattern Picker](#interview-pattern-picker)

---

# Pattern 7: ♾️ Pagination / Infinite Scroll

> **Level 2 — Intermediate**

🎯 **What you will build in this pattern:**
- A data table with page numbers, next/prev controls, and page size selector
- A product feed with a "Load More" button that appends results
- An infinite scroll list that fetches the next page when the user reaches the bottom

📋 **Interview asks this as:**
- "Add pagination to this user table"
- "Build a load more button that appends results without replacing them"
- "Implement infinite scroll on this product list"

🔷 **TypeScript concepts used in this pattern:**
- Generic `PaginatedResponse<T>` interface for API responses
- `useRef<HTMLDivElement>(null)` for scroll detection with IntersectionObserver
- Discriminated union for pagination state

🚨 **Most common mistake in interviews:**
Replacing the list instead of appending on "load more" — `setItems(newItems)` instead of `setItems(prev => [...prev, ...newItems])`. Also forgetting to handle the "no more pages" state so the button keeps showing after all data is loaded.

---

### 🔴 Pattern 7 · Scenario 1 — The Table With No Pages

🏢 **Situation:**
You are at an HR tool company. The employee table loads all
500 employees at once and dumps them on one page. The browser
slows to a crawl. Your tech lead says: "Add pagination — 10
employees per page, with page numbers and next/prev buttons.
Ship it today."

🔷 **Types for this scenario:**
```typescript
interface Employee {
  id: string;
  name: string;
  department: string;
  email: string;
  joinedAt: string;
}

interface PaginatedResponse<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
  totalPages: number;
}

interface PaginationState {
  page: number;
  pageSize: number;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

function EmployeeTable(): JSX.Element {
  const [employees, setEmployees] = useState<Employee[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);

  useEffect(() => {
    const load = async (): Promise<void> => {
      const res = await fetch('/api/employees');
      const data = await res.json();
      setEmployees(data); // loads ALL 500 at once
      setIsLoading(false);
    };
    load();
  }, []);

  if (isLoading) return <p>Loading...</p>;

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Department</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        {employees.map(emp => (
          <tr key={emp.id}>
            <td>{emp.name}</td>
            <td>{emp.department}</td>
            <td>{emp.email}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

**Your tasks:**
- ✅ Add server-side pagination — fetch only 10 records per page
- ✅ Add page number buttons (1, 2, 3... up to total pages)
- ✅ Add Previous and Next buttons, disabled at boundaries
- ✅ Show "Showing 1–10 of 500 employees" count
- ✅ Type the API response with `PaginatedResponse<Employee>`

<details>
<summary>💡 Hint</summary>

The API needs to receive `page` and `pageSize` as query params.
Store `page` in state — when it changes, the useEffect
re-fetches. Also store the full `PaginatedResponse` so you
have access to `totalPages` for building the page buttons.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface Employee {
  id: string;
  name: string;
  department: string;
  email: string;
  joinedAt: string;
}

interface PaginatedResponse<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
  totalPages: number;
}

const PAGE_SIZE = 10;

function EmployeeTable(): JSX.Element {
  const [response, setResponse] = useState<PaginatedResponse<Employee> | null>(null);
  const [page, setPage] = useState<number>(1);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        setIsLoading(true);
        setError(null);

        // Pass page and pageSize as query params
        const res = await fetch(
          `/api/employees?page=${page}&pageSize=${PAGE_SIZE}`
        );
        if (!res.ok) throw new Error('Failed to load employees');

        const data = await res.json() as PaginatedResponse<Employee>;
        setResponse(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setIsLoading(false);
      }
    };

    load();
  }, [page]); // re-fetch whenever page changes

  if (error) return <p>Error: {error}</p>;
  if (isLoading) return <p>Loading employees...</p>;
  if (!response) return <p>No data</p>;

  const { items, total, totalPages } = response;
  const startItem = (page - 1) * PAGE_SIZE + 1;
  const endItem = Math.min(page * PAGE_SIZE, total);

  return (
    <div>
      {/* Result count */}
      <p>
        Showing {startItem}–{endItem} of {total} employees
      </p>

      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Department</th>
            <th>Email</th>
          </tr>
        </thead>
        <tbody>
          {items.map(emp => (
            <tr key={emp.id}>
              <td>{emp.name}</td>
              <td>{emp.department}</td>
              <td>{emp.email}</td>
            </tr>
          ))}
        </tbody>
      </table>

      {/* Pagination controls */}
      <div style={{ display: 'flex', gap: 8, alignItems: 'center', marginTop: 16 }}>
        {/* Previous button */}
        <button
          onClick={() => setPage(p => p - 1)}
          disabled={page === 1}
        >
          Previous
        </button>

        {/* Page number buttons */}
        {Array.from({ length: totalPages }, (_, i) => i + 1).map(pageNum => (
          <button
            key={pageNum}
            onClick={() => setPage(pageNum)}
            style={{
              fontWeight: pageNum === page ? 'bold' : 'normal',
              textDecoration: pageNum === page ? 'underline' : 'none',
            }}
          >
            {pageNum}
          </button>
        ))}

        {/* Next button */}
        <button
          onClick={() => setPage(p => p + 1)}
          disabled={page === totalPages}
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Server-side pagination (query params to API) not client-side
(slicing a big array). They also check that page is in state
so `useEffect` re-fetches when it changes, and that Previous/Next
are disabled at the boundaries.

**🔷 TypeScript lesson:**
`PaginatedResponse<T>` is a generic interface — the `T` is the
item type. This lets you reuse the same interface for
`PaginatedResponse<Employee>`, `PaginatedResponse<Product>`,
`PaginatedResponse<Order>` across your app.

</details>

---

### 🔴 Pattern 7 · Scenario 2 — The Pagination That Loses Filters

🏢 **Situation:**
You are at a job portal. The job listings page has pagination
and a search filter. Users search for "React developer", see
page 1 results, then click page 2. Suddenly the search filter
is gone and they see all jobs again. Your PM wants this fixed
before the product demo tomorrow.

🔷 **Types for this scenario:**
```typescript
interface Job {
  id: string;
  title: string;
  company: string;
  location: string;
  salary: number;
  postedAt: string;
}

interface JobFilters {
  search: string;
  location: string;
  minSalary: number;
}

interface JobsQueryParams extends JobFilters {
  page: number;
  pageSize: number;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

function JobListings(): JSX.Element {
  const [jobs, setJobs] = useState<Job[]>([]);
  const [total, setTotal] = useState<number>(0);
  const [page, setPage] = useState<number>(1);
  const [search, setSearch] = useState<string>('');
  const [totalPages, setTotalPages] = useState<number>(1);

  // BUG: only sends page, ignores search filter
  useEffect(() => {
    const load = async (): Promise<void> => {
      const res = await fetch(`/api/jobs?page=${page}&pageSize=10`);
      const data = await res.json() as { items: Job[]; total: number; totalPages: number };
      setJobs(data.items);
      setTotal(data.total);
      setTotalPages(data.totalPages);
    };
    load();
  }, [page]);

  // BUG: changing search does not reset page or re-fetch
  const handleSearch = (e: React.ChangeEvent<HTMLInputElement>): void => {
    setSearch(e.target.value);
  };

  return (
    <div>
      <input
        value={search}
        onChange={handleSearch}
        placeholder="Search jobs..."
      />
      <p>{total} jobs found</p>
      {jobs.map(job => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          <p>{job.company}</p>
        </div>
      ))}
      <button onClick={() => setPage(p => p - 1)} disabled={page === 1}>
        Prev
      </button>
      <span>Page {page} of {totalPages}</span>
      <button onClick={() => setPage(p => p + 1)} disabled={page === totalPages}>
        Next
      </button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the fetch to include all active filters as query params
- ✅ Reset page to 1 whenever search changes
- ✅ Debounce the search so it does not fetch on every keystroke
- ✅ Use `URLSearchParams` to build the query string cleanly

<details>
<summary>💡 Hint</summary>

The useEffect dependency array is the problem — it only
re-runs when `page` changes, ignoring `search`. Add `search`
to the deps array. Also: whenever search changes, reset page
to 1, otherwise you might be on page 5 of a search that only
has 2 pages. Use `URLSearchParams` to build query strings
cleanly instead of string concatenation.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useCallback } from 'react';

interface Job {
  id: string;
  title: string;
  company: string;
  location: string;
  salary: number;
  postedAt: string;
}

interface PaginatedResponse<T> {
  items: T[];
  total: number;
  totalPages: number;
}

const PAGE_SIZE = 10;
const DEBOUNCE_MS = 400;

function JobListings(): JSX.Element {
  const [jobs, setJobs] = useState<Job[]>([]);
  const [total, setTotal] = useState<number>(0);
  const [totalPages, setTotalPages] = useState<number>(1);
  const [page, setPage] = useState<number>(1);
  const [searchInput, setSearchInput] = useState<string>('');
  const [debouncedSearch, setDebouncedSearch] = useState<string>('');
  const [isLoading, setIsLoading] = useState<boolean>(false);
  const [error, setError] = useState<string | null>(null);

  // Debounce search input
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedSearch(searchInput);
      setPage(1); // reset to page 1 when search changes
    }, DEBOUNCE_MS);

    return () => clearTimeout(timer);
  }, [searchInput]);

  // FIX: include debouncedSearch in deps AND in the query
  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        setIsLoading(true);
        setError(null);

        // Use URLSearchParams — clean, handles encoding automatically
        const params = new URLSearchParams({
          page: String(page),
          pageSize: String(PAGE_SIZE),
          ...(debouncedSearch && { search: debouncedSearch }),
        });

        const res = await fetch(`/api/jobs?${params.toString()}`);
        if (!res.ok) throw new Error('Failed to load jobs');

        const data = await res.json() as PaginatedResponse<Job>;
        setJobs(data.items);
        setTotal(data.total);
        setTotalPages(data.totalPages);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setIsLoading(false);
      }
    };

    load();
  }, [page, debouncedSearch]); // both page AND search are deps

  const handleSearchChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    setSearchInput(e.target.value);
    // Page reset happens in the debounce useEffect
  };

  return (
    <div>
      <input
        value={searchInput}
        onChange={handleSearchChange}
        placeholder="Search jobs..."
      />

      {isLoading ? (
        <p>Searching...</p>
      ) : (
        <p>
          {total} jobs found
          {debouncedSearch && ` for "${debouncedSearch}"`}
        </p>
      )}

      {error && <p style={{ color: 'red' }}>{error}</p>}

      {jobs.map(job => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          <p>{job.company} — {job.location}</p>
          <p>₹{job.salary.toLocaleString()}/year</p>
        </div>
      ))}

      {/* Pagination */}
      <div style={{ display: 'flex', gap: 8, marginTop: 16 }}>
        <button
          onClick={() => setPage(p => p - 1)}
          disabled={page === 1 || isLoading}
        >
          Previous
        </button>
        <span>Page {page} of {totalPages}</span>
        <button
          onClick={() => setPage(p => p + 1)}
          disabled={page === totalPages || isLoading}
        >
          Next
        </button>
      </div>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Both `page` AND `debouncedSearch` in the useEffect deps, and
`setPage(1)` in the debounce effect when search changes.
They also look for `URLSearchParams` over string concatenation —
it handles special characters and encoding automatically.

**🔷 TypeScript lesson:**
`URLSearchParams` constructor accepts `Record<string, string>`.
The spread with conditional `...(condition && { key: value })`
is a clean TypeScript pattern for optional query params.

</details>

---

### 🔴 Pattern 7 · Scenario 3 — The Load More Button That Duplicates Data

🏢 **Situation:**
You are building a social platform. The post feed has a
"Load More" button. It loads new posts. But users are seeing
duplicate posts — the same post appears twice or three times.
Your backend engineer says the API is correct. The bug is in
the frontend. Find it and fix it.

🔷 **Types for this scenario:**
```typescript
interface Post {
  id: string;
  authorName: string;
  content: string;
  likes: number;
  createdAt: string;
}

interface LoadMoreState {
  posts: Post[];
  page: number;
  hasMore: boolean;
  isLoadingMore: boolean;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

function PostFeed(): JSX.Element {
  const [posts, setPosts] = useState<Post[]>([]);
  const [page, setPage] = useState<number>(1);
  const [hasMore, setHasMore] = useState<boolean>(true);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  // BUG: runs on every page change including initial load
  // AND when Load More is clicked — causes double fetch
  useEffect(() => {
    const load = async (): Promise<void> => {
      setIsLoading(true);
      const res = await fetch(`/api/posts?page=${page}&pageSize=10`);
      const data = await res.json() as { posts: Post[]; hasMore: boolean };

      // BUG: replaces posts instead of appending on load more
      setPosts(data.posts);
      setHasMore(data.hasMore);
      setIsLoading(false);
    };
    load();
  }, [page]);

  const handleLoadMore = (): void => {
    setPage(p => p + 1); // this triggers useEffect above
  };

  return (
    <div>
      {posts.map(post => (
        <div key={post.id}>
          <strong>{post.authorName}</strong>
          <p>{post.content}</p>
        </div>
      ))}
      {hasMore && (
        <button onClick={handleLoadMore} disabled={isLoading}>
          {isLoading ? 'Loading...' : 'Load More'}
        </button>
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the duplication bug — append posts instead of replacing
- ✅ Load initial posts on mount without needing a button click
- ✅ Prevent the Load More button from firing while already loading
- ✅ Show "No more posts" when all posts are loaded

<details>
<summary>💡 Hint</summary>

The core bug is `setPosts(data.posts)` — this replaces all
posts every time, including when loading page 2 and 3.
For Load More, you need `setPosts(prev => [...prev, ...data.posts])`.
But there is a second problem: `useEffect` runs on mount with
`page = 1`, so the initial load is fine. The issue is only
when page changes — make sure you are appending, not replacing.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef } from 'react';

interface Post {
  id: string;
  authorName: string;
  content: string;
  likes: number;
  createdAt: string;
}

interface PostsApiResponse {
  posts: Post[];
  hasMore: boolean;
  total: number;
}

const PAGE_SIZE = 10;

function PostFeed(): JSX.Element {
  const [posts, setPosts] = useState<Post[]>([]);
  const [page, setPage] = useState<number>(1);
  const [hasMore, setHasMore] = useState<boolean>(true);
  const [isLoadingMore, setIsLoadingMore] = useState<boolean>(false);
  const [isInitialLoading, setIsInitialLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  // Track if this is the initial load or a load-more
  const isFirstLoad = useRef<boolean>(true);

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        // Different loading state for first load vs load more
        if (isFirstLoad.current) {
          setIsInitialLoading(true);
        } else {
          setIsLoadingMore(true);
        }
        setError(null);

        const res = await fetch(
          `/api/posts?page=${page}&pageSize=${PAGE_SIZE}`
        );
        if (!res.ok) throw new Error('Failed to load posts');

        const data = await res.json() as PostsApiResponse;

        if (isFirstLoad.current) {
          // First load — set posts directly
          setPosts(data.posts);
          isFirstLoad.current = false;
        } else {
          // Load more — APPEND to existing posts
          setPosts(prev => [...prev, ...data.posts]);
        }

        setHasMore(data.hasMore);

      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to load');
      } finally {
        setIsInitialLoading(false);
        setIsLoadingMore(false);
      }
    };

    load();
  }, [page]);

  const handleLoadMore = (): void => {
    if (!isLoadingMore && hasMore) {
      setPage(p => p + 1);
    }
  };

  if (isInitialLoading) {
    return <p>Loading posts...</p>;
  }

  if (error && posts.length === 0) {
    return <p>Error: {error}</p>;
  }

  return (
    <div>
      {posts.map(post => (
        <div key={post.id} style={{ padding: 16, borderBottom: '1px solid #eee' }}>
          <strong>{post.authorName}</strong>
          <p>{post.content}</p>
          <small>❤️ {post.likes} · {new Date(post.createdAt).toLocaleDateString()}</small>
        </div>
      ))}

      {error && <p style={{ color: 'red' }}>Error loading more posts</p>}

      {hasMore ? (
        <button
          onClick={handleLoadMore}
          disabled={isLoadingMore}
          style={{ width: '100%', padding: 12, marginTop: 16 }}
        >
          {isLoadingMore ? 'Loading...' : 'Load More'}
        </button>
      ) : (
        <p style={{ textAlign: 'center', color: '#999', marginTop: 16 }}>
          You have seen all posts
        </p>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`setPosts(prev => [...prev, ...data.posts])` for append vs
`setPosts(data.posts)` for replace — this is the exact line
they look for. They also check for the "no more posts" message
and the disabled button during loading.

**🔷 TypeScript lesson:**
`useRef<boolean>(true)` for tracking mutable values that should
not trigger re-renders. The `ref` holds a value across renders
without causing them — unlike state. Using `isFirstLoad.current`
distinguishes the first render from subsequent ones.

</details>

---

### 🔴 Pattern 7 · Scenario 4 — The Infinite Scroll That Never Stops

🏢 **Situation:**
You are at an e-commerce company. The product feed uses
infinite scroll — new products load as you scroll down.
Two bugs are reported: (1) it keeps firing the fetch even
after all products are loaded, and (2) it fires multiple
times when you scroll past the trigger point quickly.
Both are burning unnecessary API calls.

🔷 **Types for this scenario:**
```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
  category: string;
}

interface InfiniteScrollState {
  items: Product[];
  page: number;
  hasMore: boolean;
  isLoading: boolean;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

function ProductFeed(): JSX.Element {
  const [products, setProducts] = useState<Product[]>([]);
  const [page, setPage] = useState<number>(1);
  const [hasMore, setHasMore] = useState<boolean>(true);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  useEffect(() => {
    const load = async (): Promise<void> => {
      setIsLoading(true);
      const res = await fetch(`/api/products?page=${page}`);
      const data = await res.json() as { products: Product[]; hasMore: boolean };
      setProducts(prev => [...prev, ...data.products]);
      setHasMore(data.hasMore);
      setIsLoading(false);
    };
    load();
  }, [page]);

  // BUG: fires multiple times on fast scroll
  // BUG: fires even when hasMore is false
  useEffect(() => {
    const handleScroll = (): void => {
      const nearBottom =
        window.innerHeight + window.scrollY >= document.body.offsetHeight - 100;
      if (nearBottom) {
        setPage(p => p + 1);
      }
    };
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <div>
      {products.map(product => (
        <div key={product.id}>
          <img src={product.image} alt={product.name} />
          <p>{product.name}</p>
          <p>₹{product.price}</p>
        </div>
      ))}
      {isLoading && <p>Loading more products...</p>}
    </div>
  );
}
```

**Your tasks:**
- ✅ Stop fetching when `hasMore` is false
- ✅ Prevent multiple simultaneous fetches with `isLoading` guard
- ✅ Use `IntersectionObserver` instead of scroll event for better performance
- ✅ Add a sentinel div at the bottom that triggers the load

<details>
<summary>💡 Hint</summary>

Replace the `scroll` event listener with `IntersectionObserver`.
Attach it to a sentinel `div` at the bottom of the list.
When the sentinel enters the viewport, load the next page.
Guard the load with `if (!isLoading && hasMore)` to prevent
duplicate fetches. Use `useRef` for both the sentinel element
and the observer instance.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef, useCallback } from 'react';

interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
  category: string;
}

interface ProductsApiResponse {
  products: Product[];
  hasMore: boolean;
}

function ProductFeed(): JSX.Element {
  const [products, setProducts] = useState<Product[]>([]);
  const [page, setPage] = useState<number>(1);
  const [hasMore, setHasMore] = useState<boolean>(true);
  const [isLoading, setIsLoading] = useState<boolean>(false);
  const [error, setError] = useState<string | null>(null);

  // Ref for the sentinel div at the bottom of the list
  const sentinelRef = useRef<HTMLDivElement>(null);

  const loadProducts = useCallback(async (pageNum: number): Promise<void> => {
    try {
      setIsLoading(true);
      setError(null);

      const res = await fetch(`/api/products?page=${pageNum}&pageSize=12`);
      if (!res.ok) throw new Error('Failed to load products');

      const data = await res.json() as ProductsApiResponse;

      setProducts(prev => [...prev, ...data.products]);
      setHasMore(data.hasMore);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to load');
    } finally {
      setIsLoading(false);
    }
  }, []);

  // Initial load
  useEffect(() => {
    loadProducts(1);
  }, [loadProducts]);

  // IntersectionObserver — fires when sentinel enters viewport
  useEffect(() => {
    const sentinel = sentinelRef.current;
    if (!sentinel) return;

    const observer = new IntersectionObserver(
      (entries: IntersectionObserverEntry[]) => {
        const first = entries[0];
        // Only load if sentinel is visible AND not already loading AND has more
        if (first.isIntersecting && !isLoading && hasMore) {
          setPage(prev => {
            const nextPage = prev + 1;
            loadProducts(nextPage);
            return nextPage;
          });
        }
      },
      {
        threshold: 0.1, // fire when 10% of sentinel is visible
        rootMargin: '100px', // start loading 100px before reaching bottom
      }
    );

    observer.observe(sentinel);

    // Cleanup observer on unmount or when deps change
    return () => observer.disconnect();
  }, [isLoading, hasMore, loadProducts]);

  return (
    <div>
      <div
        style={{
          display: 'grid',
          gridTemplateColumns: 'repeat(auto-fill, minmax(200px, 1fr))',
          gap: 16,
        }}
      >
        {products.map(product => (
          <div key={product.id} style={{ border: '1px solid #eee', padding: 12 }}>
            <img
              src={product.image}
              alt={product.name}
              style={{ width: '100%', height: 150, objectFit: 'cover' }}
            />
            <p>{product.name}</p>
            <p style={{ fontWeight: 'bold' }}>₹{product.price.toLocaleString()}</p>
          </div>
        ))}
      </div>

      {/* Loading indicator */}
      {isLoading && (
        <p style={{ textAlign: 'center', padding: 16 }}>Loading more products...</p>
      )}

      {/* Error state */}
      {error && (
        <p style={{ color: 'red', textAlign: 'center' }}>
          {error}
          <button onClick={() => loadProducts(page)}>Retry</button>
        </p>
      )}

      {/* End of list message */}
      {!hasMore && !isLoading && (
        <p style={{ textAlign: 'center', color: '#999', padding: 16 }}>
          You have seen all {products.length} products
        </p>
      )}

      {/* Sentinel div — IntersectionObserver watches this */}
      <div ref={sentinelRef} style={{ height: 1 }} aria-hidden="true" />
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`IntersectionObserver` over scroll events — this is the modern
pattern they want to see. The `!isLoading && hasMore` guard
prevents duplicate fetches. They also look for the sentinel
div pattern and proper observer cleanup.

**🔷 TypeScript lesson:**
`useRef<HTMLDivElement>(null)` types the ref to a specific DOM
element type. `IntersectionObserverEntry[]` types the callback
parameter. TypeScript forces you to handle the `null` case
before calling `observer.observe(sentinel)`.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 8: 🔔 Toast / Notifications

> **Level 2 — Intermediate**

🎯 **What you will build in this pattern:**
- A toast notification system that shows success/error/info messages
- A global notification context that any component can use
- Auto-dismissing toasts with a progress timer

📋 **Interview asks this as:**
- "Show a success message after the user submits a form"
- "Build a toast notification system that works across the whole app"
- "Create notifications that auto-dismiss after 3 seconds"

🔷 **TypeScript concepts used in this pattern:**
- Discriminated union `Toast` type with `'success' | 'error' | 'warning' | 'info'`
- Generic context with typed `useToast` hook
- `useReducer` with typed `ToastAction` union

🚨 **Most common mistake in interviews:**
Building a toast that only works inside one component instead of
globally. Real apps need toasts from any component — a failed
API call deep in the tree should be able to show a toast.
Always use Context for toast systems.

---

### 🔴 Pattern 8 · Scenario 1 — The Action With No Feedback

🏢 **Situation:**
You are at a SaaS company. Users save their profile settings
by clicking a button. The API call happens — but nothing
appears on screen. No success message, no error message, nothing.
Users click the button 3-4 times thinking it did not work.
Your PM asks: "Add a toast notification — green for success,
red for error."

🔷 **Types for this scenario:**
```typescript
type ToastType = 'success' | 'error' | 'warning' | 'info';

interface Toast {
  id: string;
  type: ToastType;
  message: string;
  duration?: number;
}
```

**Here is the current code:**

```tsx
import { useState } from 'react';

interface ProfileData {
  name: string;
  email: string;
  bio: string;
}

interface ProfileFormProps {
  userId: string;
  initialData: ProfileData;
}

function ProfileForm({ userId, initialData }: ProfileFormProps): JSX.Element {
  const [formData, setFormData] = useState<ProfileData>(initialData);
  const [isSubmitting, setIsSubmitting] = useState<boolean>(false);

  const handleChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();
    setIsSubmitting(true);
    await fetch(`/api/users/${userId}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData),
    });
    setIsSubmitting(false);
    // No feedback to user at all
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={formData.name} onChange={handleChange} />
      <input name="email" value={formData.email} onChange={handleChange} />
      <textarea name="bio" value={formData.bio} onChange={handleChange} />
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Saving...' : 'Save Profile'}
      </button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Add a local toast state that shows a message after save
- ✅ Show green toast on success, red toast on error
- ✅ Auto-dismiss the toast after 3 seconds
- ✅ Clean up the timer on unmount

<details>
<summary>💡 Hint</summary>

For a local toast (just in this component), you only need
a `useState<Toast | null>(null)`. Set it after the API call.
Use `useEffect` to start a `setTimeout` when the toast appears,
and clear it when the toast is dismissed or the component unmounts.
The cleanup in useEffect's return function is critical here.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

type ToastType = 'success' | 'error';

interface Toast {
  type: ToastType;
  message: string;
}

interface ProfileData {
  name: string;
  email: string;
  bio: string;
}

interface ProfileFormProps {
  userId: string;
  initialData: ProfileData;
}

const TOAST_COLORS: Record<ToastType, string> = {
  success: '#22c55e',
  error: '#ef4444',
};

const TOAST_DURATION_MS = 3000;

function ProfileForm({ userId, initialData }: ProfileFormProps): JSX.Element {
  const [formData, setFormData] = useState<ProfileData>(initialData);
  const [isSubmitting, setIsSubmitting] = useState<boolean>(false);
  const [toast, setToast] = useState<Toast | null>(null);

  // Auto-dismiss toast after 3 seconds
  useEffect(() => {
    if (!toast) return;

    const timer = setTimeout(() => {
      setToast(null);
    }, TOAST_DURATION_MS);

    // Cleanup — if component unmounts or toast changes, clear timer
    return () => clearTimeout(timer);
  }, [toast]);

  const showToast = (type: ToastType, message: string): void => {
    setToast({ type, message });
  };

  const handleChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (
    e: React.FormEvent<HTMLFormElement>
  ): Promise<void> => {
    e.preventDefault();
    try {
      setIsSubmitting(true);

      const res = await fetch(`/api/users/${userId}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) throw new Error('Failed to save profile');

      showToast('success', 'Profile saved successfully!');
    } catch (err) {
      showToast('error', err instanceof Error ? err.message : 'Save failed');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <div style={{ position: 'relative' }}>
      {/* Toast notification */}
      {toast && (
        <div
          style={{
            position: 'fixed',
            top: 24,
            right: 24,
            padding: '12px 20px',
            borderRadius: 8,
            background: TOAST_COLORS[toast.type],
            color: 'white',
            fontWeight: 500,
            zIndex: 9999,
            boxShadow: '0 4px 12px rgba(0,0,0,0.15)',
            display: 'flex',
            alignItems: 'center',
            gap: 8,
          }}
        >
          {toast.type === 'success' ? '✓' : '✗'} {toast.message}
          <button
            onClick={() => setToast(null)}
            style={{ background: 'none', border: 'none', color: 'white', cursor: 'pointer' }}
          >
            ✕
          </button>
        </div>
      )}

      <form onSubmit={handleSubmit}>
        <input
          name="name"
          value={formData.name}
          onChange={handleChange}
          placeholder="Full Name"
        />
        <input
          name="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        <textarea
          name="bio"
          value={formData.bio}
          onChange={handleChange}
          placeholder="Bio"
        />
        <button type="submit" disabled={isSubmitting}>
          {isSubmitting ? 'Saving...' : 'Save Profile'}
        </button>
      </form>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `useEffect` cleanup for `setTimeout` is what they look for
first — without it, the timer fires after unmount and calls
`setToast` on an unmounted component. They also check for
`err instanceof Error` narrowing instead of `any`.

**🔷 TypeScript lesson:**
`Record<ToastType, string>` maps each union member to a value.
This is safer than a plain object because TypeScript ensures
you have a value for every possible `ToastType` — if you add
a new type to the union, TypeScript flags the missing entry.

</details>

---

### 🔴 Pattern 8 · Scenario 2 — The Toast That Stacks Forever

🏢 **Situation:**
You are at a food delivery app. The notification system shows
toasts when orders update. During a busy period, 20 order
updates come in quickly. The screen fills up with 20 stacked
toast messages and the UI is completely unusable. Your design
team says: "Maximum 3 toasts at once, oldest ones get removed."

🔷 **Types for this scenario:**
```typescript
type ToastType = 'success' | 'error' | 'warning' | 'info';

interface Toast {
  id: string;
  type: ToastType;
  message: string;
  createdAt: number;
}

type ToastAction =
  | { type: 'ADD'; payload: Omit<Toast, 'id' | 'createdAt'> }
  | { type: 'REMOVE'; payload: string };
```

**Here is the current code:**

```tsx
import { useState } from 'react';

function useToasts() {
  const [toasts, setToasts] = useState<Toast[]>([]);

  const addToast = (type: ToastType, message: string): void => {
    const newToast: Toast = {
      id: Math.random().toString(),
      type,
      message,
      createdAt: Date.now(),
    };
    // BUG: no limit — can add infinite toasts
    setToasts(prev => [...prev, newToast]);
  };

  const removeToast = (id: string): void => {
    setToasts(prev => prev.filter(t => t.id !== id));
  };

  return { toasts, addToast, removeToast };
}
```

**Your tasks:**
- ✅ Limit visible toasts to a maximum of 3
- ✅ When a 4th toast is added, remove the oldest one
- ✅ Use `useReducer` with typed actions instead of `useState`
- ✅ Auto-remove each toast after 4 seconds using the toast's own timer

<details>
<summary>💡 Hint</summary>

Switch from `useState` to `useReducer` — the ADD action
should add the new toast AND slice to keep only the last 3.
`[...prev, newToast].slice(-MAX_TOASTS)` keeps the most recent.
For auto-remove: in the toast component itself, use `useEffect`
with `setTimeout` and call `removeToast(id)` after 4 seconds.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useReducer, useEffect, useCallback } from 'react';

type ToastType = 'success' | 'error' | 'warning' | 'info';

interface Toast {
  id: string;
  type: ToastType;
  message: string;
  createdAt: number;
}

// Typed discriminated union for reducer actions
type ToastAction =
  | { type: 'ADD'; payload: Omit<Toast, 'id' | 'createdAt'> }
  | { type: 'REMOVE'; payload: string };

const MAX_TOASTS = 3;
const TOAST_DURATION_MS = 4000;

const TOAST_STYLES: Record<ToastType, { bg: string; icon: string }> = {
  success: { bg: '#22c55e', icon: '✓' },
  error: { bg: '#ef4444', icon: '✗' },
  warning: { bg: '#f59e0b', icon: '⚠' },
  info: { bg: '#3b82f6', icon: 'ℹ' },
};

// Pure reducer — easy to test
function toastReducer(state: Toast[], action: ToastAction): Toast[] {
  switch (action.type) {
    case 'ADD': {
      const newToast: Toast = {
        id: `toast-${Date.now()}-${Math.random()}`,
        createdAt: Date.now(),
        ...action.payload,
      };
      // Add new toast AND enforce max limit — oldest removed first
      return [...state, newToast].slice(-MAX_TOASTS);
    }
    case 'REMOVE':
      return state.filter(t => t.id !== action.payload);
    default:
      return state;
  }
}

// Individual Toast item — handles its own auto-dismiss
interface ToastItemProps {
  toast: Toast;
  onRemove: (id: string) => void;
}

function ToastItem({ toast, onRemove }: ToastItemProps): JSX.Element {
  // Each toast manages its own auto-dismiss timer
  useEffect(() => {
    const timer = setTimeout(() => {
      onRemove(toast.id);
    }, TOAST_DURATION_MS);

    return () => clearTimeout(timer);
  }, [toast.id, onRemove]);

  const style = TOAST_STYLES[toast.type];

  return (
    <div
      style={{
        padding: '12px 16px',
        borderRadius: 8,
        background: style.bg,
        color: 'white',
        display: 'flex',
        alignItems: 'center',
        gap: 8,
        boxShadow: '0 2px 8px rgba(0,0,0,0.2)',
      }}
    >
      <span>{style.icon}</span>
      <span style={{ flex: 1 }}>{toast.message}</span>
      <button
        onClick={() => onRemove(toast.id)}
        style={{ background: 'none', border: 'none', color: 'white', cursor: 'pointer' }}
        aria-label="Dismiss notification"
      >
        ✕
      </button>
    </div>
  );
}

// Toast container — renders all active toasts
interface ToastContainerProps {
  toasts: Toast[];
  onRemove: (id: string) => void;
}

function ToastContainer({ toasts, onRemove }: ToastContainerProps): JSX.Element {
  return (
    <div
      style={{
        position: 'fixed',
        top: 24,
        right: 24,
        display: 'flex',
        flexDirection: 'column',
        gap: 8,
        zIndex: 9999,
        minWidth: 300,
        maxWidth: 420,
      }}
    >
      {toasts.map(toast => (
        <ToastItem key={toast.id} toast={toast} onRemove={onRemove} />
      ))}
    </div>
  );
}

// Custom hook — the public API for the toast system
interface UseToastsReturn {
  toasts: Toast[];
  addToast: (type: ToastType, message: string) => void;
  removeToast: (id: string) => void;
}

function useToasts(): UseToastsReturn {
  const [toasts, dispatch] = useReducer(toastReducer, []);

  const addToast = useCallback((type: ToastType, message: string): void => {
    dispatch({ type: 'ADD', payload: { type, message } });
  }, []);

  const removeToast = useCallback((id: string): void => {
    dispatch({ type: 'REMOVE', payload: id });
  }, []);

  return { toasts, addToast, removeToast };
}

// Example usage
function OrdersPage(): JSX.Element {
  const { toasts, addToast, removeToast } = useToasts();

  return (
    <div>
      <ToastContainer toasts={toasts} onRemove={removeToast} />
      <button onClick={() => addToast('success', 'Order #1234 confirmed!')}>
        Confirm Order
      </button>
      <button onClick={() => addToast('error', 'Payment failed. Try again.')}>
        Fail Payment
      </button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`.slice(-MAX_TOASTS)` in the reducer to enforce the limit,
and `useReducer` with a typed discriminated union action.
They also look for splitting toast rendering into its own
`ToastItem` component so each toast can manage its own timer.

**🔷 TypeScript lesson:**
Discriminated union `ToastAction` with `type: 'ADD' | 'REMOVE'`
is the standard typed reducer pattern. TypeScript narrows the
action type inside each `case` — in `case 'ADD'` you get full
type safety on `action.payload`.

</details>

---

### 🔴 Pattern 8 · Scenario 3 — The Notification System With No Context

🏢 **Situation:**
You are at a banking app. Toast notifications work fine on
the main page. But deep inside a transaction form, when a
transfer fails, there is no way to show a toast — the toast
state lives in the parent component and is not accessible
from the form. Your tech lead says: "Make toasts available
everywhere with Context."

🔷 **Types for this scenario:**
```typescript
type ToastType = 'success' | 'error' | 'warning' | 'info';

interface Toast {
  id: string;
  type: ToastType;
  message: string;
}

interface ToastContextType {
  addToast: (type: ToastType, message: string) => void;
  removeToast: (id: string) => void;
}
```

**Here is the current code:**

```tsx
// Toast state lives only in App — cannot be used by deep components
function App(): JSX.Element {
  const [toasts, setToasts] = useState<Toast[]>([]);

  const addToast = (type: ToastType, message: string): void => {
    setToasts(prev => [...prev, {
      id: Date.now().toString(),
      type,
      message,
    }]);
  };

  return (
    <div>
      {/* Must manually pass addToast as a prop — prop drilling */}
      <TransferPage addToast={addToast} />
      {/* Toast container only works here */}
    </div>
  );
}

// Deep component — cannot show toast without prop drilling
interface TransferFormProps {
  addToast: (type: ToastType, message: string) => void;
}

function TransferForm({ addToast }: TransferFormProps): JSX.Element {
  const handleTransfer = async (): Promise<void> => {
    // Has to receive addToast as prop — messy
    addToast('success', 'Transfer complete');
  };
  return <button onClick={handleTransfer}>Transfer</button>;
}
```

**Your tasks:**
- ✅ Move toast state into a `ToastProvider` Context component
- ✅ Create a typed `useToast()` hook that throws if used outside provider
- ✅ Remove all prop drilling of `addToast`
- ✅ Any component anywhere can call `useToast()` to show a message

<details>
<summary>💡 Hint</summary>

Create a `ToastContext` with `createContext<ToastContextType | null>(null)`.
Wrap your app with `ToastProvider`. Inside `useToast()`, check
if the context is null and throw a helpful error if so —
this catches the mistake of using the hook outside the provider.
The toast array itself stays in the provider's state.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import {
  createContext, useContext, useState, useCallback,
  useEffect, type ReactNode
} from 'react';

type ToastType = 'success' | 'error' | 'warning' | 'info';

interface Toast {
  id: string;
  type: ToastType;
  message: string;
}

// Context only exposes the API — not the raw state
interface ToastContextType {
  addToast: (type: ToastType, message: string) => void;
  removeToast: (id: string) => void;
}

// Create context — null means "not inside a provider"
const ToastContext = createContext<ToastContextType | null>(null);

// Provider — owns the toast state
interface ToastProviderProps {
  children: ReactNode;
}

function ToastProvider({ children }: ToastProviderProps): JSX.Element {
  const [toasts, setToasts] = useState<Toast[]>([]);

  const addToast = useCallback((type: ToastType, message: string): void => {
    const id = `toast-${Date.now()}`;
    setToasts(prev => [...prev, { id, type, message }].slice(-3));
  }, []);

  const removeToast = useCallback((id: string): void => {
    setToasts(prev => prev.filter(t => t.id !== id));
  }, []);

  return (
    <ToastContext.Provider value={{ addToast, removeToast }}>
      {children}
      {/* Toast container lives inside provider — always rendered */}
      <div style={{
        position: 'fixed', top: 24, right: 24,
        display: 'flex', flexDirection: 'column', gap: 8,
        zIndex: 9999,
      }}>
        {toasts.map(toast => (
          <ToastItem key={toast.id} toast={toast} onRemove={removeToast} />
        ))}
      </div>
    </ToastContext.Provider>
  );
}

// Typed hook — throws helpful error if used outside provider
function useToast(): ToastContextType {
  const context = useContext(ToastContext);
  if (!context) {
    throw new Error(
      'useToast must be used inside <ToastProvider>. ' +
      'Wrap your app with <ToastProvider> in main.tsx'
    );
  }
  return context;
}

// Individual toast item
interface ToastItemProps {
  toast: Toast;
  onRemove: (id: string) => void;
}

function ToastItem({ toast, onRemove }: ToastItemProps): JSX.Element {
  useEffect(() => {
    const timer = setTimeout(() => onRemove(toast.id), 4000);
    return () => clearTimeout(timer);
  }, [toast.id, onRemove]);

  const colors: Record<ToastType, string> = {
    success: '#22c55e',
    error: '#ef4444',
    warning: '#f59e0b',
    info: '#3b82f6',
  };

  return (
    <div style={{
      padding: '12px 16px', borderRadius: 8,
      background: colors[toast.type], color: 'white',
      display: 'flex', gap: 8, alignItems: 'center',
    }}>
      <span style={{ flex: 1 }}>{toast.message}</span>
      <button
        onClick={() => onRemove(toast.id)}
        style={{ background: 'none', border: 'none', color: 'white', cursor: 'pointer' }}
      >
        ✕
      </button>
    </div>
  );
}

// App setup — wrap once
function App(): JSX.Element {
  return (
    <ToastProvider>
      <TransferPage />
    </ToastProvider>
  );
}

// Any component — uses hook directly, no prop drilling
function TransferForm(): JSX.Element {
  const { addToast } = useToast(); // clean — no props needed

  const handleTransfer = async (): Promise<void> => {
    try {
      const res = await fetch('/api/transfers', { method: 'POST' });
      if (!res.ok) throw new Error('Transfer failed');
      addToast('success', 'Transfer of ₹5,000 completed!');
    } catch (err) {
      addToast('error', err instanceof Error ? err.message : 'Transfer failed');
    }
  };

  return <button onClick={handleTransfer}>Transfer ₹5,000</button>;
}

function TransferPage(): JSX.Element {
  return (
    <div>
      <h1>Transfer Money</h1>
      <TransferForm /> {/* No addToast prop needed */}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`createContext<ToastContextType | null>(null)` and the null
check in `useToast()` with a helpful error message — this is
the correct typed context pattern. No prop drilling anywhere.

**🔷 TypeScript lesson:**
`createContext<Type | null>(null)` is the standard pattern.
The `null` is the default value when no provider is found.
The custom hook checks for `null` and throws — TypeScript
then knows the return type is `ToastContextType`, not
`ToastContextType | null`.

</details>

---

### 🔴 Pattern 8 · Scenario 4 — The Toast That Ignores Errors

🏢 **Situation:**
You are at a recruitment platform. The toast system shows
"Saved!" for every action regardless of what happened.
When the API returns a 500 error, it still shows the green
"Saved!" toast. Candidates are losing application data and
the toast is telling them everything is fine. Fix the toast
to correctly reflect success vs failure.

🔷 **Types for this scenario:**
```typescript
type ToastType = 'success' | 'error';

interface ApiError {
  message: string;
  code: string;
  statusCode: number;
}

type ApiResult<T> =
  | { success: true; data: T }
  | { success: false; error: ApiError };
```

**Here is the current code:**

```tsx
import { useState } from 'react';

interface Application {
  jobId: string;
  coverLetter: string;
  resumeUrl: string;
}

function ApplicationForm({ jobId }: { jobId: string }): JSX.Element {
  const [coverLetter, setCoverLetter] = useState<string>('');
  const [showToast, setShowToast] = useState<boolean>(false);

  const handleSubmit = async (
    e: React.FormEvent<HTMLFormElement>
  ): Promise<void> => {
    e.preventDefault();
    // BUG: no try/catch — errors are swallowed
    // BUG: always shows success toast regardless of result
    await fetch(`/api/jobs/${jobId}/apply`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ coverLetter }),
    });
    setShowToast(true); // always green, always "Saved!"
    setTimeout(() => setShowToast(false), 3000);
  };

  return (
    <form onSubmit={handleSubmit}>
      <textarea
        value={coverLetter}
        onChange={e => setCoverLetter(e.target.value)}
        placeholder="Cover letter..."
      />
      <button type="submit">Submit Application</button>
      {showToast && (
        <div style={{ background: 'green', color: 'white', padding: 8 }}>
          Saved!
        </div>
      )}
    </form>
  );
}
```

**Your tasks:**
- ✅ Distinguish success from error responses — check `res.ok`
- ✅ Show different toast messages and colors for success vs error
- ✅ Use the typed `ApiResult<T>` pattern for the response
- ✅ Handle both network errors and API errors differently

<details>
<summary>💡 Hint</summary>

There are two failure modes to handle: network errors
(fetch throws) and API errors (fetch succeeds but res.ok
is false). Wrap in try/catch for network errors, and check
`res.ok` before reading the response body for API errors.
Your toast type should be `'success' | 'error'` and the
message should come from the actual result.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

type ToastType = 'success' | 'error';

interface ToastState {
  type: ToastType;
  message: string;
}

interface ApplicationResponse {
  applicationId: string;
  status: string;
  message: string;
}

interface ApiErrorResponse {
  message: string;
  code: string;
}

function ApplicationForm({ jobId }: { jobId: string }): JSX.Element {
  const [coverLetter, setCoverLetter] = useState<string>('');
  const [isSubmitting, setIsSubmitting] = useState<boolean>(false);
  const [toast, setToast] = useState<ToastState | null>(null);
  const [isSubmitted, setIsSubmitted] = useState<boolean>(false);

  // Auto-dismiss toast
  useEffect(() => {
    if (!toast) return;
    const timer = setTimeout(() => setToast(null), 4000);
    return () => clearTimeout(timer);
  }, [toast]);

  const handleSubmit = async (
    e: React.FormEvent<HTMLFormElement>
  ): Promise<void> => {
    e.preventDefault();

    if (!coverLetter.trim()) {
      setToast({ type: 'error', message: 'Please write a cover letter' });
      return;
    }

    try {
      setIsSubmitting(true);

      const res = await fetch(`/api/jobs/${jobId}/apply`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ coverLetter }),
      });

      if (!res.ok) {
        // API returned an error — read the error message from body
        const errorData = await res.json() as ApiErrorResponse;

        // Specific messages for specific status codes
        if (res.status === 409) {
          setToast({
            type: 'error',
            message: 'You have already applied for this job',
          });
        } else if (res.status === 422) {
          setToast({
            type: 'error',
            message: errorData.message || 'Invalid application data',
          });
        } else {
          setToast({
            type: 'error',
            message: errorData.message || 'Application failed. Please try again.',
          });
        }
        return;
      }

      // Success — read confirmation from response
      const data = await res.json() as ApplicationResponse;
      setToast({
        type: 'success',
        message: `Application submitted! Reference: #${data.applicationId}`,
      });
      setIsSubmitted(true);

    } catch (err) {
      // Network error — different message from API error
      setToast({
        type: 'error',
        message: 'Network error. Please check your connection and try again.',
      });
    } finally {
      setIsSubmitting(false);
    }
  };

  const toastColors: Record<ToastType, string> = {
    success: '#22c55e',
    error: '#ef4444',
  };

  return (
    <div>
      {/* Toast */}
      {toast && (
        <div style={{
          padding: '12px 16px',
          borderRadius: 8,
          background: toastColors[toast.type],
          color: 'white',
          marginBottom: 16,
          display: 'flex',
          justifyContent: 'space-between',
          alignItems: 'center',
        }}>
          {toast.message}
          <button
            onClick={() => setToast(null)}
            style={{ background: 'none', border: 'none', color: 'white', cursor: 'pointer' }}
          >
            ✕
          </button>
        </div>
      )}

      {isSubmitted ? (
        <div>
          <p>Your application has been submitted successfully!</p>
        </div>
      ) : (
        <form onSubmit={handleSubmit}>
          <textarea
            value={coverLetter}
            onChange={e => setCoverLetter(e.target.value)}
            placeholder="Write your cover letter..."
            rows={8}
            disabled={isSubmitting}
          />
          <button type="submit" disabled={isSubmitting || !coverLetter.trim()}>
            {isSubmitting ? 'Submitting...' : 'Submit Application'}
          </button>
        </form>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Checking `res.ok` before reading the success response, and
handling 409/422/500 with different messages. They also check
that network errors (catch block) have a different message
from API errors (res.ok false).

**🔷 TypeScript lesson:**
Casting API responses with `as ApiErrorResponse` and
`as ApplicationResponse` gives you type safety when reading
`data.message`, `data.applicationId` etc. Without the cast,
TypeScript cannot tell you what properties exist on the parsed JSON.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 9: 🛒 Shopping Cart

> **Level 2 — Intermediate**

🎯 **What you will build in this pattern:**
- A cart with add, remove, update quantity, and clear operations
- A cart that persists across page refreshes using localStorage
- A cart with stock validation and price calculation

📋 **Interview asks this as:**
- "Build a shopping cart with add/remove functionality"
- "Add quantity controls to this cart"
- "Make the cart total update when quantities change"

🔷 **TypeScript concepts used in this pattern:**
- `useReducer` with typed `CartAction` discriminated union
- `Omit<Product, 'description'>` for cart item types
- Generic `localStorage` utility typed with `<T>`

🚨 **Most common mistake in interviews:**
Mutating state directly — `item.quantity++` instead of returning
a new array. Also forgetting to handle the case where the same
item is added twice — it should increase quantity, not add a
duplicate entry.

---

### 🔴 Pattern 9 · Scenario 1 — The Cart That Cannot Update Quantity

🏢 **Situation:**
You are at an e-commerce company. The cart has add and remove
buttons but no way to change quantity. Users have to click
"Add to Cart" 5 times to get 5 of the same item, creating
5 separate line items instead of one item with quantity 5.
Your PM wants proper quantity controls with + and - buttons.

🔷 **Types for this scenario:**
```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
  stock: number;
}

interface CartItem extends Product {
  quantity: number;
}

type CartAction =
  | { type: 'ADD_ITEM'; payload: Product }
  | { type: 'REMOVE_ITEM'; payload: string }
  | { type: 'UPDATE_QUANTITY'; payload: { id: string; quantity: number } }
  | { type: 'CLEAR_CART' };
```

**Here is the current code:**

```tsx
import { useState } from 'react';

function ShoppingCart(): JSX.Element {
  const [cartItems, setCartItems] = useState<CartItem[]>([]);

  const addItem = (product: Product): void => {
    // BUG: always adds new item even if already in cart
    setCartItems(prev => [...prev, { ...product, quantity: 1 }]);
  };

  const removeItem = (id: string): void => {
    setCartItems(prev => prev.filter(item => item.id !== id));
  };

  // No way to update quantity

  const total = cartItems.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  return (
    <div>
      {cartItems.map(item => (
        <div key={item.id}>
          <span>{item.name}</span>
          <span>x{item.quantity}</span>
          <span>₹{item.price * item.quantity}</span>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
      <p>Total: ₹{total}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add `+` and `-` quantity buttons for each cart item
- ✅ When adding an existing item, increase quantity instead of duplicating
- ✅ Remove item automatically when quantity reaches 0
- ✅ Refactor to `useReducer` with the typed `CartAction` union

<details>
<summary>💡 Hint</summary>

In the ADD_ITEM reducer case, check if the item already exists
with `state.find(item => item.id === payload.id)`. If yes,
map and increase quantity. If no, spread and add.
For UPDATE_QUANTITY, filter out items where the new quantity
would be 0 or less instead of showing negative quantities.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useReducer } from 'react';

interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
  stock: number;
}

interface CartItem extends Product {
  quantity: number;
}

type CartAction =
  | { type: 'ADD_ITEM'; payload: Product }
  | { type: 'REMOVE_ITEM'; payload: string }
  | { type: 'UPDATE_QUANTITY'; payload: { id: string; quantity: number } }
  | { type: 'CLEAR_CART' };

function cartReducer(state: CartItem[], action: CartAction): CartItem[] {
  switch (action.type) {
    case 'ADD_ITEM': {
      const existingItem = state.find(item => item.id === action.payload.id);
      if (existingItem) {
        // Item already in cart — increase quantity
        return state.map(item =>
          item.id === action.payload.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      // New item — add with quantity 1
      return [...state, { ...action.payload, quantity: 1 }];
    }

    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.payload);

    case 'UPDATE_QUANTITY': {
      const { id, quantity } = action.payload;
      // If quantity reaches 0, remove the item
      if (quantity <= 0) {
        return state.filter(item => item.id !== id);
      }
      return state.map(item =>
        item.id === id ? { ...item, quantity } : item
      );
    }

    case 'CLEAR_CART':
      return [];

    default:
      return state;
  }
}

function ShoppingCart(): JSX.Element {
  const [cartItems, dispatch] = useReducer(cartReducer, []);

  const total = cartItems.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  const itemCount = cartItems.reduce(
    (sum, item) => sum + item.quantity,
    0
  );

  if (cartItems.length === 0) {
    return <p>Your cart is empty</p>;
  }

  return (
    <div>
      <h2>Cart ({itemCount} items)</h2>

      {cartItems.map(item => (
        <div key={item.id} style={{
          display: 'flex', alignItems: 'center',
          gap: 12, padding: '12px 0',
          borderBottom: '1px solid #eee',
        }}>
          <img
            src={item.image}
            alt={item.name}
            style={{ width: 60, height: 60, objectFit: 'cover' }}
          />

          <div style={{ flex: 1 }}>
            <p style={{ margin: 0, fontWeight: 500 }}>{item.name}</p>
            <p style={{ margin: 0, color: '#666' }}>₹{item.price} each</p>
          </div>

          {/* Quantity controls */}
          <div style={{ display: 'flex', alignItems: 'center', gap: 8 }}>
            <button
              onClick={() => dispatch({
                type: 'UPDATE_QUANTITY',
                payload: { id: item.id, quantity: item.quantity - 1 },
              })}
              aria-label="Decrease quantity"
            >
              −
            </button>

            <span style={{ minWidth: 24, textAlign: 'center' }}>
              {item.quantity}
            </span>

            <button
              onClick={() => dispatch({
                type: 'UPDATE_QUANTITY',
                payload: { id: item.id, quantity: item.quantity + 1 },
              })}
              disabled={item.quantity >= item.stock}
              aria-label="Increase quantity"
            >
              +
            </button>
          </div>

          <p style={{ minWidth: 80, textAlign: 'right', fontWeight: 500 }}>
            ₹{(item.price * item.quantity).toLocaleString()}
          </p>

          <button
            onClick={() => dispatch({ type: 'REMOVE_ITEM', payload: item.id })}
            style={{ color: 'red' }}
          >
            Remove
          </button>
        </div>
      ))}

      <div style={{ marginTop: 16, textAlign: 'right' }}>
        <p style={{ fontSize: 20, fontWeight: 'bold' }}>
          Total: ₹{total.toLocaleString()}
        </p>
        <button onClick={() => dispatch({ type: 'CLEAR_CART' })}>
          Clear Cart
        </button>
        <button style={{ marginLeft: 8, background: 'blue', color: 'white' }}>
          Checkout
        </button>
      </div>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `ADD_ITEM` case that checks for existing items and increases
quantity instead of duplicating. They also look for removing
the item when quantity hits 0 in `UPDATE_QUANTITY`, and
`useReducer` with typed actions.

**🔷 TypeScript lesson:**
`CartAction` as a discriminated union means TypeScript narrows
`action.payload` inside each case. In `case 'UPDATE_QUANTITY'`,
TypeScript knows `payload` is `{ id: string; quantity: number }`.
In `case 'REMOVE_ITEM'`, it knows `payload` is `string`.

</details>

---

### 🔴 Pattern 9 · Scenario 2 — The Cart With Wrong Totals

🏢 **Situation:**
You are at an e-commerce startup. Users are complaining that
their order totals are wrong. A cart with 3 items costing
₹100, ₹200, and ₹300 shows a total of ₹600 — correct.
But after applying a 10% discount coupon, it still shows
₹600. And the "You save" amount is always ₹0. Fix the
cart calculations.

🔷 **Types for this scenario:**
```typescript
interface CartItem {
  id: string;
  name: string;
  price: number;
  quantity: number;
}

interface Coupon {
  code: string;
  discountPercent: number;
  minOrderAmount: number;
}

interface CartSummary {
  subtotal: number;
  discount: number;
  deliveryFee: number;
  total: number;
  itemCount: number;
}
```

**Here is the current code:**

```tsx
import { useState } from 'react';

function CartSummaryPanel({ items }: { items: CartItem[] }): JSX.Element {
  const [couponCode, setCouponCode] = useState<string>('');
  const [appliedCoupon, setAppliedCoupon] = useState<Coupon | null>(null);
  const [couponError, setCouponError] = useState<string>('');

  // BUG: subtotal not using quantity
  const subtotal = items.reduce((sum, item) => sum + item.price, 0);

  // BUG: discount always 0 because appliedCoupon check is wrong
  const discount = appliedCoupon
    ? subtotal * appliedCoupon.discountPercent
    : 0;

  // BUG: total not subtracting discount
  const total = subtotal;

  const applyCoupon = async (): Promise<void> => {
    const res = await fetch(`/api/coupons/${couponCode}`);
    const coupon = await res.json() as Coupon;
    setAppliedCoupon(coupon);
  };

  return (
    <div>
      <p>Subtotal: ₹{subtotal}</p>
      <p>You save: ₹{discount}</p>
      <p>Total: ₹{total}</p>
      <input value={couponCode} onChange={e => setCouponCode(e.target.value)} />
      <button onClick={applyCoupon}>Apply Coupon</button>
      {couponError && <p>{couponError}</p>}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix subtotal to multiply `price × quantity` for each item
- ✅ Fix discount to use `discountPercent / 100` (it is a percentage)
- ✅ Fix total to subtract the discount
- ✅ Add delivery fee logic (free above ₹500, else ₹50)
- ✅ Add coupon validation — check minimum order amount

<details>
<summary>💡 Hint</summary>

Three separate calculation bugs:
1. `item.price` should be `item.price * item.quantity`
2. `discountPercent` is a percentage like `10` meaning 10%,
   so the calculation should be `subtotal * (discountPercent / 100)`
3. `total = subtotal - discount` not just `subtotal`
Use `useMemo` to compute the summary object so it only
recalculates when `items` or `appliedCoupon` changes.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useMemo } from 'react';

interface CartItem {
  id: string;
  name: string;
  price: number;
  quantity: number;
}

interface Coupon {
  code: string;
  discountPercent: number;
  minOrderAmount: number;
}

interface CartSummary {
  subtotal: number;
  discount: number;
  deliveryFee: number;
  total: number;
  itemCount: number;
}

const FREE_DELIVERY_THRESHOLD = 500;
const DELIVERY_FEE = 50;

interface CartSummaryPanelProps {
  items: CartItem[];
}

function CartSummaryPanel({ items }: CartSummaryPanelProps): JSX.Element {
  const [couponCode, setCouponCode] = useState<string>('');
  const [appliedCoupon, setAppliedCoupon] = useState<Coupon | null>(null);
  const [couponError, setCouponError] = useState<string>('');
  const [isApplyingCoupon, setIsApplyingCoupon] = useState<boolean>(false);

  // Compute all cart values together in one place
  const summary = useMemo((): CartSummary => {
    // FIX 1: multiply price × quantity
    const subtotal = items.reduce(
      (sum, item) => sum + item.price * item.quantity,
      0
    );

    // FIX 2: divide by 100 — discountPercent is a percentage
    const discount = appliedCoupon
      ? subtotal * (appliedCoupon.discountPercent / 100)
      : 0;

    const subtotalAfterDiscount = subtotal - discount;

    // Free delivery above threshold
    const deliveryFee = subtotalAfterDiscount >= FREE_DELIVERY_THRESHOLD
      ? 0
      : DELIVERY_FEE;

    // FIX 3: total = subtotal - discount + delivery
    const total = subtotalAfterDiscount + deliveryFee;

    const itemCount = items.reduce((sum, item) => sum + item.quantity, 0);

    return { subtotal, discount, deliveryFee, total, itemCount };
  }, [items, appliedCoupon]);

  const applyCoupon = async (): Promise<void> => {
    if (!couponCode.trim()) return;

    try {
      setIsApplyingCoupon(true);
      setCouponError('');

      const res = await fetch(`/api/coupons/${couponCode}`);

      if (res.status === 404) {
        setCouponError('Invalid coupon code');
        return;
      }
      if (!res.ok) throw new Error('Failed to validate coupon');

      const coupon = await res.json() as Coupon;

      // Validate minimum order amount
      if (summary.subtotal < coupon.minOrderAmount) {
        setCouponError(
          `Minimum order amount for this coupon is ₹${coupon.minOrderAmount}. ` +
          `Your cart is ₹${summary.subtotal}.`
        );
        return;
      }

      setAppliedCoupon(coupon);
    } catch (err) {
      setCouponError('Could not apply coupon. Please try again.');
    } finally {
      setIsApplyingCoupon(false);
    }
  };

  const removeCoupon = (): void => {
    setAppliedCoupon(null);
    setCouponCode('');
    setCouponError('');
  };

  return (
    <div style={{ padding: 16, border: '1px solid #eee', borderRadius: 8 }}>
      <h3>Order Summary</h3>

      <div style={{ display: 'flex', justifyContent: 'space-between' }}>
        <span>Subtotal ({summary.itemCount} items)</span>
        <span>₹{summary.subtotal.toLocaleString()}</span>
      </div>

      {summary.discount > 0 && (
        <div style={{ display: 'flex', justifyContent: 'space-between', color: 'green' }}>
          <span>
            Discount ({appliedCoupon?.discountPercent}% off)
            <button onClick={removeCoupon} style={{ marginLeft: 8, fontSize: 12 }}>
              Remove
            </button>
          </span>
          <span>− ₹{summary.discount.toFixed(2)}</span>
        </div>
      )}

      <div style={{ display: 'flex', justifyContent: 'space-between' }}>
        <span>Delivery</span>
        <span>
          {summary.deliveryFee === 0 ? (
            <span style={{ color: 'green' }}>FREE</span>
          ) : (
            `₹${summary.deliveryFee}`
          )}
        </span>
      </div>

      <hr />

      <div style={{ display: 'flex', justifyContent: 'space-between', fontWeight: 'bold', fontSize: 18 }}>
        <span>Total</span>
        <span>₹{summary.total.toFixed(2)}</span>
      </div>

      {/* Coupon input */}
      {!appliedCoupon ? (
        <div style={{ marginTop: 16 }}>
          <input
            value={couponCode}
            onChange={e => setCouponCode(e.target.value.toUpperCase())}
            placeholder="Enter coupon code"
          />
          <button onClick={applyCoupon} disabled={isApplyingCoupon}>
            {isApplyingCoupon ? 'Applying...' : 'Apply'}
          </button>
          {couponError && <p style={{ color: 'red', fontSize: 14 }}>{couponError}</p>}
        </div>
      ) : (
        <p style={{ color: 'green', marginTop: 8 }}>
          ✓ Coupon {appliedCoupon.code} applied — {appliedCoupon.discountPercent}% off
        </p>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Three specific calculation fixes, `useMemo` for derived cart
values, and the minimum order amount validation before applying
a coupon. They also look for free delivery threshold logic.

**🔷 TypeScript lesson:**
`useMemo` returns a typed value — `useMemo((): CartSummary => {...}, [deps])`
gives you a fully typed `summary` object. Without the explicit
return type annotation on the useMemo callback, TypeScript
infers it — but being explicit documents intent clearly.

</details>

---

### 🔴 Pattern 9 · Scenario 3 — The Cart That Does Not Persist

🏢 **Situation:**
You are at an e-commerce startup. Users spend 10 minutes
building their cart, then accidentally close the browser tab.
When they come back, the cart is completely empty. You are
losing conversions. Your PM wants: "Cart must survive page
refresh, tab close, and browser restart."

🔷 **Types for this scenario:**
```typescript
interface CartItem {
  id: string;
  name: string;
  price: number;
  image: string;
  quantity: number;
}

type CartAction =
  | { type: 'ADD_ITEM'; payload: CartItem }
  | { type: 'REMOVE_ITEM'; payload: string }
  | { type: 'UPDATE_QUANTITY'; payload: { id: string; quantity: number } }
  | { type: 'LOAD_FROM_STORAGE'; payload: CartItem[] }
  | { type: 'CLEAR_CART' };
```

**Here is the current code:**

```tsx
import { useReducer } from 'react';

function cartReducer(state: CartItem[], action: CartAction): CartItem[] {
  switch (action.type) {
    case 'ADD_ITEM':
      return [...state, action.payload];
    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.payload);
    case 'CLEAR_CART':
      return [];
    default:
      return state;
  }
}

function useCart() {
  // BUG: no localStorage — cart is lost on refresh
  const [cartItems, dispatch] = useReducer(cartReducer, []);

  const addItem = (item: CartItem): void => {
    dispatch({ type: 'ADD_ITEM', payload: item });
  };

  const removeItem = (id: string): void => {
    dispatch({ type: 'REMOVE_ITEM', payload: id });
  };

  return { cartItems, addItem, removeItem };
}
```

**Your tasks:**
- ✅ Load cart from localStorage on app startup (lazy init)
- ✅ Save cart to localStorage whenever it changes
- ✅ Handle corrupted localStorage data safely
- ✅ Create a typed `storage` utility for safe get/set

<details>
<summary>💡 Hint</summary>

`useReducer` does not have lazy initialization like `useState`.
Pass the initial state as the third argument: `useReducer(reducer, [], init)`
where `init` is a function that reads from localStorage.
Use `useEffect` to sync to localStorage whenever `cartItems` changes.
Wrap both the read and write in try/catch.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useReducer, useEffect } from 'react';

interface CartItem {
  id: string;
  name: string;
  price: number;
  image: string;
  quantity: number;
}

type CartAction =
  | { type: 'ADD_ITEM'; payload: CartItem }
  | { type: 'REMOVE_ITEM'; payload: string }
  | { type: 'UPDATE_QUANTITY'; payload: { id: string; quantity: number } }
  | { type: 'CLEAR_CART' };

// Typed safe storage utility
const storage = {
  get: <T>(key: string, fallback: T): T => {
    try {
      const item = localStorage.getItem(key);
      return item ? (JSON.parse(item) as T) : fallback;
    } catch {
      return fallback;
    }
  },
  set: <T>(key: string, value: T): void => {
    try {
      localStorage.setItem(key, JSON.stringify(value));
    } catch (err) {
      console.warn(`Failed to save to localStorage:`, err);
    }
  },
  remove: (key: string): void => {
    try {
      localStorage.removeItem(key);
    } catch {
      // Silently fail
    }
  },
};

const CART_KEY = 'shopping-cart-v1';

function cartReducer(state: CartItem[], action: CartAction): CartItem[] {
  switch (action.type) {
    case 'ADD_ITEM': {
      const existing = state.find(item => item.id === action.payload.id);
      if (existing) {
        return state.map(item =>
          item.id === action.payload.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...state, action.payload];
    }
    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.payload);
    case 'UPDATE_QUANTITY': {
      const { id, quantity } = action.payload;
      if (quantity <= 0) return state.filter(item => item.id !== id);
      return state.map(item =>
        item.id === id ? { ...item, quantity } : item
      );
    }
    case 'CLEAR_CART':
      return [];
    default:
      return state;
  }
}

// Init function for useReducer — reads from localStorage once
function initCart(): CartItem[] {
  return storage.get<CartItem[]>(CART_KEY, []);
}

interface UseCartReturn {
  cartItems: CartItem[];
  itemCount: number;
  total: number;
  addItem: (item: CartItem) => void;
  removeItem: (id: string) => void;
  updateQuantity: (id: string, quantity: number) => void;
  clearCart: () => void;
}

function useCart(): UseCartReturn {
  // Third argument to useReducer is the init function
  const [cartItems, dispatch] = useReducer(cartReducer, [], initCart);

  // Sync to localStorage whenever cart changes
  useEffect(() => {
    if (cartItems.length === 0) {
      storage.remove(CART_KEY);
    } else {
      storage.set(CART_KEY, cartItems);
    }
  }, [cartItems]);

  const itemCount = cartItems.reduce((sum, item) => sum + item.quantity, 0);
  const total = cartItems.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  return {
    cartItems,
    itemCount,
    total,
    addItem: (item: CartItem) => dispatch({ type: 'ADD_ITEM', payload: item }),
    removeItem: (id: string) => dispatch({ type: 'REMOVE_ITEM', payload: id }),
    updateQuantity: (id: string, quantity: number) =>
      dispatch({ type: 'UPDATE_QUANTITY', payload: { id, quantity } }),
    clearCart: () => dispatch({ type: 'CLEAR_CART' }),
  };
}
```

**✅ What the interviewer is checking:**
`useReducer(cartReducer, [], initCart)` — the third argument
for lazy initialization. They also look for the generic
`storage.get<CartItem[]>()` pattern and sync in `useEffect`.

**🔷 TypeScript lesson:**
Generic functions `storage.get<T>` and `storage.set<T>` let you
type localStorage reads and writes. Without the generic, you
would get `unknown` from `JSON.parse`, requiring manual casts
everywhere. With the generic, you cast once in the utility.

</details>

---

### 🔴 Pattern 9 · Scenario 4 — The Cart With No Stock Check

🏢 **Situation:**
You are at an e-commerce company. Users are placing orders for
10 units of items that only have 2 in stock. Your warehouse
team is angry. The frontend lets users add as many items as
they want with no stock validation. Add stock checks.

🔷 **Types for this scenario:**
```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  stock: number;
}

interface CartItem extends Product {
  quantity: number;
}

interface StockCheckResult {
  available: boolean;
  currentStock: number;
  requestedQuantity: number;
}
```

**Here is the current code:**

```tsx
import { useReducer } from 'react';

function cartReducer(state: CartItem[], action: CartAction): CartItem[] {
  switch (action.type) {
    case 'ADD_ITEM':
      // BUG: no stock check — adds unlimited quantity
      const existing = state.find(item => item.id === action.payload.id);
      if (existing) {
        return state.map(item =>
          item.id === action.payload.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...state, { ...action.payload, quantity: 1 }];
    default:
      return state;
  }
}
```

**Your tasks:**
- ✅ Prevent adding more items than available stock
- ✅ Disable the "Add to Cart" button when stock is 0
- ✅ Show "Only X left" warning when quantity is near stock limit
- ✅ Show an error message when user tries to exceed stock

<details>
<summary>💡 Hint</summary>

In the ADD_ITEM reducer case, check the current quantity in
the cart for that item. If `existing.quantity >= action.payload.stock`,
do not increase — return state unchanged. Outside the reducer,
compute a `canAddMore` boolean per item by comparing the cart
quantity with the product stock, and use it to disable the button.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useReducer, useState } from 'react';

interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
  stock: number;
}

interface CartItem extends Product {
  quantity: number;
}

type CartAction =
  | { type: 'ADD_ITEM'; payload: Product }
  | { type: 'REMOVE_ITEM'; payload: string }
  | { type: 'UPDATE_QUANTITY'; payload: { id: string; quantity: number } }
  | { type: 'CLEAR_CART' };

function cartReducer(state: CartItem[], action: CartAction): CartItem[] {
  switch (action.type) {
    case 'ADD_ITEM': {
      const existing = state.find(item => item.id === action.payload.id);

      if (existing) {
        // STOCK CHECK: do not exceed available stock
        if (existing.quantity >= action.payload.stock) {
          return state; // return unchanged — cannot add more
        }
        return state.map(item =>
          item.id === action.payload.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }

      // STOCK CHECK: cannot add if out of stock
      if (action.payload.stock === 0) {
        return state;
      }

      return [...state, { ...action.payload, quantity: 1 }];
    }

    case 'UPDATE_QUANTITY': {
      const { id, quantity } = action.payload;
      if (quantity <= 0) return state.filter(item => item.id !== id);
      return state.map(item => {
        if (item.id !== id) return item;
        // STOCK CHECK: cap at available stock
        const safeQuantity = Math.min(quantity, item.stock);
        return { ...item, quantity: safeQuantity };
      });
    }

    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.payload);

    case 'CLEAR_CART':
      return [];

    default:
      return state;
  }
}

interface ProductCardProps {
  product: Product;
  cartItems: CartItem[];
  onAddToCart: (product: Product) => void;
}

const LOW_STOCK_THRESHOLD = 3;

function ProductCard({
  product,
  cartItems,
  onAddToCart,
}: ProductCardProps): JSX.Element {
  const [stockError, setStockError] = useState<string>('');

  // Find this product's quantity in cart
  const cartItem = cartItems.find(item => item.id === product.id);
  const quantityInCart = cartItem?.quantity ?? 0;
  const remainingStock = product.stock - quantityInCart;
  const isOutOfStock = product.stock === 0;
  const isCartFull = quantityInCart >= product.stock;
  const isLowStock = product.stock > 0 && product.stock <= LOW_STOCK_THRESHOLD;

  const handleAddToCart = (): void => {
    if (isCartFull || isOutOfStock) {
      setStockError(`Only ${product.stock} available in stock`);
      setTimeout(() => setStockError(''), 3000);
      return;
    }
    onAddToCart(product);
    setStockError('');
  };

  return (
    <div style={{ border: '1px solid #eee', padding: 16, borderRadius: 8 }}>
      <img src={product.image} alt={product.name} style={{ width: '100%' }} />
      <h3>{product.name}</h3>
      <p>₹{product.price.toLocaleString()}</p>

      {/* Stock status messages */}
      {isOutOfStock && (
        <p style={{ color: 'red', fontWeight: 500 }}>Out of Stock</p>
      )}
      {isLowStock && !isOutOfStock && (
        <p style={{ color: 'orange', fontWeight: 500 }}>
          Only {product.stock} left!
        </p>
      )}
      {quantityInCart > 0 && !isOutOfStock && (
        <p style={{ color: '#666', fontSize: 14 }}>
          {quantityInCart} in cart · {remainingStock} remaining
        </p>
      )}

      {stockError && (
        <p style={{ color: 'red', fontSize: 14 }}>{stockError}</p>
      )}

      <button
        onClick={handleAddToCart}
        disabled={isOutOfStock || isCartFull}
        style={{
          width: '100%',
          padding: 8,
          background: isOutOfStock || isCartFull ? '#ccc' : '#2563eb',
          color: 'white',
          border: 'none',
          borderRadius: 4,
          cursor: isOutOfStock || isCartFull ? 'not-allowed' : 'pointer',
        }}
      >
        {isOutOfStock ? 'Out of Stock' : isCartFull ? 'Max Quantity' : 'Add to Cart'}
      </button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Stock check in the reducer (`existing.quantity >= stock`) AND
disabled button in the UI — both layers of validation. They
also look for the "Only X left" warning and the "X in cart"
counter so users know their current position vs available stock.

**🔷 TypeScript lesson:**
`cartItem?.quantity ?? 0` uses optional chaining and nullish
coalescing together. `cartItem` may be `undefined` if the
product is not in the cart, so `?.quantity` returns `undefined`,
and `?? 0` gives the fallback of 0. This is cleaner and more
type-safe than `cartItem ? cartItem.quantity : 0`.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 10: 🌙 Dark Mode / User Preferences

> **Level 2 — Intermediate**

🎯 **What you will build in this pattern:**
- A dark mode toggle that persists across sessions
- A global theme context accessible by all components
- A preferences system that respects system-level settings

📋 **Interview asks this as:**
- "Add a dark mode toggle to this app"
- "Build a theme system that any component can access"
- "Make the theme persist when the user refreshes the page"

🔷 **TypeScript concepts used in this pattern:**
- `'light' | 'dark'` union type for theme
- Typed context with `createContext<ThemeContextType | null>(null)`
- `window.matchMedia` typed return for system preference detection

🚨 **Most common mistake in interviews:**
A white flash before the correct theme loads. This happens when
you read localStorage in a `useEffect` (after render) instead
of lazy-initializing state. Always initialize theme state
directly from localStorage so the correct theme renders first.

---

### 🔴 Pattern 10 · Scenario 1 — The Dark Mode That Flashes on Load

🏢 **Situation:**
You are at a SaaS company. The dark mode toggle works and saves
to localStorage. But every time a dark mode user refreshes the
page, they see a blinding white flash for half a second before
the dark theme kicks in. Your design team says this is a
critical UX bug. Fix it.

🔷 **Types for this scenario:**
```typescript
type Theme = 'light' | 'dark';

interface ThemeState {
  theme: Theme;
  toggleTheme: () => void;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

function App(): JSX.Element {
  // BUG: starts as 'light' before useEffect runs
  const [theme, setTheme] = useState<Theme>('light');

  // BUG: runs AFTER first render — causes flash
  useEffect(() => {
    const saved = localStorage.getItem('theme') as Theme | null;
    if (saved) setTheme(saved);
  }, []);

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem('theme', theme);
  }, [theme]);

  return (
    <div>
      <button onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}>
        Toggle {theme === 'light' ? '🌙' : '☀️'}
      </button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the white flash by initializing theme directly from localStorage
- ✅ Apply the theme class to `document.documentElement` immediately
- ✅ Remove the useEffect that reads from localStorage on mount

<details>
<summary>💡 Hint</summary>

The flash happens because `useState('light')` renders with
light theme, THEN `useEffect` reads localStorage and switches
to dark. The fix: use lazy initialization in `useState` —
pass a function that reads from localStorage so the correct
theme is set on the very first render.
`useState<Theme>(() => { ... read localStorage ... })`

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

type Theme = 'light' | 'dark';

const THEME_KEY = 'app-theme';
const DEFAULT_THEME: Theme = 'light';

function getInitialTheme(): Theme {
  try {
    const saved = localStorage.getItem(THEME_KEY) as Theme | null;
    if (saved === 'light' || saved === 'dark') return saved;
  } catch {
    // localStorage not available
  }
  return DEFAULT_THEME;
}

function App(): JSX.Element {
  // FIX: lazy initialization — reads localStorage BEFORE first render
  // No flash because correct theme is set immediately
  const [theme, setTheme] = useState<Theme>(getInitialTheme);

  // Apply theme to document and save to localStorage whenever it changes
  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    document.documentElement.classList.toggle('dark', theme === 'dark');
    try {
      localStorage.setItem(THEME_KEY, theme);
    } catch {
      // Storage not available — continue without persisting
    }
  }, [theme]);

  const toggleTheme = (): void => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  return (
    <div data-theme={theme}>
      <button
        onClick={toggleTheme}
        aria-label={`Switch to ${theme === 'light' ? 'dark' : 'light'} mode`}
      >
        {theme === 'light' ? '🌙 Dark Mode' : '☀️ Light Mode'}
      </button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`useState<Theme>(getInitialTheme)` — passing a function to
`useState` not a value. This is lazy initialization and it
runs once before the first render. They specifically check
that you remove the `useEffect` that reads localStorage
and replace it with this pattern.

**🔷 TypeScript lesson:**
`as Theme | null` narrows the return type of `localStorage.getItem`
from `string | null` to `Theme | null`. Then the explicit check
`saved === 'light' || saved === 'dark'` narrows further to `Theme`
before using it — preventing invalid values from corrupted storage.

</details>

---

### 🔴 Pattern 10 · Scenario 2 — The Theme That Does Not Reach Deep Components

🏢 **Situation:**
You are at a social platform. The theme toggle in the header
works. But the `NotificationPanel` component, the `PostCard`
component, and the `CommentList` component — buried 5 levels
deep in the component tree — all stay light mode because they
do not have access to the theme state. Prop drilling it down
would take hours. Use Context.

🔷 **Types for this scenario:**
```typescript
type Theme = 'light' | 'dark';

interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
  isDark: boolean;
}
```

**Here is the current code:**

```tsx
// Theme state lives only in App — not accessible to deep components
function App(): JSX.Element {
  const [theme, setTheme] = useState<Theme>('light');

  return (
    // Must pass theme as prop to every child — prop drilling
    <div>
      <Header theme={theme} onToggle={() => setTheme(t =>
        t === 'light' ? 'dark' : 'light'
      )} />
      <main>
        <Feed theme={theme} />
      </main>
    </div>
  );
}

// Every component in the chain must accept and forward theme
interface FeedProps { theme: Theme; }
function Feed({ theme }: FeedProps): JSX.Element {
  return <PostList theme={theme} />;
}
```

**Your tasks:**
- ✅ Create a `ThemeContext` and `ThemeProvider`
- ✅ Create a typed `useTheme()` hook that throws if used outside provider
- ✅ Remove all theme prop drilling from the component tree
- ✅ Any component can call `useTheme()` to get theme and toggle

<details>
<summary>✅ Full Solution</summary>

```tsx
import {
  createContext, useContext, useState, useEffect,
  type ReactNode
} from 'react';

type Theme = 'light' | 'dark';

interface ThemeContextType {
  theme: Theme;
  toggleTheme: () => void;
  isDark: boolean;
}

const ThemeContext = createContext<ThemeContextType | null>(null);

const THEME_KEY = 'app-theme';

interface ThemeProviderProps {
  children: ReactNode;
}

function ThemeProvider({ children }: ThemeProviderProps): JSX.Element {
  const [theme, setTheme] = useState<Theme>(() => {
    try {
      const saved = localStorage.getItem(THEME_KEY) as Theme | null;
      if (saved === 'light' || saved === 'dark') return saved;
    } catch { /* ignore */ }
    return 'light';
  });

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    document.documentElement.classList.toggle('dark', theme === 'dark');
    try { localStorage.setItem(THEME_KEY, theme); } catch { /* ignore */ }
  }, [theme]);

  const toggleTheme = (): void => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  const value: ThemeContextType = {
    theme,
    toggleTheme,
    isDark: theme === 'dark',
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// Typed hook — throws helpful error if outside provider
function useTheme(): ThemeContextType {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error(
      'useTheme must be used inside <ThemeProvider>. ' +
      'Wrap your app root with <ThemeProvider>.'
    );
  }
  return context;
}

// App — wrap once at the top
function App(): JSX.Element {
  return (
    <ThemeProvider>
      <Header />
      <main>
        <Feed />
      </main>
    </ThemeProvider>
  );
}

// Header — no theme prop needed
function Header(): JSX.Element {
  const { theme, toggleTheme } = useTheme();
  return (
    <header>
      <button onClick={toggleTheme}>
        {theme === 'light' ? '🌙' : '☀️'}
      </button>
    </header>
  );
}

// Deep component — uses hook directly
function PostCard(): JSX.Element {
  const { isDark } = useTheme();
  return (
    <div style={{
      background: isDark ? '#1f2937' : '#ffffff',
      color: isDark ? '#f9fafb' : '#111827',
      padding: 16, borderRadius: 8,
    }}>
      Post content here
    </div>
  );
}

function Feed(): JSX.Element { return <PostCard />; }
```

**✅ What the interviewer is checking:**
`createContext<ThemeContextType | null>(null)` with the null
guard in `useTheme()`. No prop drilling anywhere — components
call `useTheme()` directly. They also look for `isDark` as
a convenience boolean derived from the theme string.

**🔷 TypeScript lesson:**
The explicit `const value: ThemeContextType = {...}` annotation
ensures the context value matches the declared type exactly.
If you add a new field to `ThemeContextType`, TypeScript will
flag the missing field in the value object immediately.

</details>

---

### 🔴 Pattern 10 · Scenario 3 — The Preferences That Do Not Sync

🏢 **Situation:**
You are at a SaaS dashboard. Users can set font size (small,
medium, large), language (English, Hindi), and notification
preferences. When they change settings on the Settings page,
the dashboard does not update immediately — they have to refresh.
Also, if they open the app in two tabs, changes in one tab
are not reflected in the other.

🔷 **Types for this scenario:**
```typescript
type FontSize = 'small' | 'medium' | 'large';
type Language = 'en' | 'hi' | 'ta';

interface UserPreferences {
  theme: 'light' | 'dark';
  fontSize: FontSize;
  language: Language;
  emailNotifications: boolean;
  pushNotifications: boolean;
}
```

**Here is the current code:**

```tsx
import { useState } from 'react';

function SettingsPage(): JSX.Element {
  const [prefs, setPrefs] = useState<UserPreferences>({
    theme: 'light',
    fontSize: 'medium',
    language: 'en',
    emailNotifications: true,
    pushNotifications: false,
  });

  const updatePref = <K extends keyof UserPreferences>(
    key: K,
    value: UserPreferences[K]
  ): void => {
    setPrefs(prev => ({ ...prev, [key]: value }));
    // BUG: saves to localStorage but dashboard does not know
    localStorage.setItem('preferences', JSON.stringify({ ...prefs, [key]: value }));
  };

  return (
    <div>
      <select value={prefs.fontSize} onChange={e =>
        updatePref('fontSize', e.target.value as FontSize)
      }>
        <option value="small">Small</option>
        <option value="medium">Medium</option>
        <option value="large">Large</option>
      </select>
    </div>
  );
}
```

**Your tasks:**
- ✅ Apply preferences to the document immediately when changed
- ✅ Listen for `storage` events to sync changes across browser tabs
- ✅ Use the typed generic `updatePref` function correctly
- ✅ Add a reset to defaults button

<details>
<summary>💡 Hint</summary>

To sync across tabs, listen for the browser's `storage` event
on `window`. This event fires in OTHER tabs when localStorage
changes. In the handler, re-read the preferences from storage
and update state. The current tab updates immediately via
`setPrefs` — the other tabs update via the storage event.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

type FontSize = 'small' | 'medium' | 'large';
type Language = 'en' | 'hi' | 'ta';
type Theme = 'light' | 'dark';

interface UserPreferences {
  theme: Theme;
  fontSize: FontSize;
  language: Language;
  emailNotifications: boolean;
  pushNotifications: boolean;
}

const PREFS_KEY = 'user-preferences';

const DEFAULT_PREFS: UserPreferences = {
  theme: 'light',
  fontSize: 'medium',
  language: 'en',
  emailNotifications: true,
  pushNotifications: false,
};

const FONT_SIZE_MAP: Record<FontSize, string> = {
  small: '14px',
  medium: '16px',
  large: '18px',
};

function getStoredPrefs(): UserPreferences {
  try {
    const saved = localStorage.getItem(PREFS_KEY);
    if (!saved) return DEFAULT_PREFS;
    return { ...DEFAULT_PREFS, ...JSON.parse(saved) as Partial<UserPreferences> };
  } catch {
    return DEFAULT_PREFS;
  }
}

function SettingsPage(): JSX.Element {
  const [prefs, setPrefs] = useState<UserPreferences>(getStoredPrefs);

  // Apply preferences to document whenever they change
  useEffect(() => {
    document.documentElement.setAttribute('data-theme', prefs.theme);
    document.documentElement.style.fontSize = FONT_SIZE_MAP[prefs.fontSize];
    document.documentElement.setAttribute('lang', prefs.language);
  }, [prefs]);

  // Listen for storage events to sync across browser tabs
  useEffect(() => {
    const handleStorageChange = (e: StorageEvent): void => {
      // Only react to changes to our preferences key
      if (e.key !== PREFS_KEY) return;
      if (!e.newValue) return;

      try {
        const updated = JSON.parse(e.newValue) as UserPreferences;
        setPrefs({ ...DEFAULT_PREFS, ...updated });
      } catch {
        // Ignore corrupted data
      }
    };

    window.addEventListener('storage', handleStorageChange);
    return () => window.removeEventListener('storage', handleStorageChange);
  }, []);

  // Typed generic update function — key must be a valid preference key
  const updatePref = <K extends keyof UserPreferences>(
    key: K,
    value: UserPreferences[K]
  ): void => {
    setPrefs(prev => {
      const updated = { ...prev, [key]: value };
      try {
        localStorage.setItem(PREFS_KEY, JSON.stringify(updated));
      } catch {
        console.warn('Could not save preferences');
      }
      return updated;
    });
  };

  const resetToDefaults = (): void => {
    setPrefs(DEFAULT_PREFS);
    try {
      localStorage.setItem(PREFS_KEY, JSON.stringify(DEFAULT_PREFS));
    } catch {
      // Ignore storage errors
    }
  };

  return (
    <div>
      <h2>Settings</h2>

      <label>
        Theme:
        <select
          value={prefs.theme}
          onChange={e => updatePref('theme', e.target.value as Theme)}
        >
          <option value="light">Light</option>
          <option value="dark">Dark</option>
        </select>
      </label>

      <label>
        Font Size:
        <select
          value={prefs.fontSize}
          onChange={e => updatePref('fontSize', e.target.value as FontSize)}
        >
          <option value="small">Small</option>
          <option value="medium">Medium</option>
          <option value="large">Large</option>
        </select>
      </label>

      <label>
        Language:
        <select
          value={prefs.language}
          onChange={e => updatePref('language', e.target.value as Language)}
        >
          <option value="en">English</option>
          <option value="hi">Hindi</option>
          <option value="ta">Tamil</option>
        </select>
      </label>

      <label>
        Email Notifications:
        <input
          type="checkbox"
          checked={prefs.emailNotifications}
          onChange={e => updatePref('emailNotifications', e.target.checked)}
        />
      </label>

      <button onClick={resetToDefaults}>Reset to Defaults</button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `StorageEvent` listener for cross-tab sync — this is an
advanced feature most candidates miss. They also look for
the `updatePref` generic with `K extends keyof UserPreferences`
which ensures type safety on both the key and value.

**🔷 TypeScript lesson:**
`<K extends keyof UserPreferences>(key: K, value: UserPreferences[K])`
is a constrained generic. The `K extends keyof` ensures `key`
is a valid property name, and `UserPreferences[K]` ensures
`value` matches the type of that specific property. You cannot
pass a `string` value for a `boolean` field.

</details>

---

### 🔴 Pattern 10 · Scenario 4 — The Theme That Ignores System Preference

🏢 **Situation:**
You are at a productivity app. Users with system-level dark mode
enabled (OS setting) open the app for the first time — they see
light mode. They have to manually toggle to dark mode. Your PM
wants: "Detect system preference on first visit. After that,
respect the user's manual choice."

🔷 **Types for this scenario:**
```typescript
type Theme = 'light' | 'dark';
type ThemeSource = 'system' | 'user';

interface ThemeConfig {
  theme: Theme;
  source: ThemeSource;
}
```

**Here is the current code:**

```tsx
import { useState } from 'react';

function useTheme() {
  // BUG: ignores system preference entirely
  const [theme, setTheme] = useState<Theme>('light');

  const toggleTheme = (): void => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };

  return { theme, toggleTheme };
}
```

**Your tasks:**
- ✅ Detect system dark mode preference using `window.matchMedia`
- ✅ Use system preference only on first visit (no localStorage value)
- ✅ Override with user preference once they manually toggle
- ✅ Listen for system preference changes in real time

<details>
<summary>💡 Hint</summary>

`window.matchMedia('(prefers-color-scheme: dark)').matches` returns
`true` if the OS is in dark mode. Check this only as a fallback
when there is no saved preference in localStorage. Also add an
event listener on the `MediaQueryList` to update the theme if
the user changes their OS setting — but only if they have not
manually set a preference yet.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

type Theme = 'light' | 'dark';

const THEME_KEY = 'app-theme';

function getSystemTheme(): Theme {
  if (typeof window === 'undefined') return 'light';
  return window.matchMedia('(prefers-color-scheme: dark)').matches
    ? 'dark'
    : 'light';
}

function getInitialTheme(): Theme {
  try {
    // Check if user has made a manual choice before
    const saved = localStorage.getItem(THEME_KEY) as Theme | null;
    if (saved === 'light' || saved === 'dark') return saved;
  } catch { /* ignore */ }

  // No saved preference — use system setting
  return getSystemTheme();
}

interface UseThemeReturn {
  theme: Theme;
  toggleTheme: () => void;
  isSystemControlled: boolean;
}

function useTheme(): UseThemeReturn {
  const [theme, setTheme] = useState<Theme>(getInitialTheme);

  // Track whether user has made a manual choice
  const [hasUserChoice, setHasUserChoice] = useState<boolean>(() => {
    try {
      return localStorage.getItem(THEME_KEY) !== null;
    } catch {
      return false;
    }
  });

  // Apply theme to document
  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    document.documentElement.classList.toggle('dark', theme === 'dark');
  }, [theme]);

  // Listen for system theme changes — only applies if user has not set a preference
  useEffect(() => {
    const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');

    const handleSystemChange = (e: MediaQueryListEvent): void => {
      if (!hasUserChoice) {
        // No manual choice — follow system
        setTheme(e.matches ? 'dark' : 'light');
      }
    };

    mediaQuery.addEventListener('change', handleSystemChange);
    return () => mediaQuery.removeEventListener('change', handleSystemChange);
  }, [hasUserChoice]);

  const toggleTheme = (): void => {
    const newTheme: Theme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    setHasUserChoice(true); // user has made a manual choice

    try {
      localStorage.setItem(THEME_KEY, newTheme);
    } catch { /* ignore */ }
  };

  return {
    theme,
    toggleTheme,
    isSystemControlled: !hasUserChoice,
  };
}

function ThemeToggle(): JSX.Element {
  const { theme, toggleTheme, isSystemControlled } = useTheme();

  return (
    <div>
      <button onClick={toggleTheme}>
        {theme === 'light' ? '🌙 Dark Mode' : '☀️ Light Mode'}
      </button>
      {isSystemControlled && (
        <small style={{ color: '#666', marginLeft: 8 }}>
          Following system preference
        </small>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`window.matchMedia('(prefers-color-scheme: dark)')` for system
preference detection. The `MediaQueryListEvent` listener for
real-time system changes. The `hasUserChoice` flag to stop
following system once the user makes a manual selection.

**🔷 TypeScript lesson:**
`MediaQueryListEvent` is the typed parameter for the `change`
event on a `MediaQueryList`. TypeScript knows `e.matches` is
`boolean` — the OS dark mode state. Without the type, you would
get `Event` which has no `matches` property.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 11: 🔐 Auth Flow

> **Level 2 — Intermediate**

🎯 **What you will build in this pattern:**
- A full login → token → protected route → logout flow
- An auth context that stores user and role globally
- Token expiry detection and silent refresh

📋 **Interview asks this as:**
- "Implement the complete authentication flow"
- "Build a protected route that checks if the user is logged in"
- "How do you store and send JWT tokens securely?"

🔷 **TypeScript concepts used in this pattern:**
- `User` interface with `role` as union type
- Typed `AuthContext` with null guard custom hook
- `Axios` interceptor typed with `AxiosRequestConfig` and `AxiosError`

🚨 **Most common mistake in interviews:**
Storing the JWT token in localStorage and thinking that is
secure. Interviewers at security-conscious companies want to
hear about `httpOnly` cookies for refresh tokens and memory
(JavaScript variable) for access tokens. Also forgetting to
handle the page refresh case — when the user refreshes, the
in-memory token is gone and needs to be restored.

---

### 🔴 Pattern 11 · Scenario 1 — The Token Stored in the Wrong Place

🏢 **Situation:**
You are reviewing a PR at a fintech company. A junior dev
stored the JWT access token in localStorage. Your security
lead rejects the PR and says: "XSS can steal tokens from
localStorage. Access token goes in memory. Refresh token
goes in httpOnly cookie." Rewrite the token storage correctly.

🔷 **Types for this scenario:**
```typescript
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'manager' | 'user';
}

interface LoginResponse {
  accessToken: string;
  user: User;
  // refreshToken comes as httpOnly cookie — not in response body
}

interface AuthState {
  user: User | null;
  accessToken: string | null;
  isLoading: boolean;
}
```

**Here is the current code:**

```tsx
// BAD: stores token in localStorage
const handleLogin = async (email: string, password: string): Promise<void> => {
  const res = await fetch('/api/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, password }),
  });
  const data = await res.json() as { token: string; user: User };

  // SECURITY ISSUE: localStorage can be stolen by XSS
  localStorage.setItem('token', data.token);
  localStorage.setItem('user', JSON.stringify(data.user));
};
```

**Your tasks:**
- ✅ Store access token in memory (module-level variable) not localStorage
- ✅ Configure login to send `credentials: 'include'` for the httpOnly cookie
- ✅ Create `getAccessToken` and `setAccessToken` utility functions
- ✅ Attach token to every API request via a request interceptor pattern

<details>
<summary>💡 Hint</summary>

A module-level variable persists for the app session but
is gone on page refresh — that is intentional for the
access token. For page refreshes, you call `/api/auth/me`
with `credentials: 'include'` — the browser sends the
httpOnly cookie automatically. The server validates it
and returns a new access token.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
// tokenStore.ts — module-level storage, not localStorage
let accessToken: string | null = null;

export function getAccessToken(): string | null {
  return accessToken;
}

export function setAccessToken(token: string | null): void {
  accessToken = token;
}

export function clearAccessToken(): void {
  accessToken = null;
}

// api.ts — typed fetch utility with auth header
interface FetchOptions extends RequestInit {
  skipAuth?: boolean;
}

export async function apiFetch<T>(
  url: string,
  options: FetchOptions = {}
): Promise<T> {
  const { skipAuth = false, ...fetchOptions } = options;

  const headers: HeadersInit = {
    'Content-Type': 'application/json',
    ...fetchOptions.headers,
  };

  // Attach access token from memory — not localStorage
  if (!skipAuth) {
    const token = getAccessToken();
    if (token) {
      (headers as Record<string, string>)['Authorization'] = `Bearer ${token}`;
    }
  }

  const res = await fetch(url, {
    ...fetchOptions,
    headers,
    credentials: 'include', // sends httpOnly cookies automatically
  });

  if (!res.ok) {
    const errorData = await res.json().catch(() => ({}));
    throw new Error(
      (errorData as { message?: string }).message || `HTTP ${res.status}`
    );
  }

  return res.json() as Promise<T>;
}

// auth.ts — login flow
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'manager' | 'user';
}

interface LoginResponse {
  accessToken: string;
  user: User;
}

export async function login(
  email: string,
  password: string
): Promise<User> {
  const data = await apiFetch<LoginResponse>('/api/auth/login', {
    method: 'POST',
    body: JSON.stringify({ email, password }),
    skipAuth: true,
    // Server sets httpOnly refresh token cookie in Set-Cookie header
    // credentials: 'include' above sends this cookie on subsequent requests
  });

  // Store access token in memory — not localStorage
  setAccessToken(data.accessToken);

  return data.user;
}

export async function logout(): Promise<void> {
  await apiFetch('/api/auth/logout', {
    method: 'POST',
  }).catch(() => {
    // Even if API fails, clear local auth state
  });

  clearAccessToken();
}

// Restore session on page refresh
interface MeResponse {
  accessToken: string;
  user: User;
}

export async function restoreSession(): Promise<User | null> {
  try {
    const data = await apiFetch<MeResponse>('/api/auth/me', {
      skipAuth: true,
      // Browser sends the httpOnly cookie automatically
    });
    setAccessToken(data.accessToken);
    return data.user;
  } catch {
    return null; // No active session
  }
}
```

**✅ What the interviewer is checking:**
Module-level variable for the access token (not localStorage),
`credentials: 'include'` for cookie-based refresh token,
and the `restoreSession` pattern for page refresh recovery.
This shows security awareness beyond basic implementation.

**🔷 TypeScript lesson:**
`interface FetchOptions extends RequestInit` extends the
built-in browser `RequestInit` type to add custom fields.
This keeps full type compatibility with the native `fetch`
API while adding your own options like `skipAuth`.

</details>

---

### 🔴 Pattern 11 · Scenario 2 — The Auth State That Disappears on Refresh

🏢 **Situation:**
You are at a SaaS company. Users log in successfully. But
when they refresh the page, they are immediately logged out
and redirected to the login page — even though their session
should last 7 days. The backend team confirms the httpOnly
cookie is valid and present. The problem is in the frontend
auth state restoration.

🔷 **Types for this scenario:**
```typescript
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'manager' | 'user';
}

type AuthStatus =
  | { status: 'checking' }
  | { status: 'authenticated'; user: User }
  | { status: 'unauthenticated' };
```

**Here is the current code:**

```tsx
import { createContext, useContext, useState, type ReactNode } from 'react';

interface AuthContextType {
  user: User | null;
  isLoggedIn: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | null>(null);

function AuthProvider({ children }: { children: ReactNode }): JSX.Element {
  // BUG: starts as null — on refresh, user is null = logged out
  const [user, setUser] = useState<User | null>(null);

  // No session restoration on mount

  const login = async (email: string, password: string): Promise<void> => {
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      credentials: 'include',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password }),
    });
    const data = await res.json() as { user: User; accessToken: string };
    setUser(data.user);
    setAccessToken(data.accessToken);
  };

  return (
    <AuthContext.Provider value={{
      user,
      isLoggedIn: !!user,
      login,
      logout: () => { setUser(null); clearAccessToken(); },
    }}>
      {children}
    </AuthContext.Provider>
  );
}
```

**Your tasks:**
- ✅ Add session restoration on mount — call `/api/auth/me` with the cookie
- ✅ Use `AuthStatus` discriminated union instead of `User | null`
- ✅ Show a loading state while checking session (prevents flash to login)
- ✅ The `ProtectedRoute` must wait for the auth check to complete

<details>
<summary>💡 Hint</summary>

Add a `useEffect` in `AuthProvider` that calls `/api/auth/me`
with `credentials: 'include'` when the app loads. The browser
sends the httpOnly cookie automatically. If it returns a user,
set them as authenticated. The `status: 'checking'` state
prevents `ProtectedRoute` from redirecting before the check
finishes.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import {
  createContext, useContext, useState, useEffect,
  type ReactNode
} from 'react';
import { Navigate, Outlet } from 'react-router-dom';

interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'manager' | 'user';
}

// Discriminated union — cleaner than separate boolean flags
type AuthStatus =
  | { status: 'checking' }
  | { status: 'authenticated'; user: User }
  | { status: 'unauthenticated' };

interface AuthContextType {
  authStatus: AuthStatus;
  user: User | null;
  isAuthenticated: boolean;
  isChecking: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => Promise<void>;
}

const AuthContext = createContext<AuthContextType | null>(null);

interface MeResponse {
  user: User;
  accessToken: string;
}

interface LoginResponse {
  user: User;
  accessToken: string;
}

function AuthProvider({ children }: { children: ReactNode }): JSX.Element {
  const [authStatus, setAuthStatus] = useState<AuthStatus>({
    status: 'checking', // start as checking — not unauthenticated
  });

  // RESTORE SESSION on mount
  useEffect(() => {
    const restoreSession = async (): Promise<void> => {
      try {
        const res = await fetch('/api/auth/me', {
          credentials: 'include', // browser sends httpOnly cookie
        });

        if (res.ok) {
          const data = await res.json() as MeResponse;
          setAccessToken(data.accessToken);
          setAuthStatus({ status: 'authenticated', user: data.user });
        } else {
          setAuthStatus({ status: 'unauthenticated' });
        }
      } catch {
        // Network error — treat as unauthenticated
        setAuthStatus({ status: 'unauthenticated' });
      }
    };

    restoreSession();
  }, []); // run once on mount

  const login = async (email: string, password: string): Promise<void> => {
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      credentials: 'include',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password }),
    });

    if (!res.ok) {
      const data = await res.json() as { message: string };
      throw new Error(data.message || 'Login failed');
    }

    const data = await res.json() as LoginResponse;
    setAccessToken(data.accessToken);
    setAuthStatus({ status: 'authenticated', user: data.user });
  };

  const logout = async (): Promise<void> => {
    try {
      await fetch('/api/auth/logout', {
        method: 'POST',
        credentials: 'include',
      });
    } catch { /* ignore */ }

    clearAccessToken();
    setAuthStatus({ status: 'unauthenticated' });
  };

  const value: AuthContextType = {
    authStatus,
    user: authStatus.status === 'authenticated' ? authStatus.user : null,
    isAuthenticated: authStatus.status === 'authenticated',
    isChecking: authStatus.status === 'checking',
    login,
    logout,
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

function useAuth(): AuthContextType {
  const context = useContext(AuthContext);
  if (!context) throw new Error('useAuth must be inside AuthProvider');
  return context;
}

// Protected Route — waits for auth check before redirecting
function ProtectedRoute(): JSX.Element {
  const { isAuthenticated, isChecking } = useAuth();

  // Still checking session — do not redirect yet
  if (isChecking) {
    return <div>Loading...</div>;
  }

  // Check done — redirect if not authenticated
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return <Outlet />;
}

// Placeholder functions — defined in tokenStore.ts
function setAccessToken(_token: string): void {}
function clearAccessToken(): void {}
```

**✅ What the interviewer is checking:**
`status: 'checking'` as the initial state — this is the key
that prevents the flash redirect. The `useEffect` that calls
`/api/auth/me` on mount. `ProtectedRoute` returning a loader
when `isChecking` is true — not `<Navigate>`.

**🔷 TypeScript lesson:**
When `authStatus.status === 'authenticated'`, TypeScript narrows
the type and knows `authStatus.user` exists. The line
`authStatus.status === 'authenticated' ? authStatus.user : null`
is safe because TypeScript narrows the discriminated union.

</details>

---

### 🔴 Pattern 11 · Scenario 3 — The Role-Based UI That Anyone Can See

🏢 **Situation:**
You are at an HR platform. The admin panel shows "Delete User"
and "Export Data" buttons. Regular users should never see these.
But the buttons are only hidden with CSS (`display: none`) —
a developer opening DevTools can remove the CSS and see them.
Your security team wants proper role-based rendering.

🔷 **Types for this scenario:**
```typescript
type UserRole = 'admin' | 'manager' | 'user';

interface User {
  id: string;
  name: string;
  email: string;
  role: UserRole;
}

interface RoleGuardProps {
  allowedRoles: UserRole[];
  fallback?: React.ReactNode;
  children: React.ReactNode;
}
```

**Here is the current code:**

```tsx
function AdminActions({ user }: { user: User }): JSX.Element {
  return (
    <div>
      {/* BAD: CSS hiding — DevTools can remove this */}
      <div style={{ display: user.role === 'admin' ? 'block' : 'none' }}>
        <button>Delete User</button>
        <button>Export Data</button>
      </div>
      <button>Edit Profile</button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Replace CSS hiding with conditional rendering (component not in DOM)
- ✅ Build a reusable `RoleGuard` component
- ✅ Build a `usePermissions` hook that checks role-based access
- ✅ Explain why frontend checks are UX only — backend must always validate

<details>
<summary>💡 Hint</summary>

CSS hiding means the element IS in the DOM — React has rendered
it, just made it invisible. Conditional rendering means the
element is NOT in the DOM at all. Use `{user.role === 'admin' && <button>}`.
For the reusable `RoleGuard` component, accept `allowedRoles: UserRole[]`
and render `children` only if the user's role is in the array.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useCallback } from 'react';

type UserRole = 'admin' | 'manager' | 'user';

interface User {
  id: string;
  name: string;
  email: string;
  role: UserRole;
}

interface RoleGuardProps {
  allowedRoles: UserRole[];
  fallback?: React.ReactNode;
  children: React.ReactNode;
}

// Reusable role guard component
function RoleGuard({
  allowedRoles,
  fallback = null,
  children,
}: RoleGuardProps): JSX.Element {
  const { user } = useAuth();

  if (!user) return <>{fallback}</>;

  // Component not rendered at all if role not allowed
  // Not CSS hidden — completely absent from the DOM
  if (!allowedRoles.includes(user.role)) {
    return <>{fallback}</>;
  }

  return <>{children}</>;
}

// Hook for programmatic permission checks
interface UsePermissionsReturn {
  hasRole: (roles: UserRole[]) => boolean;
  isAdmin: boolean;
  isManager: boolean;
  canManageUsers: boolean;
  canExportData: boolean;
}

function usePermissions(): UsePermissionsReturn {
  const { user } = useAuth();

  const hasRole = useCallback(
    (roles: UserRole[]): boolean => {
      if (!user) return false;
      return roles.includes(user.role);
    },
    [user]
  );

  return {
    hasRole,
    isAdmin: hasRole(['admin']),
    isManager: hasRole(['admin', 'manager']),
    canManageUsers: hasRole(['admin']),
    canExportData: hasRole(['admin', 'manager']),
  };
}

// Clean component — no CSS hiding
function AdminActions(): JSX.Element {
  const { canManageUsers, canExportData } = usePermissions();

  return (
    <div>
      {/* Conditional render — not in DOM if not authorized */}
      <RoleGuard allowedRoles={['admin']}>
        <button>Delete User</button>
      </RoleGuard>

      <RoleGuard
        allowedRoles={['admin', 'manager']}
        fallback={<p style={{ color: '#999' }}>Contact admin to export</p>}
      >
        <button>Export Data</button>
      </RoleGuard>

      {/* Or using the hook for inline checks */}
      {canManageUsers && <button>Manage Permissions</button>}

      {/* Always visible */}
      <button>Edit Profile</button>
    </div>
  );
}

/*
 * IMPORTANT SECURITY NOTE — always tell the interviewer this:
 *
 * Frontend role checks are UX only — they improve the user
 * experience by not showing buttons the user cannot use.
 *
 * They are NOT security. A user can modify React state in
 * DevTools, send API requests directly with curl, or bypass
 * the frontend entirely.
 *
 * REAL security: the backend must validate the JWT role on
 * EVERY API request:
 *   DELETE /api/users/:id → verify token → check role === 'admin'
 *   GET /api/export → verify token → check role in ['admin', 'manager']
 *
 * The frontend is the door handle. The backend is the lock.
 */
```

**✅ What the interviewer is checking:**
They specifically want to hear: "Frontend checks are UX only,
the backend must always validate." This is the most important
point in RBAC discussions. They also check for `RoleGuard`
as a reusable component over scattered `user.role === 'admin'`
checks throughout the codebase.

**🔷 TypeScript lesson:**
`allowedRoles: UserRole[]` with `allowedRoles.includes(user.role)`
is type-safe — TypeScript ensures you can only pass valid role
strings. If you add a new role to the `UserRole` union, TypeScript
will tell you everywhere `UserRole` is used — including
`allowedRoles` arrays that might need updating.

</details>

---

### 🔴 Pattern 11 · Scenario 4 — The Token That Expires Mid-Session

🏢 **Situation:**
You are at a banking app. The access token expires every 15
minutes. Users who have been on a page for 20 minutes
suddenly get 401 errors on every API call — but with no
explanation. They just see blank data or broken forms.
Implement silent token refresh so users never notice
the expiry.

🔷 **Types for this scenario:**
```typescript
interface RefreshResponse {
  accessToken: string;
}

interface QueuedRequest {
  resolve: (token: string) => void;
  reject: (error: unknown) => void;
}
```

**Here is the current code:**

```tsx
// API calls fail silently with 401 — no refresh logic
async function apiFetch<T>(url: string, options?: RequestInit): Promise<T> {
  const token = getAccessToken();
  const res = await fetch(url, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      ...(token ? { Authorization: `Bearer ${token}` } : {}),
      ...options?.headers,
    },
    credentials: 'include',
  });

  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json() as Promise<T>;
}
```

**Your tasks:**
- ✅ Detect 401 responses and attempt a silent token refresh
- ✅ Retry the original request with the new token after refresh
- ✅ Handle multiple simultaneous 401s — only one refresh call
- ✅ Redirect to login if the refresh token is also expired

<details>
<summary>💡 Hint</summary>

The `isRefreshing` flag and `failedQueue` pattern prevents
multiple simultaneous refresh calls. When the first 401 fires,
set `isRefreshing = true`. All other 401s while refreshing
add themselves to `failedQueue` as promises. When refresh
succeeds, resolve all queued promises with the new token.
Each queued request then retries with the new token.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { getAccessToken, setAccessToken, clearAccessToken } from './tokenStore';

interface RefreshResponse {
  accessToken: string;
}

interface QueuedRequest {
  resolve: (token: string) => void;
  reject: (error: unknown) => void;
}

// Module-level state for refresh coordination
let isRefreshing = false;
let failedQueue: QueuedRequest[] = [];

function processQueue(error: unknown, token: string | null = null): void {
  failedQueue.forEach(({ resolve, reject }) => {
    if (error) {
      reject(error);
    } else {
      resolve(token as string);
    }
  });
  failedQueue = [];
}

async function refreshAccessToken(): Promise<string> {
  const res = await fetch('/api/auth/refresh', {
    method: 'POST',
    credentials: 'include', // sends httpOnly refresh token cookie
  });

  if (!res.ok) {
    throw new Error('Refresh token expired');
  }

  const data = await res.json() as RefreshResponse;
  return data.accessToken;
}

interface ExtendedRequestInit extends RequestInit {
  _retry?: boolean;
}

export async function apiFetch<T>(
  url: string,
  options: ExtendedRequestInit = {}
): Promise<T> {
  const token = getAccessToken();

  const headers: Record<string, string> = {
    'Content-Type': 'application/json',
    ...(token ? { Authorization: `Bearer ${token}` } : {}),
  };

  const res = await fetch(url, {
    ...options,
    headers: { ...headers, ...options.headers },
    credentials: 'include',
  });

  // Happy path — return response
  if (res.ok) {
    return res.json() as Promise<T>;
  }

  // 401 — token expired, attempt silent refresh
  if (res.status === 401 && !options._retry) {
    if (isRefreshing) {
      // Another refresh is in progress — queue this request
      return new Promise<T>((resolve, reject) => {
        failedQueue.push({
          resolve: (newToken: string) => {
            // Retry with new token when refresh completes
            resolve(
              apiFetch<T>(url, {
                ...options,
                _retry: true,
                headers: { ...headers, Authorization: `Bearer ${newToken}` },
              })
            );
          },
          reject,
        });
      });
    }

    // This is the first 401 — start refresh
    isRefreshing = true;

    try {
      const newToken = await refreshAccessToken();
      setAccessToken(newToken);
      processQueue(null, newToken);

      // Retry original request with new token
      return apiFetch<T>(url, {
        ...options,
        _retry: true,
        headers: { ...headers, Authorization: `Bearer ${newToken}` },
      });
    } catch (refreshError) {
      // Refresh failed — session fully expired
      processQueue(refreshError, null);
      clearAccessToken();

      // Redirect to login
      window.location.href = '/login?reason=session-expired';
      throw new Error('Session expired. Please log in again.');
    } finally {
      isRefreshing = false;
    }
  }

  // Other error (403, 404, 500) — throw as normal
  const errorData = await res.json().catch(() => ({})) as { message?: string };
  throw new Error(errorData.message || `HTTP ${res.status}`);
}
```

**✅ What the interviewer is checking:**
The `isRefreshing` flag and `failedQueue` pattern — this is
the production-grade solution. Without it, 10 simultaneous
API calls all get 401, all try to refresh, causing a race
condition. They also check for `_retry: true` to prevent
infinite retry loops.

**🔷 TypeScript lesson:**
`interface ExtendedRequestInit extends RequestInit` adds a
custom `_retry` field to the standard browser `RequestInit`.
The `QueuedRequest` interface types the resolve/reject functions
stored in the queue — making the queue type-safe and ensuring
both callbacks match their expected signatures.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Interview Pattern Picker

> **"Interviewer asks [X]" → "Go to Pattern [N]"**

| Interviewer Says | Go To |
|---|---|
| "Add pagination to this table" | Pattern 7 · Scenario 1 |
| "My pagination loses filters when I change pages" | Pattern 7 · Scenario 2 |
| "Load More is showing duplicate items" | Pattern 7 · Scenario 3 |
| "Implement infinite scroll" | Pattern 7 · Scenario 4 |
| "How do you type a paginated API response?" | Pattern 7 — `PaginatedResponse<T>` generic |
| "Show a success message after form submission" | Pattern 8 · Scenario 1 |
| "My toasts are stacking up and covering the screen" | Pattern 8 · Scenario 2 |
| "Make toast notifications available anywhere in the app" | Pattern 8 · Scenario 3 |
| "The toast always shows success even when the API fails" | Pattern 8 · Scenario 4 |
| "How do you type a toast system with multiple types?" | Pattern 8 — discriminated union `ToastType` |
| "Build a shopping cart with add/remove/quantity" | Pattern 9 · Scenario 1 |
| "The cart total is wrong when quantities change" | Pattern 9 · Scenario 2 |
| "Make the cart persist on page refresh" | Pattern 9 · Scenario 3 |
| "Prevent users from adding more items than available stock" | Pattern 9 · Scenario 4 |
| "How do you type a cart reducer with TypeScript?" | Pattern 9 — `CartAction` discriminated union |
| "Add a dark mode toggle that remembers the preference" | Pattern 10 · Scenario 1 |
| "There is a white flash before dark mode loads" | Pattern 10 · Scenario 1 |
| "Make the theme available in every component" | Pattern 10 · Scenario 2 |
| "Theme changes do not sync across browser tabs" | Pattern 10 · Scenario 3 |
| "Detect the user's system dark mode preference" | Pattern 10 · Scenario 4 |
| "Implement the complete login and logout flow" | Pattern 11 · Scenario 2 |
| "Where should you store JWT tokens?" | Pattern 11 · Scenario 1 |
| "User gets logged out on page refresh" | Pattern 11 · Scenario 2 |
| "How do you implement role-based access control?" | Pattern 11 · Scenario 3 |
| "Handle token expiry without logging the user out" | Pattern 11 · Scenario 4 |
| "How do you handle multiple simultaneous 401 errors?" | Pattern 11 · Scenario 4 — `isRefreshing` + `failedQueue` |
| "How do you type an auth context in TypeScript?" | Pattern 11 — `AuthContextType` with null guard hook |

---

> **Part 2 complete.**
> Say **"Generate Part 3 — Level 3, Patterns 12 to 14"** to continue.