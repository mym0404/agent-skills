# React Native Skills

**Version 1.0.0**
Engineering
March 2026

---

## Abstract

Minimal React Native guidance for performant large lists.

---

## 1. List Performance

### 1.1 Use FlashList for Large Collections

Use FlashList for large or frequently updated lists to reduce frame drops.

**Incorrect:**

```tsx
import { FlatList, Text } from "react-native"

export const Messages = ({ items }: { items: string[] }) => {
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

export const Messages = ({ items }: { items: string[] }) => {
  return (
    <FlashList
      data={items}
      estimatedItemSize={48}
      keyExtractor={(item) => item}
      renderItem={({ item }) => <Text>{item}</Text>}
    />
  )
}
```

Reference:
[FlashList Docs](https://shopify.github.io/flash-list/)
