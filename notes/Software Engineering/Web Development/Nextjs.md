# Nextjs : `React`, `Server Side Rendering`, `Client Side Rendering`

This is a `React` framework for full stack web applications.

## Routers

There are two routers, `App Router` which is the newer router that supports Server Components and the original `pages Router`.

### app Router

If you are using the App router, you'll have a folder called _app_ at your top level or underneath _src_.

## Routing Files

There are important files used by NextJS that sit inside the router. They will automatically be picked up by NextJS and used for their specific purpose. i.e. your not-found.tsx will be used automatically if the user goes to a route that is not defined.

- layout
- page
- loading
- not-found
- error
- global-error
- route
- template
- default

## Public Folder

If you have a public folder in the root, then this can be used to store static assets to be referenced in the code from the base URL /.

```typescript
import Image from "next/image";

export default function Page() {
  return <Image src="/profile.png" alt="Profile" width={100} height={100} />;
}
```
