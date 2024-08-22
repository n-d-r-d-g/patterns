# Classes in JavaScript/TypeScript

I rarely use classes in front-end JS/TS code and I don't see other developers make use of this pattern very often.

So, here's a use case I came across where classes can help make DX better in React.

## Original code (without classes)

When using [React Router](https://reactrouter.com), you need to specify your app's routes with `createBrowserRouter`. Let's take an example where the app currently contains 3 routes, namely `/`, `/about` and `/contact`.

src/main.tsx:

```tsx
const routes = [
  {
    path: "/",
    element: <HomePage />,
    errorElement: <ErrorBoundary />,
  },
  {
    path: "/about",
    element: <AboutPage />,
    errorElement: <ErrorBoundary />,
  },
  {
    path: "/contact",
    element: <ContactPage />,
    errorElement: <ErrorBoundary />,
  },
];

const router = createBrowserRouter(routes);
```

You may have noticed that in the code block above, each object in the `routes` array contains an `errorElement` property with the same value, i.e. `<ErrorBoundary />`. As the app grows, more routes might be added and every time developers need to add a route, they will need to specify this property. This goes against the DRY principle, hence making the code less maintainable.

## New code (with classes)

Using classes along with TypeScript basically solves this issue:

src/classes/Route.ts:

```tsx
export class Route {
  routeObject: RouteObject;

  constructor({ errorElement, ...route }: RouteObject) {
    this.routeObject = route;

    if (typeof errorElement === "undefined") {
      this.routeObject.errorElement = <ErrorPage />;
    } else if (errorElement === null) {
      this.routeObject.errorElement = undefined;
    } else if (errorElement) {
      this.routeObject.errorElement = errorElement;
    }
  }
}
```

src/classes/Routes.ts:

```tsx
export class Routes {
  routeObjects: Array<RouteObject> = [];

  constructor(...newRoutes: Array<Route>) {
    this.routeObjects = newRoutes.map((route) => route.routeObject);
  }
}
```

src/main.jsx:

```tsx
const routes = new Routes(
  new Route({ path: "/", element: <HomePage /> }),
  new Route({ path: "/about", element: <AboutPage /> }),
  new Route({ path: "/contact", element: <ContactPage /> })
);
const routeObjects = routes.routeObjects;

const router = createBrowserRouter(routeObjects);
```

Now, the code is organized such that developers are less prone to forget adding the `errorElement` property as it is automatically done by the `Route` class during instantiation.
