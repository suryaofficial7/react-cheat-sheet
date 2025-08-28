Perfect 👍 You want me to convert this **React Intermediate** notes (15–30) into the same **`.md` structured style** I gave for React Basics.

Here’s the **full markdown version**:

````markdown
# 📘 React Intermediate

---

## 15. useEffect Hook (Side Effects)
- Runs after render.  
- Used for **side effects**: API calls, subscriptions, timers, etc.  

```jsx
import { useEffect, useState } from "react";

function Timer() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    const interval = setInterval(() => setCount(c => c + 1), 1000);
    return () => clearInterval(interval); // cleanup
  }, []);
  return <p>Seconds: {count}</p>;
}
````

---

## 16. useRef Hook

* Stores mutable values **without re-rendering**.

```jsx
import { useRef } from "react";

function InputFocus() {
  const inputRef = useRef(null);
  return (
    <>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
    </>
  );
}
```

---

## 17. useContext Hook (Global State)

```jsx
import { createContext, useContext } from "react";

const ThemeContext = createContext("light");

function Child() {
  const theme = useContext(ThemeContext);
  return <p>Theme is {theme}</p>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}
```

---

## 18. Component Lifecycle (Class Components)

```jsx
class LifecycleDemo extends React.Component {
  componentDidMount() { console.log("Mounted"); }
  componentDidUpdate() { console.log("Updated"); }
  componentWillUnmount() { console.log("Unmounted"); }
  render() { return <h1>Lifecycle Example</h1>; }
}
```

---

## 19. Lifting State Up

* Share state between components by **moving it to their common parent**.

```jsx
function Parent() {
  const [text, setText] = useState("");
  return (
    <>
      <ChildInput text={text} setText={setText} />
      <ChildDisplay text={text} />
    </>
  );
}

function ChildInput({ text, setText }) {
  return <input value={text} onChange={(e) => setText(e.target.value)} />;
}

function ChildDisplay({ text }) {
  return <p>{text}</p>;
}
```

---

## 20. React Fragments

```jsx
function FragmentDemo() {
  return (
    <>
      <h1>Title</h1>
      <p>Paragraph</p>
    </>
  ); // <> is shorthand for <React.Fragment>
}
```

---

## 21. React Router (v6+)

### a) Routing Basics

```bash
npm install react-router-dom
```

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function RoutingExample() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<h1>Home</h1>} />
        <Route path="/about" element={<h1>About</h1>} />
      </Routes>
    </BrowserRouter>
  );
}
```

### b) Nested Routes

```jsx
function Dashboard() {
  return (
    <Routes>
      <Route path="profile" element={<h1>Profile</h1>} />
      <Route path="settings" element={<h1>Settings</h1>} />
    </Routes>
  );
}
```

### c) Dynamic Routing (useParams)

```jsx
import { useParams } from "react-router-dom";

function User() {
  const { id } = useParams();
  return <h1>User ID: {id}</h1>;
}
// <Route path="/user/:id" element={<User />} />
```

### d) Redirects & Navigate

```jsx
import { Navigate } from "react-router-dom";

function RedirectExample() {
  return <Navigate to="/about" />;
}
```

---

## 22. Fetching API Data (fetch / axios)

```jsx
import axios from "axios";
import { useEffect, useState } from "react";

function FetchData() {
  const [data, setData] = useState([]);
  useEffect(() => {
    axios.get("https://jsonplaceholder.typicode.com/posts")
      .then(res => setData(res.data));
  }, []);
  return <ul>{data.slice(0,5).map(post => <li key={post.id}>{post.title}</li>)}</ul>;
}
```

---

## 23. Custom Hooks

```jsx
function useLocalStorage(key, initial) {
  const [value, setValue] = useState(() => localStorage.getItem(key) || initial);
  useEffect(() => { localStorage.setItem(key, value); }, [value]);
  return [value, setValue];
}

function CustomHookDemo() {
  const [name, setName] = useLocalStorage("name", "");
  return <input value={name} onChange={e => setName(e.target.value)} />;
}
```

---

## 24. Error Boundaries (Class Only)

```jsx
class ErrorBoundary extends React.Component {
  constructor() { super(); this.state = { hasError: false }; }
  static getDerivedStateFromError() { return { hasError: true }; }
  componentDidCatch(error, info) { console.log(error, info); }
  render() { return this.state.hasError ? <h1>Error!</h1> : this.props.children; }
}
```

---

## 25. Conditional Component Rendering

* Similar to conditional rendering but can switch entire components.

```jsx
function Conditional({ show }) {
  return show ? <CompA /> : <CompB />;
}
```

---

## 26. React.memo & useMemo

* **React.memo** → Memoizes components.
* **useMemo** → Memoizes values.

```jsx
const MemoComp = React.memo(function({ value }) {
  console.log("Rendered!");
  return <p>{value}</p>;
});

function MemoDemo({ num }) {
  const squared = React.useMemo(() => num * num, [num]);
  return <p>{squared}</p>;
}
```

---

## 27. useCallback Hook

* Memoizes functions to prevent unnecessary re-renders.

```jsx
function CallbackDemo() {
  const [count, setCount] = useState(0);
  const increment = React.useCallback(() => setCount(c => c + 1), []);
  return <button onClick={increment}>{count}</button>;
}
```

---

## 28. Controlled vs Uncontrolled Components

* **Controlled →** React manages state
* **Uncontrolled →** DOM manages state via refs

```jsx
function ControlledInput() {
  const [value, setValue] = useState("");
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

function UncontrolledInput() {
  const inputRef = useRef();
  return (
    <>
      <input ref={inputRef} />
      <button onClick={() => alert(inputRef.current.value)}>Show</button>
    </>
  );
}
```

---

## 29. Portals in React

* Render children into a DOM node **outside the parent hierarchy**.

```jsx
import ReactDOM from "react-dom";

function Modal({ children }) {
  return ReactDOM.createPortal(children, document.getElementById("modal-root"));
}
```

---

## 30. Handling Side Effects

* Any external effect (API, logging, DOM manipulation).
* Usually handled inside **useEffect with cleanup**.

```jsx
useEffect(() => {
  console.log("Subscribed!");
  return () => console.log("Unsubscribed!");
}, []);
```

---

Here’s your **React Performance & Data Fetching** notes (1–4) formatted in the same clean **`.md` style** for study or documentation:

````markdown
# 📙 React Performance & Data Fetching

---

## 1. React Query / TanStack Query
- Library for **data fetching, caching, synchronization**, and **updating server state**.  
- Reduces manual state management, handles caching, and background updates.

```jsx
import { useQuery } from "@tanstack/react-query";
import axios from "axios";

function FetchPosts() {
  const { data, error, isLoading } = useQuery({
    queryKey: ["posts"],
    queryFn: () => axios.get("https://jsonplaceholder.typicode.com/posts").then(res => res.data),
  });

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error fetching data</p>;

  return (
    <ul>
      {data.slice(0, 5).map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
````

**Output:** Renders first 5 posts from API dynamically with caching and automatic refetch.

---

## 2. Memoization

* Caches results to avoid expensive re-calculations or re-renders.
* **React.memo()** → Memoizes components
* **useMemo()** → Memoizes values
* **useCallback()** → Memoizes functions

```jsx
import { useMemo, useState } from "react";

function ExpensiveCalc({ num }) {
  const squared = useMemo(() => {
    console.log("Calculating...");
    return num * num;
  }, [num]);

  return <p>Squared: {squared}</p>;
}
```

**Output:** "Calculating..." logs only when `num` changes.

---

## 3. Lazy Loading

* Loads components **asynchronously** to reduce initial bundle size.

```jsx
import React, { Suspense, lazy } from "react";

const LazyComponent = lazy(() => import("./LazyComponent"));

function App() {
  return (
    <Suspense fallback={<p>Loading Component...</p>}>
      <LazyComponent />
    </Suspense>
  );
}
```

**Output:** Shows "Loading Component..." until `LazyComponent` is fetched and rendered.

---

## 4. Virtualization

* Only renders **visible items** in a long list for performance (e.g., 1000+ items).
* Libraries: `react-window`, `react-virtualized`

```jsx
import { FixedSizeList as List } from "react-window";

const Row = ({ index, style }) => (
  <div style={style}>Row #{index}</div>
);

function VirtualList() {
  return (
    <List height={150} itemCount={1000} itemSize={30} width={300}>
      {Row}
    </List>
  );
}
```

**Output:** Only visible rows render; scrolling dynamically loads rows.
**Performance improvement:** Reduces DOM nodes drastically for huge lists.

---

