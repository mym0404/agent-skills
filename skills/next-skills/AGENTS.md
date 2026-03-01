# Next Skills

**Version 1.0.0**
Engineering
March 2026

---

## Abstract

Minimal Next.js guidance for server-first data fetching.

---

## 1. Data Fetching

### 1.1 Prefer Server-First Fetching

Fetch page data in Server Components before moving to client-side fetching.

**Incorrect:**

```tsx
"use client"

import { useEffect, useState } from "react"

export default function ProductsPage() {
  const [items, setItems] = useState<string[]>([])

  useEffect(() => {
    fetch("/api/products").then(async (res) => {
      const data = (await res.json()) as { items: string[] }
      setItems(data.items)
    })
  }, [])

  return <ul>{items.map((item) => <li key={item}>{item}</li>)}</ul>
}
```

**Correct:**

```tsx
export default async function ProductsPage() {
  const res = await fetch("https://example.com/api/products", {
    next: { revalidate: 60 },
  })
  const data = (await res.json()) as { items: string[] }

  return <ul>{data.items.map((item) => <li key={item}>{item}</li>)}</ul>
}
```

Reference:
[Next.js Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching)
