# Render functions in React

This pattern can be used whenever a child component needs to expose data to its parent component.

In the following example, a custom `Tooltip` component is created to wrap text. The `Tooltip` should be displayed only if the text is truncated.

The component should be able to wrap nested elements and target the specified one to check whether it's truncated or not.

**`src/hooks/useTruncatedText.tsx`**:

This hook accepts a `ref` to the element whose text needs to be checked for truncation and the actual `text` to trigger the check every time the text changes.

`useLayoutEffect` is used in favor of `useEffect` to avoid rendering glitches.

A generic type that extends `HTMLElement` limits the `ref` type to only HTML elements.

```tsx
import { JSXElementConstructor, ReactElement, RefObject, useLayoutEffect, useState } from 'react';

export const useTruncatedText = <T extends HTMLElement>(
  ref: RefObject<T>,
  text?: TooltipProps['title']
) => {
  const [isTextTruncated, setIsTextTruncated] = useState(false);

  useLayoutEffect(() => {
    if (ref.current) {
      setIsTextTruncated(ref.current.scrollWidth > ref.current.clientWidth);
    }
  }, [ref, text]);

  return { isTextTruncated };
};

```

**`src/components/Tooltip.tsx`**:

The `children` prop type from `MUI`'s `Tooltip` component needs to be overridden to a render prop, i.e. a function that returns a `ReactElement`.

The `ref` is created in this component to avoid having to create one in every single component using `Tooltip`.

This component also accepts a generic type that extends `HTMLElement`, to ensure type safety.

```tsx
import { useRef } from 'react';
import { Tooltip as BaseTooltip, TooltipProps } from '@mui/material';
import { useTruncatedText } from '../hooks/useTruncatedText';

type Props<T extends HTMLElement> = Omit<TooltipProps, 'children'> & {
  children: (ref: RefObject<T>) => ReactElement;
};

export function Tooltip<T extends HTMLElement>({
  children,
  title,
  ...tooltipProps
}: Props<T>) {
  const ref = useRef<T>(null);
  const { isTextTruncated } = useTruncatedText<T>(ref, title);

  return (
    <Tooltip
      title={isTextTruncated && title}
      disableHoverListener={!isTextTruncated}
      {...tooltipProps}
    >
      {children(ref)}
    </Tooltip>
  );
}
```

**`src/_app.tsx`**:

When using `Tooltip`, we should specify the type argument as the type of the element we want to check for truncation.

```tsx
import { Tooltip } from './components/Tooltip';

function App() {
  const title = 'This text can be very long (i.e. it will be truncated) or short enough not to be truncated.';

  return (
    <main>
      <Tooltip<HTMLSpanElement> title={title}>
        {ref => (
          <h1>
            <span>Icon</span>
            <span ref={ref}>{title}</span>
          </h1>
        )}
      </Tooltip>
    </main>
  )
}
```

We could have only wrapped the `span` which has a `ref`:

```tsx
<main>
  <h1>
    <span>Icon</span>
    <Tooltip<HTMLSpanElement> title={title}>
      {ref => (
        <span ref={ref}>{title}</span>
      )}
    </Tooltip>
  </h1>
</main>
```

However, we might want the user to be able to hover on both the icon and the text inside the `<h1>` to trigger the tooltip.