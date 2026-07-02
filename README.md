# Polymorph.ts 

> **Polymorph.ts** is a secure, zero-trust specification and runtime framework for cross-game save states and behavioral scripts. Author natively in **TypeScript**, sandbox execution with **Deno**, and render fluidly in 3D using **Babylon.js**.

Traditional cross-game saves transfer static integers and strings. **Polymorph.ts** transfers *capabilities*. It allows game characters, inventory items, and spell logic to carry their actual execution scripts into completely different games, adapting gracefully to the host engine's capabilities without crashing.

---

## 🛠 The Tech Stack

*   **Language:** TypeScript (Strict type safety across shared schemas).
*   **Backend/Runtime:** Deno (OS-level sandboxing via permissionless sub-workers).
*   **Frontend/Graphics:** Babylon.js (AAA-grade WebGL/WebGPU 3D rendering engine).

---

## 🧬 Core Concepts

### 1. Polymorphic Adaptation (Feature Detection)
Instead of relying on rigid versioning, incoming scripts query the host engine for available capabilities. If a script looks for a magic system but finds a space simulator, it adapts its behavior downstream rather than crashing.

### 2. Zero-Trust Execution
Never trust the client. Player scripts are compiled to JavaScript on upload and executed inside a locked-down Deno Worker (`permissions: { net: false, read: false, write: false }`). Communication happens entirely via a strict, asynchronous message-passing bridge.

### 3. Shared Extensible Schema
A single unified data structure acts as the source of truth, allowing developers to append new game namespaces seamlessly without breaking backward compatibility for older titles.

---

## 🚀 Quick Architecture Overview

### The Shared Schema (`@polymorph/schema`)
```typescript
export interface CoreEntity {
  id: string;
  name: string;
  scriptSource: string; // The sandboxed TypeScript/JS behavior code
}

export interface SharedSave {
  version: string;
  entity: CoreEntity;
  gameData: Record<string, unknown>; // Extensible game-specific namespaces
}
