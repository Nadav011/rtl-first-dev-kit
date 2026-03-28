# Next.js Proxy And Direction

Use `proxy.ts` in Next.js 16, not `middleware.ts`, when steering locale or direction at the edge.

## `proxy.ts` Example

```ts
import { NextResponse, type NextRequest } from 'next/server';

const RTL_LOCALES = new Set(['he', 'ar']);

export function proxy(request: NextRequest) {
  const locale = request.cookies.get('locale')?.value ?? 'he';
  const response = NextResponse.next();
  response.headers.set('x-locale', locale);
  response.headers.set('x-dir', RTL_LOCALES.has(locale) ? 'rtl' : 'ltr');
  return response;
}
```

## Layout Propagation

Read the headers once at the layout boundary and set `lang` + `dir` there:

```tsx
import { headers } from 'next/headers';

export default async function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const headerStore = await headers();
  const locale = headerStore.get('x-locale') ?? 'he';
  const dir = headerStore.get('x-dir') ?? 'rtl';

  return (
    <html lang={locale} dir={dir}>
      <body>{children}</body>
    </html>
  );
}
```

## Rules

- Set `dir` once at the document layout, not ad hoc per component.
- Keep locale and direction derivation deterministic.
- When a route depends on `cookies()` or `headers()`, treat it as dynamic and cache accordingly.
- Component code should still use logical utilities even when the layout already has `dir=\"rtl\"`.
