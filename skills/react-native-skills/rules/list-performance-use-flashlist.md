---
title: Use FlashList for Large Collections
impact: HIGH
impactDescription: Improves scroll smoothness for large lists
tags: react-native, list, flashlist
---

## Use FlashList for Large Collections

Prefer FlashList when rendering large lists or frequently updating feeds.

**Incorrect:**

```tsx
import { FlatList, Text } from "react-native"

export const Feed = ({ items }: { items: string[] }) => {
  return (
    <FlatList
      data={items}
      keyExtractor={(item) => item}
      renderItem={({ item }) => <Text>{item}</Text>}
    />
  )
}
```

**Correct:**

```tsx
import { FlashList } from "@shopify/flash-list"
import { Text } from "react-native"

export const Feed = ({ items }: { items: string[] }) => {
  return (
    <FlashList
      data={items}
      estimatedItemSize={44}
      keyExtractor={(item) => item}
      renderItem={({ item }) => <Text>{item}</Text>}
    />
  )
}
```

Reference:
[FlashList Docs](https://shopify.github.io/flash-list/)
