# Filesystem Overview

- Date: 2026/06/07
- Summary: A source-recovery archive of Anthropic's official Claude Code CLI, version 2.1.88, reconstructed from the `cli.js.map` source map that shipped (and was later unpublished) on npm. It is a read-only study of the CLI's architecture — not a buildable project (no `package.json` or build toolchain). The recovered TypeScript/TSX under `src/` covers the entrypoints, the slash-command system, the React + Ink terminal UI, the agent/tool execution loop, MCP integration, the API/provider layer, and a large supporting `utils/` and `services/` surface. `node_modules/` holds the runtime dependencies the original package was built against, and `vendor/` holds native-addon TypeScript stubs.

## Structure

```
ponponon/
├── README.md                  — Project intro: what the recovery is, install via mirror, structure highlights, disclaimer
├── claude-code-2.1.88.tgz     — Original npm tarball of @anthropic-ai/claude-code@2.1.88 (the source of the recovery)
├── .git/                      — Git version-control metadata   (dot-folder leaf, not expanded)
├── node_modules/              — Installed third-party dependencies the original package was built against (~190 packages: @anthropic-ai/sdk, react, ink-fork @ant, @modelcontextprotocol, @aws-sdk, axios, chalk, zod, etc.); large vendored tree, not enumerated
├── vendor/                    — Native-addon source stubs (audio capture, image processing, modifiers, URL handler)   (expanded)
│   ├── audio-capture-src/
│   │   └── index.ts           — Native audio-capture addon TypeScript binding stub
│   ├── image-processor-src/
│   │   └── index.ts           — Native image-processor addon binding stub
│   ├── modifiers-napi-src/
│   │   └── index.ts           — Native keyboard-modifier detection addon binding stub
│   └── url-handler-src/
│       └── index.ts           — Native URL-scheme handler addon binding stub
└── src/                       — Recovered CLI source (TypeScript/TSX)   (expanded)
    ├── QueryEngine.ts         — High-level conversation orchestrator wrapping query() (state, compaction, snapshots)
    ├── Task.ts                — Task type definitions for the agent/task subsystem
    ├── Tool.ts                — Tool interface definition and tool-lookup utilities
    ├── commands.ts            — Slash-command registry assembly (built-in + skills + plugins + MCP)
    ├── context.ts             — Builds system/user context (git status, date, CLAUDE.md, memory) for API calls
    ├── cost-tracker.ts        — Per-session token/cost accounting
    ├── costHook.ts            — Hook wiring cost tracking into the turn loop
    ├── dialogLaunchers.tsx    — Launchers for interactive Ink dialogs/overlays
    ├── history.ts             — Conversation history persistence and retrieval
    ├── ink.ts                 — Ink render wrapper with ThemeProvider injection
    ├── interactiveHelpers.tsx — Shared helpers for the interactive REPL flow
    ├── main.tsx               — Commander.js CLI definition; registers all subcommands and the main REPL/headless action (~800KB)
    ├── projectOnboardingState.ts — Tracks first-run project onboarding/trust state
    ├── query.ts               — Core API query loop: streams responses, dispatches tool calls, runs the turn loop
    ├── replLauncher.tsx       — Bootstraps and launches the interactive REPL screen
    ├── setup.ts               — Runtime setup/initialization helpers
    ├── tasks.ts               — Task subsystem entry helpers
    ├── tools.ts               — Tool registry assembly (built-in tools, feature-gated loading)
    ├── assistant/             — Assistant-side session history handling
    │   └── sessionHistory.ts
    ├── bootstrap/             — Module-level session-global singletons (session id, cwd, project root)
    │   └── state.ts
    ├── bridge/                — Remote Control / Bridge mode: remote session API, JWT auth, messaging, permission callbacks (bridgeMain.ts is entry; ~30 files)
    ├── buddy/                 — "Companion" sprite/notification feature (animated terminal buddy)
    ├── cli/                   — CLI I/O plumbing: exit handling, print, NDJSON/structured/remote transports
    │   ├── exit.ts
    │   ├── ndjsonSafeStringify.ts
    │   ├── print.ts
    │   ├── remoteIO.ts
    │   ├── structuredIO.ts
    │   ├── update.ts
    │   ├── handlers/          — CLI command/mode handlers
    │   └── transports/        — I/O transport implementations
    ├── commands/              — Slash-command implementations, one folder/file per command (~90 commands: login, mcp, review, tasks, config, model, doctor, plugin, etc.)
    ├── components/            — React + Ink terminal UI components (~33 subfolders + many files: App, Messages, PromptInput, permissions/, design-system/, agents/, etc.)
    ├── constants/             — Static constants (API limits, betas, prompts, system prompt sections, tool limits, OAuth, figures)
    ├── context/              — React context providers (stats, notifications, modals, overlays, voice, mailbox, fps)
    ├── coordinator/           — Multi-worker coordinator mode
    │   └── coordinatorMode.ts
    ├── entrypoints/           — Process entrypoints and SDK surface
    │   ├── agentSdkTypes.ts
    │   ├── cli.tsx            — True CLI entrypoint; fast-path dispatch (version, mcp, daemon, bridge, …)
    │   ├── init.ts           — One-time initialization (telemetry, config, trust dialog)
    │   ├── mcp.ts            — MCP server entrypoint
    │   ├── sandboxTypes.ts
    │   └── sdk/              — Programmatic SDK entry surface
    ├── hooks/                 — React hooks for terminal state/interaction (~80 hooks: input, history, keybindings, IDE integration, mailbox, model)
    │   ├── notifs/
    │   └── toolPermission/    — Tool-permission hook + handlers/
    ├── ink/                   — Customized Ink terminal-rendering infrastructure (reconciler, renderer, layout, termio, ANSI, focus)
    │   ├── components/
    │   ├── events/
    │   ├── hooks/
    │   ├── layout/
    │   └── termio/
    ├── keybindings/           — Keybinding schema, parser, resolver, default bindings, user-binding loader
    ├── memdir/                — Memory directory: scan, age, relevance, team-memory paths/prompts
    ├── migrations/            — One-shot settings/model migrations (model renames, auto-update opt-in, etc.)
    ├── moreright/             — "More right" overflow UI hook
    │   └── useMoreRight.tsx
    ├── native-ts/             — Pure-TS implementations of native modules
    │   ├── color-diff/        — Color-difference computation
    │   ├── file-index/        — File indexing
    │   └── yoga-layout/       — Yoga flexbox layout (enums + bindings)
    ├── outputStyles/          — Loader for user output-style definitions
    │   └── loadOutputStylesDir.ts
    ├── plugins/               — Plugin system: built-in plugins and bundled/ plugin assets
    │   ├── builtinPlugins.ts
    │   └── bundled/
    ├── query/                 — Query loop config/deps: stop hooks, token budget
    ├── remote/                — Remote session manager, WebSocket sessions, permission bridge, SDK message adapter
    ├── schemas/               — Zod/JSON schemas (hooks)
    │   └── hooks.ts
    ├── screens/               — Top-level Ink screens (REPL, Doctor, ResumeConversation)
    ├── server/                — Direct-connect session server (manager, session creation, types)
    ├── services/              — Core business-logic services (~22 subfolders: api/, mcp/, lsp/, oauth/, compact/, extractMemories/, plus voice, rate-limit, analytics)
    │   ├── AgentSummary/      — Agent summary generation
    │   ├── MagicDocs/         — Auto-updating "magic docs"
    │   ├── PromptSuggestion/  — Prompt suggestion service
    │   ├── SessionMemory/     — Session memory persistence
    │   ├── analytics/         — Analytics (largely stubbed)
    │   ├── api/               — API client layer: claude.ts, client.ts, usage, errors, retry, files API, dump prompts
    │   ├── autoDream/         — Background "dream"/idle processing
    │   ├── compact/           — Conversation compaction
    │   ├── extractMemories/   — Memory extraction from conversations
    │   ├── lsp/               — Language-server-protocol management
    │   ├── mcp/               — MCP client/server services
    │   ├── oauth/             — OAuth flows
    │   ├── plugins/           — Plugin service logic
    │   ├── policyLimits/      — Policy/rate-limit enforcement
    │   ├── remoteManagedSettings/ — Remotely managed (MDM-style) settings
    │   ├── settingsSync/      — Settings synchronization
    │   ├── teamMemorySync/    — Team-memory synchronization
    │   ├── tips/              — In-app tips
    │   ├── toolUseSummary/    — Tool-use summarization
    │   └── tools/             — Tool-related services
    ├── skills/                — Skill system and bundled/ skill assets
    │   └── bundled/
    ├── state/                 — Central app state (AppState, store, selectors)
    ├── tasks/                 — Task-type implementations (Dream, InProcessTeammate, LocalAgent, LocalShell, RemoteAgent)
    │   ├── DreamTask/
    │   ├── InProcessTeammateTask/
    │   ├── LocalAgentTask/
    │   ├── LocalShellTask/
    │   └── RemoteAgentTask/
    ├── tools/                 — Built-in tool implementations, one folder per tool (~50 tools: Bash, FileEdit/Read/Write, Glob, Grep, Agent, Task*, MCP*, WebFetch/Search, Skill, etc.)
    │   ├── AgentTool/         — Agent tool with built-in/ agents
    │   ├── shared/            — Shared tool helpers
    │   └── testing/           — Tool testing utilities
    ├── types/                 — TypeScript types (commands, hooks, ids, logs, permissions, plugin) + generated/ protobuf types
    │   └── generated/         — Codegen'd protobuf event/Google types (events_mono, google/protobuf)
    ├── upstreamproxy/         — Upstream proxy handling
    ├── utils/                 — Large grab-bag of utility modules (~300 files + subfolders: auth, git, github, mcp, model, settings, shell, sandbox, telemetry, bash, computerUse, etc.)
    │   ├── background/        — Background task management (+ remote/)
    │   ├── bash/              — Bash command handling (+ specs/)
    │   ├── claudeInChrome/    — Claude-in-Chrome integration
    │   ├── computerUse/       — Computer-use support
    │   ├── deepLink/          — Deep-link handling
    │   ├── dxt/               — DXT extension support
    │   ├── filePersistence/   — File persistence helpers
    │   ├── git/               — Git helpers
    │   ├── github/            — GitHub integration helpers
    │   ├── hooks/             — Hook-execution utilities
    │   ├── mcp/               — MCP utilities
    │   ├── memory/            — Memory utilities
    │   ├── messages/          — Message utilities
    │   ├── model/             — Model selection/capabilities/providers/aliases
    │   ├── nativeInstaller/   — Native-binary installer
    │   ├── permissions/       — Permission utilities
    │   ├── plugins/           — Plugin utilities
    │   ├── powershell/        — PowerShell support
    │   ├── processUserInput/  — User-input processing
    │   ├── sandbox/           — Sandbox utilities
    │   ├── secureStorage/     — Secure credential storage
    │   ├── settings/          — Settings (+ mdm/) handling
    │   ├── shell/             — Shell utilities
    │   ├── skills/            — Skill utilities
    │   ├── suggestions/       — Suggestion utilities
    │   ├── swarm/             — Agent-swarm support (+ backends/)
    │   ├── task/              — Task utilities
    │   ├── telemetry/         — Telemetry utilities
    │   ├── teleport/          — Session teleport
    │   ├── todo/              — Todo utilities
    │   └── ultraplan/         — Ultraplan planning support
    ├── vim/                   — Vim-mode editing support
    └── voice/                 — Voice-mode (push-to-talk) support
```
