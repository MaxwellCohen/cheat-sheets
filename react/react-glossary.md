# React Glossary

Definitions of React-specific terms and phrases, drawn from the [React documentation](https://react.dev) (including [react.dev/llms.txt](https://react.dev/llms.txt) and linked docs).

---

## A

**Action** — (1) **Reducer action:** A plain JavaScript object you pass to `dispatch()` to describe what the user did. By convention it has a `type` (e.g. `'added'`, `'deleted'`) and any extra fields needed. The reducer uses actions to compute the next state. (2) **Transition Action:** The function passed to `startTransition`. React calls it immediately and marks all state updates scheduled synchronously during the call as **Transitions**. The callback can be sync or async; state updates after `await` must be wrapped in another `startTransition`. By convention, callbacks used with `startTransition` are named `action` or include the "Action" suffix (e.g. `submitAction`). [Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer), [useTransition](https://react.dev/reference/react/useTransition)

---

## B

**Batching** — React waits until all code in event handlers has run before processing state updates. So multiple `setState` calls in one event result in a single re-render. Batching makes the app faster and avoids “half-finished” renders. React does not batch across separate events (e.g. two clicks). [Queueing a Series of State Updates](https://react.dev/learn/queueing-a-series-of-state-updates)

---

## C

**Child component** — A component that is rendered inside another component. The outer one is the **parent component**. [Your First Component](https://react.dev/learn/your-first-component)

**Children** — A special prop: when you nest content inside a JSX tag, the parent receives that content in a prop named `children`. Used for wrappers (e.g. panels, cards) that render arbitrary nested JSX. [Passing Props to a Component](https://react.dev/learn/passing-props-to-a-component#passing-jsx-as-children)

**Client Component** — A component that runs in the browser and can use interactive APIs like `useState`, event handlers, and browser APIs. Marked with the `"use client"` directive. Composed with Server Components to add interactivity. [Server Components](https://react.dev/reference/rsc/server-components), ['use client'](https://react.dev/reference/rsc/use-client)

**Commit** — The phase after React has rendered your components when it applies changes to the DOM. For the initial render, React creates DOM nodes; for re-renders, it applies the minimal updates so the DOM matches the latest render output. React sets `ref.current` for DOM refs during commit (not during render). [Render and Commit](https://react.dev/learn/render-and-commit), [Manipulating the DOM with Refs](https://react.dev/learn/manipulating-the-dom-with-refs)

**Component** — A piece of the UI with its own logic and appearance. In React, a component is a JavaScript function that returns markup (JSX). Components can be nested. Names must start with a capital letter. [Quick Start](https://react.dev/learn#creating-and-nesting-components)

**Context** — A way for a parent to make data available to any component in the tree below it without passing props through every level. You create context with `createContext()`, provide a value with a **context provider**, and read it with `useContext()`. Use for theming, current user, routing, etc. [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)

**Conditional rendering** — Showing different JSX based on conditions. Use JavaScript `if`, the ternary operator `cond ? a : b`, or `cond && jsx` inside your component. [Conditional Rendering](https://react.dev/learn/conditional-rendering)

**Context Provider** — A component that wraps children and supplies the value for a context. Any descendant can read that value via `useContext()`. [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)

**createContext** — React API to create a new context. You pass a default value; components that are not inside a provider get this value from `useContext()`. [createContext](https://react.dev/reference/react/createContext)

**createRoot** — React DOM API that creates a **React root** for a browser DOM node. Call `createRoot(domNode)`, then `root.render(<App />)` to display your app. Use one root for a full React app, or multiple roots for “sprinkles” of React. For server-rendered HTML, use `hydrateRoot` instead. [createRoot](https://react.dev/reference/react-dom/client/createRoot)

**Custom Hook** — A JavaScript function whose name starts with `use` and that calls other Hooks. Used to reuse stateful logic between components. Must follow the Rules of Hooks. [Reusing Logic with Custom Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)

---

## D

**Dependency array** — The second argument to `useEffect`. It lists values (e.g. props, state) the Effect depends on. React re-runs the Effect only when one of these values changes. An empty array `[]` means “run only on mount.” [Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects)

**Dispatch** — The function returned by `useReducer`. You call it with an **action** object; React passes the current state and that action to the reducer and updates state with the returned value. [useReducer](https://react.dev/reference/react/useReducer), [Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

---

## E

**Effect** — In React docs, a side effect that is caused by **rendering** (e.g. connecting to a server when a component appears), not by a specific event. Declared with `useEffect`. Runs after **commit** so you can sync with external systems (network, DOM, third-party libs). [Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects)

**Effect cleanup** — A function returned from the function you pass to `useEffect`. React calls it before running the Effect again and when the component unmounts. Use it to disconnect, unsubscribe, or undo what the Effect did. [Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects)

**Effect Event** — A function created with `useEffectEvent` that contains non-reactive logic. Effect Events can be called inside Effects and always read the latest props and state when invoked, without being listed in the Effect’s dependency array. Use to separate event-like logic from Effects (e.g. logging a visit when `url` changes without re-running when other values change). [useEffectEvent](https://react.dev/reference/react/useEffectEvent), [Separating Events from Effects](https://react.dev/learn/separating-events-from-effects)

**Escape hatch** — A React feature that lets you “step outside” the normal declarative model. Refs and Effects are escape hatches: refs for values that don’t drive the UI, Effects for syncing with non-React systems. Use sparingly. [Escape Hatches](https://react.dev/learn/escape-hatches)

**Event handler** — A function you pass to a JSX attribute like `onClick` or `onChange`. React calls it when the user interacts (e.g. click, input). Handlers can update state, call APIs, or navigate; they are where you intentionally cause side effects in response to user actions. [Responding to Events](https://react.dev/learn/responding-to-events)

**External store** — A data source outside React that changes over time (e.g. a third-party state library, or a browser API like `navigator.onLine`). Subscribe to it in components with `useSyncExternalStore` so React can re-render when the store changes and stay in sync with concurrent rendering and SSR. [useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore)

**Initial render** — The first time React renders your app. Triggered by calling `createRoot` and then `render()` with your root component. [Render and Commit](https://react.dev/learn/render-and-commit)

---

## F

**flushSync** — A React DOM API that forces React to flush updates to the DOM synchronously inside the callback. Rare; use when you need to read the DOM immediately after a state update (e.g. scroll to a newly added list item). [flushSync](https://react.dev/reference/react-dom/flushSync), [Manipulating the DOM with Refs](https://react.dev/learn/manipulating-the-dom-with-refs)

**Fragment** — `<>...</>` or `<Fragment>`. Lets you group JSX children without adding an extra DOM node. Use `<Fragment key={...}>` when you need a key (e.g. in a list). [Fragment](https://react.dev/reference/react/Fragment)

---

## H

**Hook** — A function whose name starts with `use` (e.g. `useState`, `useEffect`). Hooks let you use React features (state, context, effects, etc.) from function components and custom Hooks. They can only be called at the top level of a component or custom Hook (not in conditions, loops, or nested functions). [State: A Component's Memory](https://react.dev/learn/state-a-components-memory), [Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks)

---

## J

**JSX** — The markup syntax used in React. It looks like HTML but is stricter (e.g. tags must be closed). You can embed JavaScript expressions in curly braces `{}`. Components return JSX. [Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx)

---

## K

**Key** — A string or number you pass to list items (e.g. `key={item.id}`) so React can correctly match items across re-renders when the list changes (reorder, add, remove). Keys should be stable and unique among siblings. You can also use keys on any component to **reset state** when the key changes (e.g. `key={userId}` on a form). Keys are not passed as props. [Rendering Lists](https://react.dev/learn/rendering-lists), [Preserving and Resetting State](https://react.dev/learn/preserving-and-resetting-state)

---

## L

**Lifting state up** — Moving state from a child component into a shared parent so multiple children can read and update the same state. The parent passes the value and updater (e.g. setter or dispatch) down as props. [Sharing State Between Components](https://react.dev/learn/sharing-state-between-components)

---

## M

**memo** — A higher-order function that wraps a component so React skips re-rendering it when its props are unchanged (shallow comparison). Use as a performance optimization, not a guarantee. Often used with `useCallback` and `useMemo` for stable props. [memo](https://react.dev/reference/react/memo)

**Memoized component** — A component wrapped in `memo`. React will not always re-render it when the parent re-renders if the props are the same. [memo](https://react.dev/reference/react/memo)

**Mount** — When a component is first added to the screen. Effects with an empty dependency array run when the component mounts. [Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects)

**Mutation** — Changing an existing object or variable. In React, you must not mutate props, state, or context during render; that breaks **purity**. Updating state via the setter (or dispatch) is not mutation from React’s perspective. [Keeping Components Pure](https://react.dev/learn/keeping-components-pure)

---

## O

**Optimistic update** — Updating the UI immediately with the expected result of an async action (e.g. showing a new message as “Sending…”) before the action completes. If the action fails, you revert. The `useOptimistic` Hook helps implement this pattern. [useOptimistic](https://react.dev/reference/react/useOptimistic)

---

## P

**Prop drilling** — Passing props through many layers of components that don’t use them, only to reach a deep child. Context is one way to avoid prop drilling. [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)

**Props** — Inputs to a component, passed like HTML attributes in JSX. React components receive a single `props` object (often destructured). Props are **immutable**: a component cannot change its own props; the parent must pass new props. [Passing Props to a Component](https://react.dev/learn/passing-props-to-a-component)

**Parent component** — A component that renders another component (the **child**). Parent passes props to child and often owns state that the child displays or updates. [Your First Component](https://react.dev/learn/your-first-component)

**Preserving state** — React keeps a component’s state as long as the same component is rendered at the same position in the **render tree**. If you remove the component or render a different component at that position, state is destroyed. [Preserving and Resetting State](https://react.dev/learn/preserving-and-resetting-state)

**Purity / Pure component** — A component that, for the same props/state/context, always returns the same JSX and does not change preexisting variables or objects during render. React assumes components are pure; it enables safe optimization and features like Suspense. [Keeping Components Pure](https://react.dev/learn/keeping-components-pure)

---

## R

**React node** — What you can pass to `root.render()`: a piece of JSX, the result of `createElement()`, a string, a number, `null`, or `undefined`. [createRoot](https://react.dev/reference/react-dom/client/createRoot)

**React root** — The top-level container created by `createRoot(domNode)`. React manages all DOM inside that node. You call `root.render()` to display a component tree and `root.unmount()` to destroy it. [createRoot](https://react.dev/reference/react-dom/client/createRoot)

**Reducer** — A function of the form `(state, action) => nextState`. It centralizes update logic: given the current state and an **action**, it returns the next state. Used with `useReducer`. Reducers must be pure and not run side effects. [Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

**Ref** — A mutable value that persists between renders and does **not** trigger a re-render when changed. Created with `useRef(initialValue)`; the value lives on `ref.current`. Use for DOM nodes, timeout/interval IDs, or any value you need to keep but not display. Refs are an **escape hatch**. [Referencing Values with Refs](https://react.dev/learn/referencing-values-with-refs)

**Ref callback** — A function you pass to the `ref` attribute instead of a ref object. React calls it with the DOM node when the element is mounted and with `null` (or calls the cleanup you return) when it is unmounted. Used to manage a list of refs (e.g. in a Map keyed by id). [Manipulating the DOM with Refs](https://react.dev/learn/manipulating-the-dom-with-refs), [ref callback](https://react.dev/reference/react-dom/components/common#ref-callback)

**Render tree** — The tree of components React builds from your UI. React ties state to positions in this tree; same component at the same position keeps state, different component or position resets it. [Preserving and Resetting State](https://react.dev/learn/preserving-and-resetting-state)

**Resetting state** — Forcing a component to lose its state and remount. Give it a different **key** when you want a “new” instance (e.g. `key={userId}` on a form), or render it at a different position in the tree. [Preserving and Resetting State](https://react.dev/learn/preserving-and-resetting-state)

**root.render(reactNode)** — Method on a React root that displays the given **React node** (e.g. JSX) inside the root’s DOM node. Call it once for the initial app; calling it again updates the UI. [createRoot](https://react.dev/reference/react-dom/client/createRoot)

**root.unmount()** — Method on a React root that destroys the rendered tree and detaches React from the DOM node. Use when the root’s container is removed from the page. [createRoot](https://react.dev/reference/react-dom/client/createRoot)

**Root component** — The top-level component you pass to `root.render()`, e.g. `<App />`. Your app’s component tree starts here. [Your First Component](https://react.dev/learn/your-first-component), [createRoot](https://react.dev/reference/react-dom/client/createRoot)

**Render** — When React calls your component functions to compute what should be on screen. Rendering should be a **pure** calculation (same inputs → same output). The result is then **committed** to the DOM. [Render and Commit](https://react.dev/learn/render-and-commit)

**Re-render** — When React runs a component again, e.g. because its state or a parent’s state changed, or because a parent re-rendered. React may skip re-rendering a component if it detects the output is unchanged.

**Rules of Hooks** — (1) Call Hooks only at the **top level** of a function component or custom Hook (not in conditions, loops, or after early returns). (2) Call Hooks only from React function components or from custom Hooks. Breaking these rules can cause bugs. [Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks)

---

## S

**Server Component** — A component that runs on the server (or at build time) before bundling. It is not sent to the client, so it cannot use `useState` or other client-only APIs. Used for data fetching and rendering static or server-driven content; interactivity is added by composing with **Client Components**. [Server Components](https://react.dev/reference/rsc/server-components)

**State** — Component “memory”: data that persists between renders and, when updated (via the state setter), triggers a re-render. Declared with `useState` or `useReducer`. State is private to the component instance. [State: A Component's Memory](https://react.dev/learn/state-a-components-memory)

**State as a snapshot** — During a single render, state is fixed; it doesn’t change until the next render. So in an event handler, `number` is always the value from that render even after you call `setNumber`. Setting state only schedules an update for the *next* render. [State as a Snapshot](https://react.dev/learn/state-as-a-snapshot)

**State setter** — The function returned by `useState`, e.g. `setCount`. Calling it with a new value (or with an **updater function**) updates state and queues a re-render.

**State variable** — The current value returned by `useState`, e.g. `count`. React keeps this value across re-renders until you update it with the state setter.

**startTransition** — A standalone React API (not a Hook) that marks state updates inside the given callback as **Transitions**. Use when you need to start a Transition from outside a component (e.g. from a data library). Same behavior as the `startTransition` returned by `useTransition`, but does not provide `isPending`. [startTransition](https://react.dev/reference/react/startTransition)

**Strict Mode** — A development-only mode (e.g. `<StrictMode>`) where React double-invokes components and some logic to help find impure code and missing Effect cleanup. [StrictMode](https://react.dev/reference/react/StrictMode)

**Suspend / Suspending** — When a component that reads a resource (e.g. via `use(Promise)`) is not yet ready to render because the resource is still loading. React “suspends” that part of the tree and shows the nearest **Suspense** fallback until the resource is ready. [Suspense](https://react.dev/reference/react/Suspense), [use](https://react.dev/reference/react/use)

**Suspense** — A built-in component that lets you show a fallback (e.g. a loading UI) while children are loading or suspending. Used with lazy loading and async data. [Suspense](https://react.dev/reference/react/Suspense)

---

## T

**Transition** — A state update marked as non-urgent. React keeps the current UI visible and responsive while applying the update in the background; when ready, it commits the new UI. Use `useTransition` (or the standalone `startTransition`) to wrap updates you want to be non-blocking. Transitions are interruptible and avoid hiding already-revealed content with loading indicators. [useTransition](https://react.dev/reference/react/useTransition)

---

## U

**Unmount** — When a component is removed from the screen. React runs the **cleanup** of any Effects when a component unmounts.

**Updater function** — A function you pass to the state setter, e.g. `setNumber(n => n + 1)`. React queues it and, on the next render, calls it with the previous state and uses the return value as the new state. Use when you need to update state multiple times in one event or when the next state depends on the previous one. Must be pure. [Queueing a Series of State Updates](https://react.dev/learn/queueing-a-series-of-state-updates)

**use** — A React API that reads a **resource** (a Promise or a **context**). When called with a Promise, the component **suspends** until the Promise resolves; use with **Suspense** and Error Boundaries. Unlike Hooks, `use` can be called inside conditionals and loops. [use](https://react.dev/reference/react/use)

**useActionState** — A Hook that updates state based on the result of a **form action**. You pass an action function and initial state; it returns `[state, formAction, isPending]`. The returned action is passed to `<form action={...}>` or a button’s `formAction`. With Server Components, form state can be shown before hydration. Formerly `useFormState` in React DOM. [useActionState](https://react.dev/reference/react/useActionState)

**useCallback** — A Hook that returns a memoized function. Used when you need to pass a stable function reference to a child that is optimized (e.g. with `memo`). [useCallback](https://react.dev/reference/react/useCallback)

**useContext** — A Hook that reads and subscribes to a **context** value. Pass the context object (from `createContext()`); it returns the value from the nearest provider (or the default). [useContext](https://react.dev/reference/react/useContext)

**useDebugValue** — A Hook that adds a custom label for a **custom Hook** in React DevTools. Call it at the top level of your custom Hook with a value (and optionally a formatting function). Most useful for shared libraries. [useDebugValue](https://react.dev/reference/react/useDebugValue)

**useDeferredValue** — A Hook that defers updating a part of the UI. You pass a value; it returns a version that may “lag behind” during updates. React first re-renders with the old deferred value, then re-renders in the background with the new value. Use to keep the UI responsive (e.g. keep input snappy while a heavy list updates) or to show stale content until new content loads (with Suspense). [useDeferredValue](https://react.dev/reference/react/useDeferredValue)

**useEffect** — A Hook that lets you run code (and optionally return a **cleanup** function) after render. Used to synchronize with external systems. You pass a function and optionally a **dependency array**. [useEffect](https://react.dev/reference/react/useEffect), [Synchronizing with Effects](https://react.dev/learn/synchronizing-with-effects)

**useEffectEvent** — A Hook that declares an **Effect Event**: a function you can call inside Effects that always reads the latest props and state and does not need to be in the dependency array. Use to extract non-reactive logic from Effects (e.g. logging a visit when `url` changes without re-running when other values change). [useEffectEvent](https://react.dev/reference/react/useEffectEvent), [Separating Events from Effects](https://react.dev/learn/separating-events-from-effects)

**useFormStatus** — A React DOM Hook that returns status information for the parent `<form>` submission: `pending`, `data`, `method`, `action`. Must be called from a component rendered *inside* a form (e.g. a submit button). Use to show “Submitting…” or disable the button while the form is pending. [useFormStatus](https://react.dev/reference/react-dom/hooks/useFormStatus)

**useId** — A Hook that generates a unique ID string stable across server and client. Use for accessibility attributes (e.g. `aria-describedby`, `id`/`htmlFor` pairs). Do not use for list **keys** or cache keys. [useId](https://react.dev/reference/react/useId)

**useImperativeHandle** — A Hook that customizes what a parent sees when it uses a ref on your component. You expose a limited API (e.g. only `focus()`) instead of the full DOM node. Used with `forwardRef`. [useImperativeHandle](https://react.dev/reference/react/useImperativeHandle), [Manipulating the DOM with Refs](https://react.dev/learn/manipulating-the-dom-with-refs)

**useLayoutEffect** — A version of `useEffect` that runs synchronously after all DOM mutations but before the browser repaints. Use when you need to measure the DOM or update it before paint (e.g. positioning a tooltip). Prefer `useEffect` when possible, since `useLayoutEffect` blocks painting. [useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)

**useMemo** — A Hook that memoizes the result of a computation. React will reuse the value until a dependency changes. Used to avoid expensive recalculations on every render. [useMemo](https://react.dev/reference/react/useMemo)

**useOptimistic** — A Hook that lets you show **optimistic** state while an async action is in progress. You pass the real state and an update function; it returns `[optimisticState, addOptimistic]`. Call `addOptimistic(optimisticValue)` when starting the action; the UI shows the optimistic state until the action completes. [useOptimistic](https://react.dev/reference/react/useOptimistic)

**useReducer** — A Hook for state that is updated via a **reducer** and **actions**. Returns `[state, dispatch]`. Useful when updates depend on previous state or when many event handlers share the same update logic. [useReducer](https://react.dev/reference/react/useReducer)

**useRef** — A Hook that returns a mutable ref object with a `current` property. Persists across re-renders; updating `current` does not cause a re-render. Use for DOM nodes (pass ref to the `ref` attribute), timers, or any non-rendering value. [useRef](https://react.dev/reference/react/useRef), [Referencing Values with Refs](https://react.dev/learn/referencing-values-with-refs)

**useState** — A Hook that declares a **state variable** and a **state setter**. Returns `[value, setValue]`. The setter can take a new value or an updater function `(prev) => next`. [useState](https://react.dev/reference/react/useState), [State: A Component's Memory](https://react.dev/learn/state-a-components-memory)

**useSyncExternalStore** — A Hook that subscribes to an **external store** (e.g. a third-party state library or a browser API like `navigator.onLine`). You pass `subscribe`, `getSnapshot`, and optionally `getServerSnapshot`. Returns the current snapshot. Use when integrating with non-React state; supports concurrent rendering and server rendering. [useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore)

**useTransition** — A Hook that marks some state updates as **Transitions**. Returns `[isPending, startTransition]`. Wrap updates in `startTransition(callback)` so they are non-blocking; React keeps the current UI visible and applies the update in the background. Use for tab switches, navigation, or heavy updates that shouldn’t block the UI. [useTransition](https://react.dev/reference/react/useTransition)

---

## References

- [React Documentation](https://react.dev)
- [React Documentation (LLMs)](https://react.dev/llms.txt)
- [Learn React](https://react.dev/learn)
- [API Reference](https://react.dev/reference/react)
