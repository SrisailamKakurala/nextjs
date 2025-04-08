# nextjs
repo for learning nextjs

---

# Cons of ReactJS
- no built-in routing
- no compiler
- no SEO
- no SSR
- no SSG
- no Full-Stack
- no API-Integration
- no Structure

# Pros of NEXTJS
- Hot reloading
- built-in compiler
- SSR & SSG
- SEO Friendly
- Full-Stack
- API Integration
- Built-in Routing
- Performance

---

# Creating a NextJS APP

```bash
npx create-next-app@latest my-app
```

---

## 🧠 `use client` vs `use server` in Next.js

### 🔹 `use client`

- Used **at the top of a file** to tell Next.js it should run **in the browser**.
- Enables usage of **state**, **event handlers**, **hooks** (like `useState`, `useEffect`).
- Makes the component **interactive** (e.g., buttons, inputs, modals).

**Use it when:**
- You need to handle **user interactions**
- You’re using **React hooks**
- You need to **update UI without page reload**

```tsx
'use client'

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>
}
```

---

### 🔸 `use server` (default behavior)

- Next.js **automatically runs components on the server** unless `use client` is added.
- Can **fetch data**, access **env variables**, or **call databases**.
- Doesn’t support hooks like `useState`.

**Use it when:**
- You’re rendering static content
- You’re calling a database (e.g., via Prisma)
- You don’t need interactivity

```tsx
export default async function Page() {
  const users = await prisma.user.findMany()

  return <pre>{JSON.stringify(users, null, 2)}</pre>
}
```

---

### 🧠 TL;DR:

| Use This       | When You Need                             |
|----------------|--------------------------------------------|
| `use client`   | State, interactivity, event handling       |
| Server (default)| Data fetching, static content, DB access  |

---

### 🧭 `useRouter()` in Next.js (Client Side Routing)

`useRouter()` is a **React hook** from `next/navigation` (in App Router) or `next/router` (in Pages Router) used for **navigating** between pages and **accessing route info**.

---

### 📦 Common Methods

#### `router.push(url)`
- Navigates to a new route.
- Adds a **new entry** to browser history.
- 🔁 **Back button** works.

```js
router.push('/dashboard')
```

---

#### `router.replace(url)`
- Navigates to a new route.
- **Replaces** the current history entry.
- 🚫 **Back button** doesn't go back to previous page.

```js
router.replace('/login')
```

---

#### `router.back()`
- Goes back to the previous page (like browser back).

---

#### `router.refresh()`
- Reloads the current route (in App Router).

---

### 🔍 Other Properties

- `router.pathname` → Current route path
- `router.query` → Route parameters (`/user/[id]`)
- `router.asPath` → Full path including query

---

### ✅ Use `push` when:
- You want user to **go back** later (normal navigation)

### 🚫 Use `replace` when:
- You don’t want to keep the current page in history  
  (e.g., after login, form submission, etc.)

---

### 🧠 `useRouter()` vs `Link` in Next.js

#### 🧭 `useRouter()`
- A **hook** used in **JavaScript logic** (inside components).
- Useful for **programmatic navigation**, conditional routing, or side-effects after actions.

**Use When:**
- Navigating **after a button click**, form submit, or custom logic.
- Need access to route details (`pathname`, `query`, etc.).

```js
const router = useRouter();
const handleClick = () => {
  router.push('/dashboard');
};
```

---

#### 🔗 `Link`
- A **component** used in **JSX/HTML**.
- Optimized for **static, declarative navigation** (like `<a>` tags).

**Use When:**
- You just want to **render a navigation link**.
- In menus, buttons, or anywhere routing is triggered by user clicks **without logic**.

```jsx
<Link href="/dashboard">Go to Dashboard</Link>
```

---

### ✅ Summary:
| Situation | Use |
|----------|-----|
| Declarative/static navigation | `<Link />` |
| Dynamic/programmatic routing | `useRouter()` |

---

Here’s a breakdown of **all folder naming conventions** used in **Next.js App Router** (especially with `app/` directory structure):

---

### 📁 1. `folderName/`
- A **regular route segment**.
- Represents a path in the URL.

**Example:**
```
app/dashboard/page.tsx → /dashboard
```

---

### 📁 2. `[folderName]/`
- A **dynamic segment**.
- Matches any value in that part of the URL.

**Example:**
```
app/user/[id]/page.tsx → /user/123
```
Use `useParams()` to get `id`.

---

### 📁 3. `[[folderName]]/`
- An **optional catch-all** route.
- Matches zero or more segments.

**Example:**
```
app/blog/[[...slug]]/page.tsx → /blog or /blog/a/b
```

---

### 📁 4. `(folderName)/`
- A **grouping folder**.
- Does **not** affect the route path.
- Helps **organize** components/pages without changing the URL structure.

**Example:**
```
app/(auth)/login/page.tsx → /login
```

---

### 📁 5. `_folderName/`
- **Not special to routing.**
- It's a **custom dev convention**, often used to mark:
  - private folders,
  - temp files,
  - ignored during route generation.

_Not handled specially by Next.js — you can name folders this way, but it won’t affect routing._

---

### 📁 6. `@folderName/`
- Not a route feature.
- Commonly used with **module aliasing** in `tsconfig.json` or `webpack`.

---

### 📁 7. `route.ts`, `layout.tsx`, `page.tsx`
- Not folders, but important:
  - `page.tsx` → actual page component.
  - `layout.tsx` → shared layout for child pages.
  - `route.ts` / `route.js` → for **API routes** inside the `app/` directory.

---

### 📁 8. `...folderName/`
- **Catch-all segment** (non-optional).
- Matches multiple segments but **must** have at least one.

**Example:**
```
app/docs/[...slug]/page.tsx → /docs/a or /docs/a/b/c
```

---

### ✅ Summary Table:

| Syntax           | Purpose                          | Example URL            |
|------------------|----------------------------------|-------------------------|
| `folder/`         | Static segment                   | `/about`               |
| `[param]/`        | Dynamic segment                  | `/user/123`            |
| `[[...param]]/`   | Optional catch-all               | `/blog`, `/blog/a/b`   |
| `[...param]/`     | Catch-all (required)             | `/docs/a/b`            |
| `(group)/`        | Layout/route grouping            | `/login` from `(auth)` |
| `_private/`       | Dev convention (ignored by Next) | N/A                    |

---

### 🔄 Dynamic Routes in Next.js (App Router)

---

#### 📌 What Are Dynamic Routes?

Dynamic routes let you create pages where part of the URL can **change based on data** (e.g., user ID, slug, etc.).

---

### 🧩 Syntax & Examples

#### 1. `[param]`
Matches a **single dynamic segment**.

**File Structure:**
```
app/blog/[slug]/page.tsx
```

**URL Example:**
```
/blog/my-first-post
```

**Accessing Param:**
```ts
import { useParams } from 'next/navigation';

const { slug } = useParams(); // { slug: 'my-first-post' }
```

---

#### 2. `[...param]` (Catch-all Route)
Matches **multiple segments** (at least one).

**File Structure:**
```
app/docs/[...slug]/page.tsx
```

**URL Examples:**
```
/docs/a
/docs/a/b/c
```

**Accessing Param:**
```ts
const { slug } = useParams(); // ['a', 'b', 'c']
```

---

#### 3. `[[...param]]` (Optional Catch-all)
Same as `[...param]`, but can match **zero** segments too.

**File Structure:**
```
app/docs/[[...slug]]/page.tsx
```

**URL Examples:**
```
/docs
/docs/a
/docs/a/b
```

**Output:**
```ts
// /docs → { slug: undefined }
// /docs/a → { slug: ['a'] }
```

---

### ✅ Use Cases

- `[id]` → `/user/123` (user profile)
- `[slug]` → `/blog/how-to-use-nextjs` (blog post)
- `[...slug]` → `/docs/a/b/c` (deep docs)
- `[[...slug]]` → `/docs` (optional deep docs)

---

### catch-all route

`[x]` matches **a single dynamic segment** (e.g. `/user/123` → `x = 123`)  
`[...x]` matches **multiple segments as an array** (e.g. `/docs/a/b` → `x = ['a', 'b']`)  
The `...` tells Next.js it's a **catch-all route** for deep nesting.

- `[x]` captures a **single dynamic segment**, like `/post/123`.  
- `[...x]` captures **multiple segments**, like `/docs/a/b/c`, as an array `['a', 'b', 'c']`.  
- `[...x]` is called a **catch-all route**.  
- Useful for nested content, breadcrumbs, or fallback pages.  
- You can access the segments using `router.query.x` in a page.  
- Using `[x]` instead of `[...x]` for multi-segment paths leads to a 404 error.

---

### 📐 Layouts in Next.js (App Router) – From Basic to Production-Level

---

#### 🧱 **What is a Layout?**
In Next.js App Router (since `app/` structure), a **layout** is a special React component defined in `layout.tsx` (or `.js`) inside a route folder. It **wraps the page and persists across nested routes**, making it perfect for navigation bars, sidebars, or persistent UI.

---

#### 🔍 **How Layouts Work Internally**
- **Layouts are automatically detected** by their filename `layout.tsx`.
- When a page (like `page.tsx`) exists in the same folder, Next.js **automatically renders it inside the layout**, even if `children` aren’t manually passed in the page.
- The framework **injects `children` into the layout** under the hood via server components logic.

So this works automatically:

```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex">
      <Sidebar />
      <main>{children}</main> // ← injected automatically
    </div>
  );
}
```

And this will be rendered:

```tsx
// app/dashboard/page.tsx
export default function DashboardPage() {
  return <div>Dashboard Content</div>;
}
```

You don’t need to manually wrap it around `children` like in `_app.js` from the old Pages Router.

---

#### 🧠 Min vs Max Complexity

| Level | Description |
|-------|-------------|
| **Minimal** | Just a single `layout.tsx` with `children`, no reusability |
| **Intermediate** | Nested layouts (e.g., `app/(marketing)/layout.tsx` and `app/(dashboard)/layout.tsx`) for sectioned UI |
| **Advanced (Prod)** | Shared layouts across routes using **route groups**, **dynamic layouts**, **context providers**, **auth guards**, and layout-specific `metadata` setup |

---

#### 🔁 Layout Inheritance
- Layouts are **recursive**. If you nest folders, each folder’s layout will wrap its children unless overridden.
- Example: `app/layout.tsx` → wraps everything, but `app/dashboard/layout.tsx` can override it for dashboard-only styling.

---

#### 💡 Route Groups + Layouts
Use `(group)` folders when you want **different layouts without affecting the URL path**.

Example:

```
app
├── (marketing)
│   └── layout.tsx
│   └── home/page.tsx
├── (dashboard)
│   └── layout.tsx
│   └── analytics/page.tsx
```

URLs will be `/home`, `/analytics` — no `(marketing)` or `(dashboard)` in URL.

---

#### ✅ Summary
- Layouts auto-detect and inject `children`—you don’t need to manually do it.
- They persist across navigation unless explicitly replaced.
- Use route groups for layout separation without changing routes.
- Production setups use nested + grouped + protected layouts with context and metadata.

---

### 🧩 Templates in Next.js (App Router) – Explained Simply

---

#### 📌 What is a Template?

A **template** in Next.js is just like a **layout**, but instead of persisting between route changes, it **re-renders from scratch every time** the route changes.

---

#### 📂 Where do we use it?

You create it in the `template.tsx` file inside any route folder:

```tsx
// app/products/template.tsx
export default function ProductTemplate({ children }: { children: React.ReactNode }) {
  return (
    <div className="p-4 bg-white shadow">
      <Header />
      {children} {/* This will rerender on every route inside /products */}
    </div>
  );
}
```

---

#### 🆚 Layout vs Template (in simple terms)

| Feature | `layout.tsx` | `template.tsx` |
|--------|--------------|----------------|
| Persists across routes | ✅ Yes | ❌ No |
| Re-renders on route change | ❌ No | ✅ Yes |
| Good for | Sidebar, nav, wrappers | Per-page transitions, modals, animations |
| Execution | Only once | Every time route loads |

---

#### 🧠 When to use which?

- Use **`layout.tsx`** when you want consistent UI (like a dashboard sidebar).
- Use **`template.tsx`** when you want fresh re-renders (like product previews, dynamic steps, etc).

---

So, **layouts are cached and persistent**, while **templates are fresh and route-specific**.


---

✅ **Yes**, `not-found.tsx` **must be lowercase** — it's a special file that Next.js **auto-detects by name**, and it only works if named exactly `not-found.tsx` in lowercase.

---

In Next.js (App Router), just like `not-found.tsx`, the **`loading.tsx`** and **`error.tsx`** files are:

- ✅ **Automatically recognized**
- ✅ **Must be lowercase**
- ✅ **Must be placed inside route segments**

### 🔄 `loading.tsx`
- Used for **route-level loading UI**
- Automatically shown when a route or its data is loading

### ❌ `error.tsx`
- Catches and displays errors thrown during:
  - Rendering
  - Data fetching (e.g., `fetch`, `getServerSideProps`, etc.)
- Works like an **error boundary**

> 📁 Example directory:
```
app/
  dashboard/
    page.tsx
    loading.tsx
    error.tsx
    not-found.tsx
```

Each route can have its own versions of these for scoped UX.

---

