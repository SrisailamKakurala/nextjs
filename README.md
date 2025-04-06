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

