# Types of React Components in Next.js App Router

## Summary

Next.js App Router introduces a new paradigm for React components, distinguishing between **Server Components** and **Client Components**. By default, all components in the App Router are Server Components, which run on the server and can directly access backend resources. Client Components (marked with `'use client'`) run in the browser and enable interactivity. Additionally, Server Components can be async to fetch data directly, and Server Actions (marked with `'use server'`) allow client components to invoke server-side functions.

## Component Types Comparison

| Component Type | Directive | code executes | Can Use Hooks | Can Be Async | Data Fetching | Interactivity | Use Case |
|----------------|-----------|---------------|---------------|--------------|---------------|---------------|----------|
| **Class Component** | 'use client' | client and server | ❌ | ❌ | Lifecycle methods | ✅ | Error boundaries |
| **Functional Component (Server)** | None (default) | Server | ❌ | ✅ | Direct fetch/DB | ❌ | Static content, data fetching |
| **Client Component** | `'use client'` | client and server | ✅ | ❌ | useEffect/fetch | ✅ | Interactive UI, state management |
| **Async Server Component** | None (default) | Server | ❌ | ✅ | Direct fetch/DB | ❌ | Server-side data fetching |
| **Server Action** | `'use server'` | Server | ❌ | ✅ | Direct fetch/DB | ❌ | Form submissions, mutations |

## Class Components

Class components are not commonly used in modern React development. Their main use case is for **Error Boundaries**, which require the `componentDidCatch` lifecycle method that doesn't have a functional component equivalent.

### Characteristics
- Must extend `React.Component`
- Use lifecycle methods (`componentDidMount`, `componentDidUpdate`, etc.)
- Access props via `this.props`
- Manage state via `this.state`
- Cannot be async
- Always run on the client

### Example: Error Boundary

```tsx
"use client"
class ErrorBoundary extends React.Component<
  { children: React.ReactNode; fallback: React.ReactNode },
  { hasError: boolean }
> {
  constructor(props: { children: React.ReactNode; fallback: React.ReactNode }) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <>{this.props.fallback}</>;
    }
    return <>{this.props.children}</>;
  }
}
```

## Functional Components (Server Components)

In Next.js App Router, **all components are Server Components by default**. They run on the server during the build process or request time, allowing direct access to backend resources like databases, file systems, and environment variables.

### Characteristics
- No directive needed (default behavior)
- Run on the server
- Cannot use React hooks (`useState`, `useEffect`, etc.)
- Cannot use browser-only APIs
- Can directly access backend resources
- Smaller bundle size (code sent to browsers is JSON representation of the component)
- Can be async

### Example

```tsx
// app/components/UserProfile.tsx
import { db } from '@/lib/db';

export default function UserProfile({ userId }: { userId: string }) {
  // Direct database access - no API route needed!
  const user = db.users.find(userId);
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

## Client Components

Client Components are marked with the `'use client'` directive and run in the browser. They enable interactivity, state management, and access to browser APIs.

### Characteristics
- Must have `'use client'` directive at the top
- Code may run on the server (SSR) as well as in the browser during hydration, so browser APIs should be accessed with care:
  - Wrap any browser-only APIs (like `window` or `localStorage`) with checks (e.g. `typeof window !== 'undefined'`) or use them inside `useEffect`
- Can use all React hooks
- Can use browser APIs (`window`, `localStorage`, etc.) with proper safeguards
- Cannot be async
- Can import and use Server Components (but not the other way around)
- Larger bundle size (code sent to client)

### Example

```tsx
'use client';

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### When to Use Client Components
- Interactive elements (buttons, forms, inputs)
- State management (`useState`, `useReducer`)
- Effects and side effects (`useEffect`)
- Browser APIs (`localStorage`, `window`, etc.)
- Event listeners
- Third-party libraries that require client-side JavaScript

## Async Server Components

Async Server Components are Server Components that can be declared as `async` and use `await` to fetch data directly. This is the recommended pattern for data fetching in Next.js App Router.

### Characteristics
- No directive needed (default Server Component)
- Must be declared as `async`
- Can use `await` for data fetching
- Run on the server
- Cannot use hooks
- Automatically handle loading states with Suspense

### Example

```tsx
// app/users/page.tsx
import { Suspense } from 'react';

async function getUsers() {
  const res = await fetch('https://api.example.com/users', {
    cache: 'no-store', // or 'force-cache' for static
  });
  return res.json();
}

export default async function UsersPage() {
  const users = await getUsers();
  
  return (
    <div>
      <h1>Users</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <ul>
          {users.map(user => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      </Suspense>
    </div>
  );
}
```

### Data Fetching Patterns

```tsx
// Direct database access
async function getPosts() {
  const posts = await db.posts.findMany();
  return posts;
}

// API route
async function getData() {
  const res = await fetch('https://api.example.com/data');
  return res.json();
}

// With caching
async function getCachedData() {
  const res = await fetch('https://api.example.com/data', {
    next: { revalidate: 3600 }, // Revalidate every hour
  });
  return res.json();
}
```

## Server Actions

Server Actions are functions marked with `'use server'` that can be called from Client Components. They run on the server and are perfect for form submissions, mutations, and server-side operations.

### Characteristics
- Must have `'use server'` directive
- Can be defined in separate files or at the top of component files
- Can be async
- Run on the server when invoked
- Can be called from Client Components
- Automatically handle form submissions with `action` prop
- Type-safe with TypeScript

### Example: Inline Server Action

```tsx
// app/components/AddUser.tsx
'use client';

import { useActionState } from 'react';

async function addUser(prevState: any, formData: FormData) {
  'use server';
  
  const name = formData.get('name') as string;
  // Direct database access
  await db.users.create({ name });
  
  return { success: true, message: 'User added!' };
}

export default function AddUser() {
  const [state, formAction] = useActionState(addUser, null);

  return (
    <form action={formAction}>
      <input name="name" type="text" />
      <button type="submit">Add User</button>
      {state?.message && <p>{state.message}</p>}
    </form>
  );
}
```

### Example: Separate Server Actions File

```tsx
// app/actions/user.ts
'use server';

import { db } from '@/lib/db';

export async function createUser(formData: FormData) {
  const name = formData.get('name') as string;
  const email = formData.get('email') as string;
  
  await db.users.create({ name, email });
  
  return { success: true };
}

export async function deleteUser(userId: string) {
  await db.users.delete(userId);
  return { success: true };
}
```

```tsx
// app/components/UserForm.tsx
'use client';

import { createUser } from '@/app/actions/user';

export default function UserForm() {
  return (
    <form action={createUser}>
      <input name="name" type="text" />
      <input name="email" type="email" />
      <button type="submit">Create User</button>
    </form>
  );
}
```

## Component Composition Rules

### ✅ Allowed
- Server Components can import and render other Server Components
- Client Components can import and render other Client Components
- Client Components can import and render Server Components (as children/props)
- Server Components can pass Server Components as props to Client Components

### ❌ Not Allowed
- Server Components cannot import Client Components directly
- Server Components cannot use hooks
- Client Components cannot be async
- Server Components cannot use browser-only APIs

### Example: Proper Composition

```tsx
// ✅ Server Component
// app/components/Layout.tsx
import ClientNavbar from './ClientNavbar'; // Client Component
import ServerSidebar from './ServerSidebar'; // Server Component

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div>
      <ClientNavbar>
        {/* Server Component passed as children */}
        <ServerSidebar />
      </ClientNavbar>
      {children}
    </div>
  );
}
```

## Best Practices

1. **Default to Server Components**: Start with Server Components and only add `'use client'` when you need interactivity or browser APIs.

2. **Keep Client Components Small**: Move logic to Server Components when possible to reduce bundle size.

3. **Use Async Server Components for Data Fetching**: Prefer async Server Components over `useEffect` + fetch in Client Components.

4. **Use Server Actions for Mutations**: Use Server Actions for form submissions and data mutations instead of API routes.

5. **Leverage Suspense**: Use Suspense boundaries with async Server Components for better loading states.

6. **Error Boundaries**: Use Class Components only for Error Boundaries when needed.
