---
title: Expose a Minimal Preload API
impact: CRITICAL
impactDescription: Reduce renderer privilege and attack surface
tags: electron, preload, context-bridge
---

## Expose a Minimal Preload API

Expose only specific functions to renderer code.

**Incorrect:**

```typescript
import { contextBridge, ipcRenderer } from "electron"

contextBridge.exposeInMainWorld("electron", { ipcRenderer })
```

**Correct:**

```typescript
import { contextBridge, ipcRenderer } from "electron"

contextBridge.exposeInMainWorld("electronAPI", {
  readVersion: () => ipcRenderer.invoke("app:version"),
})
```

Reference:
[Electron Security Tutorial](https://www.electronjs.org/docs/latest/tutorial/security)
