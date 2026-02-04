# React Concepts 

```mermaid
flowchart TB
    %% Invisible links force 2x3 grid: row1 = Debugging, Optimizations, Escape | row2 = Sync, Data, Async

    subgraph Debugging["🔍 Debugging"]
        direction TB
        useDebugValue["useDebugValue - better debugging in dev tools"]
        Profiler["&lt;Profiler&gt; - measure render cost"]
    end

    subgraph Optimizations["⚡ Optimizations"]
        direction TB
        useMemo["useMemo - memoize values"]
        useCallback["useCallback - stable refs"]
        reactCompiler["react compiler - runtime optimize"]
        Activity["&lt;Activity&gt; - delay render"]
        Fragment["&lt;Fragment&gt; - group subparts"]
    end

    subgraph EscapeHatches["🚪 Escape hatches"]
        direction TB
        useLayoutEffect["useLayoutEffect - styles on render"]
        useEffect["useEffect - effects on state"]
        useEffectEvent["useEffectEvent - events vs Effects"]
        useRef["useRef - stable ref or DOM"]
        useImperativeHandle["useImperativeHandle - customize ref"]
        useInsertionEffect["useInsertionEffect - CSS-in-JS"]
        useId["useId - stable id SSR/client"]
    end

    subgraph SyncState["🔄 Sync State"]
        direction TB
        createContext["createContext - provider"]
        useContext["useContext - consume context"]
        useState["useState - reactive value"]
        useReducer["useReducer - reducer state"]
        useSyncExternalStore["useSyncExternalStore - external store"]
    end

    subgraph DataLoading["📥 Data loading"]
        direction TB
        Suspense["&lt;Suspense&gt; - wait for data"]
        use["use - promises and context"]
    end

    subgraph AsyncReact["⏳ Async React"]
        direction TB
        formAction["&lt;form action&gt; - native actions"]
        startTransition["startTransition - non-urgent fn"]
        useTransition["useTransition - non-urgent hook"]
        useActionState["useActionState - form/async reducer"]
        useDeferredValue["useDeferredValue - defer UI"]
        useOptimistic["useOptimistic - shadow during transition"]
        ViewTransition["&lt;ViewTransition&gt; - animate"]
    end

    %% Layout: two rows of three
    Debugging ~~~ Optimizations ~~~ EscapeHatches
    SyncState ~~~ DataLoading ~~~ AsyncReact
    EscapeHatches ~~~ SyncState

    %% Debugging ↔ Optimizations
    Profiler -->|measures| useMemo
    Profiler -->|measures| useCallback

    %% Escape hatches ↔ Sync State
    useEffect -->|reads/writes| useState
    useEffect -->|reads/writes| useReducer
    useLayoutEffect -->|reads/writes| useState
    useRef -->|persists across renders like| useState
    useImperativeHandle -->|wraps| useRef

    %% Escape hatches ↔ Optimizations
    useEffectEvent -->|used inside| useEffect
    useMemo -->|returns stable ref like| useRef

    %% Sync State internal
    createContext -->|provides to| useContext
    useState -->|simpler than| useReducer
    useSyncExternalStore -->|reactive value like| useState

    %% Sync State ↔ Data loading
    useContext -->|same source as| use
    use -->|loaded value into| useState

    %% Data loading ↔ Async React
    Suspense -->|boundary for| use
    use -->|unwraps promises from| formAction
    useTransition -->|wraps| startTransition
    useActionState -->|used with| formAction
    useOptimistic -->|used during| useTransition
    useDeferredValue -->|pairs with| useTransition
    ViewTransition -->|animates| useTransition
```








