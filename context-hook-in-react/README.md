# Context & a custom hook in React

This pattern shows how to share and update state using React Context via a custom provider and hook.

**`src/AppContext.tsx`**:

This file exports the provider, i.e. AppProvider, and the custom hook, i.e. useApp.

```tsx
import {
  Dispatch,
  PropsWithChildren,
  SetStateAction,
  createContext,
  useContext,
  useState,
} from "react";

type TAppContext = {
  count: number;
  setCount: Dispatch<SetStateAction<number>>;
};

const AppContext = createContext<TAppContext | undefined>(undefined);

export function AppProvider({ children }: PropsWithChildren) {
  const [count, setCount] = useState(0);

  return (
    <AppContext.Provider value={{ count, setCount }}>
      {children}
    </AppContext.Provider>
  );
}

export function useApp() {
  const ctx = useContext(AppContext);

  if (!ctx) throw new Error("useApp must be used within an AppProvider!");

  return ctx!;
}
```

**`src/_app.tsx`**:

```tsx
import { AppProvider } from "./AppContext";
import { Child } from "./Child";

function App() {
  return (
    <AppProvider>
      <Child />
    </AppProvider>
  );
}
```

**`src/Child.tsx`**:

```tsx
import { useApp } from "./AppContext";

function Child() {
  const { count, setCount } = useApp();

  const incrementCount = () => setCount((prevCount) => ++prevCount);

  return (
    <>
      <p>{count}</p>
      <button onClick={incrementCount}>Increment count</button>
    </>
  );
}
```
