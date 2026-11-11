# json-api-builder-create-files

[![npm version](https://img.shields.io/npm/v/json-api-builder-create-files?color=cb3837&style=flat-square)](https://www.npmjs.com/package/json-api-builder-create-files)
[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)
[![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen.svg?style=flat-square)](package.json)
[![GitHub Repository](https://img.shields.io/badge/GitHub-keshavsoft%2Fjson-api-builder-create-files-181717.svg?style=flat-square&logo=github)](https://github.com/keshavsoft/json-api-builder-create-files)
[![Documentation](https://img.shields.io/badge/docs-live_portal-amber.svg?style=flat-square)](https://keshavsoft.github.io/json-api-builder-create-files/)

> **Stage 2 of 8**: Stage 2: Non-Destructive Leaf File Provisioning for JSON API Builder

Part of the modular **[JSON API Builder](https://github.com/keshavsoft/json-api-builder-orchestrator)** ecosystem by [KeshavSoft](https://github.com/keshavsoft).

---

## The Story

Traverses tree.json to create empty placeholder files for all string-valued leaf entries. Strictly non-destructive: existing files are never overwritten, protecting custom client modifications.

### The Problem It Solves
Standard code generators indiscriminately overwrite existing files or require complex diffing algorithms just to lay down initial file placeholders.

### The Solution
Verifies the presence of each target file on disk. If absent, an empty placeholder file is safely created; if already present, it is completely preserved.

---

## Architecture & Pipeline Position

```
[1. Folders] ──▶ [2. Files] ──▶ [3. Content] ──▶ [4. Hook Routes]
                                                         │
[8. Orchestrator] ◀── [7. Validate] ◀── [6. Endpoints] ◀── [5. App]
```

**This repository implements Stage 2 in isolation.** It can be used independently in custom build workflows or composed automatically via `json-api-builder`.

---

## Installation

```bash
npm install json-api-builder-create-files
```

---

## Quick Start

```js
import { createFiles } from "json-api-builder-create-files";

const result = createFiles({
    clientRoot: "./my-project",
    outputRoot: "./my-project/output"
});

console.log("Provisioned placeholder files:", result.createdFiles);
```

---

## Input Specification

Tree definition containing leaf nodes mapped to template identifiers.

```json
{
  "api": {
    "routes.js": "routes.estree.json",
    "v1": {
      "routes.js": "routes.estree.json"
    }
  }
}
```

---

## Output Transformation

Placeholder leaf files initialized without disturbing any existing files.

```text
output/
└── api/
    ├── routes.js          (empty placeholder)
    └── v1/
        └── routes.js      (empty placeholder)
```

---

## API Reference

### `createFiles`

Accepts an options object with the following configuration:

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `clientRoot` | `string` | `process.cwd()` | Root path of the client project. |
| `outputRoot` | `string` | `path.join(clientRoot, 'output')` | Destination directory containing folder hierarchy. |
| `treePath` | `string` | `input/source-of-truth/tree.json` | Path to tree.json specification. |

---

## Sibling Pipeline Packages

| Stage | Package | Description |
| :---: | :--- | :--- |
| **01** | [json-api-builder-create-folders](https://github.com/keshavsoft/json-api-builder-create-folders) | Directory hierarchy scaffolding |
| **02** | [json-api-builder-create-files](https://github.com/keshavsoft/json-api-builder-create-files) | Non-destructive placeholder file creation |
| **03** | [json-api-builder-insert-content](https://github.com/keshavsoft/json-api-builder-insert-content) | ESTree template to JavaScript insertion |
| **04** | [json-api-builder-alter-content](https://github.com/keshavsoft/json-api-builder-alter-content) | Hierarchical route hooking & AST imports |
| **05** | [json-api-builder-create-app](https://github.com/keshavsoft/json-api-builder-create-app) | Root app.js assembly & server runtime |
| **06** | [json-api-builder-create-endpoints](https://github.com/keshavsoft/json-api-builder-create-endpoints) | CRUD endpoints & local JSON file sync |
| **07** | [json-api-builder-validation](https://github.com/keshavsoft/json-api-builder-validation) | Specification linting & artifact checks |
| **08** | [json-api-builder](https://github.com/keshavsoft/json-api-builder-orchestrator) | Master orchestrator coordinating all stages |

---

## Development & Testing

```bash
# Install dependencies
npm install

# Run automated tests
npm test

# Run syntax check
npm run check

# Prepublish verification
npm run prepublishOnly
```

---

## License

[MIT](LICENSE) © 2026 [KeshavSoft](https://github.com/keshavsoft)
