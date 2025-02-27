---
title: Preloading
---

Preloading in TanStack Router is a way to load a route before the user actually navigates to it. This is useful for routes that are likely to be visited by the user, but not necessarily on the first page load.

**Preloading works by using hover and touch start events on `<Link>` components to preload the dependencies for the destination route.**

The simplest way to preload routes for your application is to set the `preload` option to `intent` for your entire router:

```tsx
import { Router } from '@tanstack/react-router'

const router = new Router({
  // ...
  preload: 'intent',
})
```

This will turn on preloading by default for all `<Link>` components in your application. You can also set the `preload` prop on individual `<Link>` components to override the default behavior.

## Preload Delay

By default, preloading will start after **50ms** of the user hovering or touching a `<Link>` component. You can change this delay by setting the `preloadDelay` option on your router:

```tsx
import { Router } from '@tanstack/react-router'

const router = new Router({
  // ...
  preloadDelay: 100,
})
```

You can also set the `preloadDelay` prop on individual `<Link>` components to override the default behavior on a per-link basis.

## Preloading with Data Loaders

Preloading is most useful when combined with the built-in loaders (or your favorite data loading library). To make this easier, the `loader` route option function receives a `preload` boolean denoting whether the route is being preloaded or not. **If you're using the built-in loader state, preload states are already handled for you automatically**. If that's not the case, then the `preload` flag allows you to load data differently depending on whether the user is navigating to the route or preloading it.

Here's a simple example of how you might use the `preload` flag with the built-in loader state:

```tsx
import { Route } from '@tanstack/react-router'

const postsRoute = new Route({
  getParentRoute: () => rootRoute,
  path: 'posts',
  loader: ({ preload }) => fetchPosts({ isPreload: preload }),
})
```

Not all data loading libraries will differentiate between preloading or not, but usually a preload method or option is available and can control different aspects of the actual data fetching or caching strategy of the data.

## Preloading Manually

If you need to manually preload a route, you can use the router's `preloadRoute` method. It accepts a standard TanStack `NavigateOptions` object and returns a promise that resolves when the route is preloaded.

```tsx
function Component() {
  const router = useRouter()

  useEffect(() => {
    try {
      const matches = await router.preloadRoute({
        to: postRoute,
        params: { id: 1 },
      })
    } catch (err) {
      // Failed to preload route
    }
  }, [])

  return <div />
}
```
