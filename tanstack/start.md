# TanStack Start + TanStack Router Cheat Sheet

[TanStack Start](https://tanstack.com/start/latest/docs/framework/react/overview) is a full-stack React framework: SSR, streaming, and server functions, built on [TanStack Router](https://tanstack.com/router/latest/docs/framework/react/overview) for type-safe routing, URLs, and data loading. This sheet is React-focused (`@tanstack/react-start`, `@tanstack/react-router`). Router behavior is the same inside Start; Start adds the runtime, bundler plugin, and server-function RPC layer.

## Installation

Scaffold from the official quickstart when you can — it wires Vite, the Start plugin, and route generation together:

- [TanStack Start — Quick Start](https://tanstack.com/start/latest/docs/framework/react/quickstart)

Typical packages:

```bash
npm install @tanstack/react-router @tanstack/react-start
```

Vite config uses `@tanstack/react-start/plugin/vite` (see the quickstart). The Router bundler plugin / CLI generates `routeTree.gen.ts` from files under `src/routes`.

## Mental model

1. **Route files** — You add files under `src/routes/` (default). Naming maps to URL segments, dynamic params (`$postId`), splats (`$.tsx`), index routes (`index.tsx` / `posts.index.tsx`), and layout routes (`posts.tsx`, `posts/route.tsx`, or `posts.route.tsx` — same URL; see [file-based routing API](https://tanstack.com/router/latest/docs/api/file-based-routing)).
2. **Generated tree** — Dev/build runs the generator and produces `src/routeTree.gen.ts` (default path). Import `routeTree` from there into `router.tsx`.
3. **Router instance** — `createRouter({ routeTree, ... })` inside an exported `getRouter()` in `src/router.tsx` (Start expects a fresh instance per request/server usage as documented).
4. **Route modules** — Each file exports `Route` from `createRootRoute` (root only) or `createFileRoute` (all other files). The string passed to `createFileRoute('/exact/path')` is **maintained by tooling** when you create/move files — do not hand-edit it to match URLs.

## Project layout (Start)

```
src/
├── router.tsx           # export function getRouter()
├── routeTree.gen.ts     # generated — do not edit
└── routes/
    ├── __root.tsx       # root layout (document shell)
    ├── index.tsx        # /
    ├── about.tsx        # /about
    └── posts/
        ├── route.tsx    # layout for /posts (or posts.tsx at routes root)
        ├── index.tsx    # /posts/
        └── $postId.tsx  # /posts/:postId
```

Details: [Start — Routing](https://tanstack.com/start/latest/docs/framework/react/guide/routing).

## `router.tsx`

```tsx
import { createRouter } from '@tanstack/react-router'
import { routeTree } from './routeTree.gen'

export function getRouter() {
  return createRouter({
    routeTree,
    scrollRestoration: true,
    // defaultPreload, defaultStaleTime, etc. — see Router docs
  })
}
```

## Root route (`__root.tsx`)

The root route has **no path**; it always matches. Use it for the HTML shell, global providers, and `Outlet`.

```tsx
import {
  Outlet,
  createRootRoute,
  HeadContent,
  Scripts,
} from '@tanstack/react-router'
import type { ReactNode } from 'react'

export const Route = createRootRoute({
  head: () => ({
    meta: [
      { charSet: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { title: 'My App' },
    ],
  }),
  component: RootComponent,
})

function RootComponent() {
  return (
    <RootDocument>
      <Outlet />
    </RootDocument>
  )
}

function RootDocument({ children }: Readonly<{ children: ReactNode }>) {
  return (
    <html lang="en">
      <head>
        <HeadContent />
      </head>
      <body>
        {children}
        <Scripts />
      </body>
    </html>
  )
}
```

- **`HeadContent`** — Renders head tags from route `head()` (and related APIs).
- **`Scripts`** — Emits client script tags. **Include it** in the root document or hydration will break.
- **`Outlet`** — Renders the active child route; renders `null` if none.

## File → URL cheat table

| URL | Example file(s) | Notes |
| --- | --- | --- |
| `/` | `index.tsx` | Index route |
| `/about` | `about.tsx` | Static segment |
| `/posts` | `posts.tsx`, `posts.route.tsx`, or `posts/route.tsx` | Layout route (`routeToken` default: `route`) |
| `/posts/` | `posts/index.tsx` or `posts.index.tsx` | Index under `/posts` |
| `/posts/:postId` | `posts/$postId.tsx` | Dynamic param |
| `/files/*` | `files/$.tsx` | Splat / wildcard |

Param access in components: `Route.useParams()` (types flow from the file route).

## Co-location and ignored files

Default **`routeFileIgnorePrefix`** is `-`. Folders/files prefixed with `-` are **not** routes (good for `-components`, `-hooks`, etc.). See [file-based routing API](https://tanstack.com/router/latest/docs/api/file-based-routing).

## Pathless layouts, groups, non-nested

- **Pathless layout** — Underscore-prefixed segments in the file tree can define layouts that wrap children **without adding a URL segment** (e.g. a folder like `_auth/` with a layout route file and child routes). See [Routing concepts](https://tanstack.com/router/latest/docs/guide/routing-concepts) and the [file-based routing API](https://tanstack.com/router/latest/docs/api/file-based-routing).
- **Grouped routes** — Organize directories for clarity; consult the Router docs for how your chosen pattern affects the URL tree.
- **Non-nested routes** — Escape normal parent/child nesting when you need a different component tree; covered in Router routing guides.

## Defining a file route

```tsx
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/posts/$postId')({
  component: PostPage,
})
```

Add **loaders**, **beforeLoad**, **validateSearch**, **pendingComponent**, **errorComponent**, etc. on that same object as needed.

## Navigation

```tsx
import { Link, useNavigate } from '@tanstack/react-router'

<Link to="/posts/$postId" params={{ postId: '123' }} search={{ tab: 'comments' }}>
  Open post
</Link>

const navigate = useNavigate()
navigate({ to: '/posts', search: { sort: 'new' } })
```

Use **`to`** with route paths (not arbitrary href strings) for type safety when the route tree is generated.

## Search params (typed)

Validate search on the route so `useSearch` is typed and safe:

```tsx
import { createFileRoute } from '@tanstack/react-router'
import { z } from 'zod'

const searchSchema = z.object({
  page: z.coerce.number().optional().default(1),
})

export const Route = createFileRoute('/posts')({
  validateSearch: (search) => searchSchema.parse(search),
  component: PostsPage,
})

function PostsPage() {
  const { page } = Route.useSearch()
  // ...
}
```

More: [Router — Search params](https://tanstack.com/router/latest/docs/guide/search-params).

## Data loading

- **`beforeLoad`** — Runs top-down; use for auth gates, context that children need. Can return data merged into route context.
- **`loader`** — Runs after `beforeLoad`; sibling loaders can run in parallel. Good for data fetching (including calling server functions).

```tsx
export const Route = createFileRoute('/posts/$postId')({
  loader: async ({ params }) => {
    const post = await fetchPost(params.postId)
    return { post }
  },
  component: PostPage,
})

function PostPage() {
  const { post } = Route.useLoaderData()
  // or useLoaderData({ from: '/posts/$postId' }) for specificity
  return <article>{post.title}</article>
}
```

Overview: [Router — Data loading](https://tanstack.com/router/latest/docs/guide/data-loading).

## Errors, not-found, redirects

From **`@tanstack/react-router`**:

```tsx
import { redirect, notFound } from '@tanstack/react-router'

// In beforeLoad / loader / server function handler:
throw redirect({ to: '/login' })
throw notFound()
```

## Server functions (Start)

Define RPC-style server entry points with **`createServerFn`** from **`@tanstack/react-start`**. They run on the server; client bundles call them via generated stubs (network under the hood).

```tsx
import { createServerFn } from '@tanstack/react-start'

export const getMessage = createServerFn().handler(async () => {
  return { message: 'Hello from server' }
})

export const save = createServerFn({ method: 'POST' })
  .inputValidator((data: { name: string }) => data)
  .handler(async ({ data }) => {
    return { ok: true, name: data.name }
  })
```

Call from a **loader** or another server function with `await getMessage()`. In client components, use **`useServerFn(getMessage)`** and call the returned function (e.g. inside React Query `queryFn` or event handlers).

- **`inputValidator`** — Use Zod or a function; input arrives as `{ data }` in the handler.
- **Throws** — Errors serialize to the client; **`redirect`** / **`notFound`** work when thrown from handlers used in route lifecycles or via `useServerFn`.

### Request / response helpers (server)

```tsx
import { createServerFn } from '@tanstack/react-start'
import {
  getRequest,
  getRequestHeader,
  setResponseHeaders,
  setResponseStatus,
} from '@tanstack/react-start/server'

export const cached = createServerFn({ method: 'GET' }).handler(async () => {
  const req = getRequest()
  const auth = getRequestHeader('Authorization')
  setResponseHeaders(new Headers({ 'Cache-Control': 'public, max-age=300' }))
  setResponseStatus(200)
  return load()
})
```

Guide: [Start — Server Functions](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions).

### File organization (larger apps)

- **`.functions.ts`** — Export `createServerFn` wrappers; safe to import from client code.
- **`.server.ts`** — Server-only helpers (DB, secrets); import **only** inside server function handlers.
- Shared **types / Zod schemas** — Plain `.ts` modules usable on both sides.

## Common Router hooks and components

| API | Role |
| --- | --- |
| `Link` | Declarative navigation with typed `to` / `params` / `search` |
| `Outlet` | Render active child route |
| `useNavigate()` | Imperative navigation |
| `useRouter()` | Router instance (location, subscribe, etc.) |
| `Route.useParams()` / `Route.useSearch()` | Typed URL state for the current file route |
| `Route.useLoaderData()` | Loader data for that route |
| `useMatch()` / `useMatches()` | Inspect matched routes |

**Preloading** and **scroll restoration** are configured on `createRouter` / `Link` — see [Preloading](https://tanstack.com/router/latest/docs/guide/preloading) and Start’s routing guide.

## See also

- [TanStack Start — Overview](https://tanstack.com/start/latest/docs/framework/react/overview)
- [TanStack Start — Routing](https://tanstack.com/start/latest/docs/framework/react/guide/routing)
- [TanStack Start — Server Functions](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions)
- [TanStack Router — Overview](https://tanstack.com/router/latest/docs/framework/react/overview)
- [File-Based Routing API](https://tanstack.com/router/latest/docs/api/file-based-routing)
- [Router — Data loading](https://tanstack.com/router/latest/docs/guide/data-loading)
- [Router — Search params](https://tanstack.com/router/latest/docs/guide/search-params)
- [Router — Routing concepts](https://tanstack.com/router/latest/docs/guide/routing-concepts)
