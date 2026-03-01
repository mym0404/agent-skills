# Electron Skills

**Version 1.0.0**
Engineering
March 2026

---

## Abstract

Minimal Electron security guidance focused on preload API boundaries.

---

## 1. Security Boundary

### 1.1 Expose a Minimal Preload API

Do not expose `ipcRenderer` directly to renderer code.

**Incorrect:**

```typescript
import { contextBridge, ipcRenderer } from "electron"

contextBridge.exposeInMainWorld("electron", {
  ipcRenderer,
})
```

**Correct:**

```typescript
import { contextBridge, ipcRenderer } from "electron"

contextBridge.exposeInMainWorld("electronAPI", {
  openFile: () => ipcRenderer.invoke("dialog:open-file"),
})
```

Reference:
[Electron Security Tutorial](https://www.electronjs.org/docs/latest/tutorial/security)
