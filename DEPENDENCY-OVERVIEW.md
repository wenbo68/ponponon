# Dependency Overview

## Manifest Status

This repository has **no `package.json` at the root**. It is a recovered source archive — not a buildable npm package. The `node_modules/` directory was committed directly to the repo and contains 192 top-level packages that were present in the original `@anthropic-ai/claude-code@2.1.88` npm bundle.

The list below is derived from the directory names in `node_modules/`.

---

## Runtime Dependencies (from node_modules)

### Anthropic / AI SDK
| Package | Purpose |
|---|---|
| `@anthropic-ai/*` | Anthropic Claude API SDK and related packages |
| `@modelcontextprotocol/*` | Model Context Protocol client/server libraries |
| `@growthbook/*` | Feature flag / A/B testing (stubbed out in source) |

### AWS / Cloud
| Package | Purpose |
|---|---|
| `@aws-sdk/*` | AWS SDK (Bedrock provider support) |
| `@aws-crypto/*` | AWS cryptographic utilities |
| `@azure/*` | Azure SDK (Foundry provider support) |
| `google-auth-library` | Google auth (Vertex AI provider) |
| `gcp-metadata` | GCP metadata (Vertex AI provider) |
| `gtoken` | Google service account token |
| `gaxios` | Google HTTP client |
| `@grpc/*` | gRPC for Google Cloud APIs |

### CLI Framework
| Package | Purpose |
|---|---|
| `commander` | CLI argument parsing (`@commander-js/extra-typings`) |
| `chalk` | Terminal color output |
| `figures` | Terminal-safe Unicode symbols |
| `ansi-regex` / `ansi-styles` | ANSI escape code utilities |
| `string-width` | CJK-aware string width |
| `wrap-ansi` | Word-wrap with ANSI support |
| `strip-ansi` | Strip ANSI codes |
| `supports-color` | Detect terminal color support |
| `supports-hyperlinks` | Detect terminal hyperlink support |
| `cli-highlight` | Syntax highlighting in terminal |
| `cli-boxes` / `cli-width` | Box drawing / terminal width |
| `highlight.js` | Code highlighting engine |

### Terminal UI (Ink / React)
| Package | Purpose |
|---|---|
| `react` | React runtime (used by Ink) |
| `react-reconciler` | React custom renderer base |
| `@ant/*` | Internal Ink fork and computer-use packages |
| `usehooks-ts` | React hooks utilities |
| `asciichart` | ASCII charts in terminal |

### HTTP / Networking
| Package | Purpose |
|---|---|
| `axios` | HTTP client (API calls, MCP) |
| `node-fetch` | Fetch polyfill |
| `follow-redirects` | HTTP redirect following |
| `undici` | Node.js HTTP/1.1 client |
| `form-data` | Multipart form data |
| `eventsource` / `eventsource-parser` | SSE streaming |
| `ws` | WebSocket client/server |
| `http-proxy-agent` / `https-proxy-agent` | Proxy support |
| `proxy-from-env` | Read proxy settings from env |
| `gaxios` | Google HTTP client |

### MCP / gRPC / Protobuf
| Package | Purpose |
|---|---|
| `@modelcontextprotocol/*` | MCP protocol implementation |
| `vscode-jsonrpc` | JSON-RPC transport (MCP) |
| `protobufjs` | Protocol Buffers runtime |
| `@grpc/*` | gRPC framework |
| `@opentelemetry/*` | Telemetry/tracing (stubbed) |

### Authentication / Security
| Package | Purpose |
|---|---|
| `jsonwebtoken` / `jwa` / `jws` | JWT auth (Claude OAuth) |
| `node-forge` | Crypto (TLS, keys) |
| `pkce-challenge` | PKCE OAuth flow |
| `buffer-equal-constant-time` | Timing-safe buffer compare |
| `ecdsa-sig-formatter` | ECDSA signature formatting |

### File System / Process
| Package | Purpose |
|---|---|
| `chokidar` | File watching |
| `readdirp` | Recursive directory read |
| `graceful-fs` | Graceful fs wrappers |
| `jsonfile` | JSON file read/write |
| `proper-lockfile` | File locking |
| `execa` | Child process execution |
| `tree-kill` | Kill process trees |
| `cross-spawn` | Cross-platform spawn |
| `npm-run-path` | PATH for npm scripts |
| `which` | Locate executables |

### Data / Validation
| Package | Purpose |
|---|---|
| `zod` | Schema validation (primary) |
| `zod-to-json-schema` | Zod → JSON Schema conversion |
| `ajv` / `ajv-formats` | JSON Schema validation |
| `fast-deep-equal` | Deep equality |
| `fast-xml-parser` | XML parsing |
| `yaml` | YAML parsing |
| `json-bigint` | BigInt-safe JSON |
| `jsonc-parser` | JSON with comments |
| `fast-uri` | URI utilities |
| `fuse.js` | Fuzzy search |
| `lodash-es` | Functional utilities (ES modules) |
| `lodash.*` | Individual lodash functions |
| `lru-cache` | LRU cache |

### Markdown / Content
| Package | Purpose |
|---|---|
| `marked` | Markdown parser |
| `turndown` | HTML → Markdown |
| `parse5` | HTML parser |
| `xss` | XSS sanitization |
| `cli-highlight` | Syntax-highlighted code rendering |

### Image / Media
| Package | Purpose |
|---|---|
| `sharp` | High-performance image processing |
| `@img/*` | Sharp sub-packages |
| `pngjs` | PNG encode/decode |
| `qrcode` | QR code generation |
| `fflate` | Fast compression (zlib) |

### Utilities
| Package | Purpose |
|---|---|
| `uuid` | UUID generation |
| `semver` | Semantic versioning |
| `chalk` | Terminal colors |
| `open` | Open URLs/files in default browser |
| `default-browser` | Detect default browser |
| `env-paths` | Standard OS paths (config, cache) |
| `p-map` | Async array map with concurrency |
| `retry` | Retry logic |
| `diff` | Text diff |
| `pretty-bytes` | Human-readable byte sizes |
| `ms` | Millisecond duration parsing |
| `debug` | Debug logging |
| `tslib` | TypeScript helpers |
| `ignore` | .gitignore-style pattern matching |
| `mime-types` | MIME type lookup |
| `xmlbuilder` | XML builder |
| `wsl-utils` | WSL detection utilities |
| `run-applescript` | macOS AppleScript runner |
| `plist` | macOS plist parser |

---

## Notes

- There is no `package.json` manifest — the list above is inferred from `node_modules/` directory contents.
- The `vendor/` directory contains C/C++/Rust/Swift native addon sources (audio capture, image processor, keyboard modifiers, URL handler) that would require a separate native build step (`node-gyp` / `napi-rs` / `swift build`).
- The original package was `@anthropic-ai/claude-code@2.1.88`, runtime target: **Bun** (not Node.js).
