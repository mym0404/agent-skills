# Electron Skills

**Version 1.0.0**
Engineering
March 2026

---

## Abstract

Type-safe Electron IPC architecture with centralized API types, helper functions, and a structured 5-step process for adding new IPC endpoints. Ensures compile-time safety across main, preload, and renderer process boundaries.

---

## 1. IPC Architecture

### 1.1 Type-Safe IPC Architecture

End-to-end type-safe Electron IPC using a central `API` type, helper functions (`createIpcInvoker`, `registerIpcMainHandle`), and a consistent 5-step wiring process.

Do not use raw `ipcRenderer.invoke` or `ipcMain.handle` directly. All IPC signatures must go through the central `API` type.

#### Central API type

```typescript
// src/domain/api/API.ts

type UserAPI = {
  getUser: (params: { id: string }) => Promise<User>;
  saveUser: (params: { user: Omit<User, "id" | "createdAt"> }) => Promise<void>;
  deleteUser: (params: { id: string }) => Promise<void>;
};

export type API = {
  user: UserAPI;
  dialog: DialogAPI;
};

export type APIFunction<T extends keyof API, K extends keyof API[T]> = API[T][K];

export const createIpcInvoker = <T extends keyof API, K extends keyof API[T]>(
  channel: string,
): APIFunction<T, K> => {
  return ((...args: unknown[]) =>
    ipcRenderer.invoke(channel, ...args)) as APIFunction<T, K>;
};

export const registerIpcMainHandle = <T extends keyof API, K extends keyof API[T]>(
  channel: string,
  handler: APIFunction<T, K>,
): void => {
  ipcMain.handle(channel, async (_event, ...args: unknown[]) => {
    return await (handler as any)(...args);
  });
};
```

#### Preload bridge

```typescript
// src/preload/bridges/userBridge.ts
export const userBridge = {
  getUser:    createIpcInvoker<"user", "getUser">("app:user-get"),
  saveUser:   createIpcInvoker<"user", "saveUser">("app:user-save"),
  deleteUser: createIpcInvoker<"user", "deleteUser">("app:user-delete"),
};

// src/preload/index.ts
const api = { user: userBridge, dialog: dialogBridge };
contextBridge.exposeInMainWorld("api", api);
```

#### Main process handlers

```typescript
// src/main/feature/user/index.ts
export const getUser: APIFunction<"user", "getUser"> = async ({ id }) => {
  return db.findUser(id);
};

// src/main/feature/user/ipcHandlers.ts
export const registerUserIpcHandlers = () => {
  registerIpcMainHandle<"user", "getUser">("app:user-get", getUser);
  registerIpcMainHandle<"user", "saveUser">("app:user-save", saveUser);
};
```

#### 5-step checklist for new APIs

| Step | File | Action |
|------|------|--------|
| 1 | `src/domain/api/API.ts` | Add domain type to `API` |
| 2 | `src/main/feature/{domain}/index.ts` | Implement with `APIFunction` |
| 3 | `src/main/feature/{domain}/ipcHandlers.ts` | Register with `registerIpcMainHandle` |
| 4 | `src/preload/bridges/{domain}Bridge.ts` | Create with `createIpcInvoker` |
| 5 | `src/preload/index.ts` | Add bridge to `api` object |

#### Key rules

- Single object parameter for every IPC method: `(params: { ... })`
- Every method returns `Promise<T>`
- Channel naming: `{app-prefix}:{domain}-{action}`
- Business logic (`index.ts`) separate from IPC wiring (`ipcHandlers.ts`)
- Never use raw `ipcRenderer`/`ipcMain` — always use helpers

### 1.2 Structured Error Handling Across IPC Boundary

Main process throws structured errors with `throwAppError`, renderer catches and routes by error code with `catchAppError`. Errors survive IPC serialization via JSON encoding inside `AppErrorClass`.

#### Error definition

```typescript
// src/domain/types/AppError.ts
export class AppErrorClass extends Error {}

export type AppErrorCode = "user_not_found" | "validation_failed" | "duplicate_entry" | "unknown";

export const appErrorMessages: Record<AppErrorCode, string> = {
  user_not_found: "User not found.",
  validation_failed: "Data validation failed.",
  duplicate_entry: "A record with this identifier already exists.",
  unknown: "An unknown error occurred.",
};
```

#### Main process — throw

```typescript
throwAppError("user_not_found");

throwAppError({
  code: "validation_failed",
  payload: { field: "email", value: input.email },
});
```

#### Renderer — catch by code

```typescript
catchAppError(e, {
  validation_failed: ({ payload }) => {
    setErrorField(payload.field);
    showErrorToast("Validation failed");
  },
  DEFAULT: ({ code, message }) => {
    showErrorToast(`${code}: ${message}`);
  },
});
```

#### IPC handler integration

`registerIpcMainHandle` preserves `AppErrorClass` instances across the IPC boundary and re-wraps unstructured errors.

#### Key rules

- Every known error condition gets an `AppErrorCode` entry
- Main process uses `throwAppError` — never raw `throw new Error`
- Renderer uses `catchAppError` with code handlers + `DEFAULT` fallback
- Add new codes to `AppErrorCode` union and `appErrorMessages` record together

Reference:
- [Electron IPC Tutorial](https://www.electronjs.org/docs/latest/tutorial/ipc)
- [Electron Context Bridge](https://www.electronjs.org/docs/latest/api/context-bridge)
- [Electron Process Model](https://www.electronjs.org/docs/latest/tutorial/process-model)
