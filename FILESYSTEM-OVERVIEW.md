# Filesystem Overview

## Summary

**ponponon** is a source-code recovery project for `@anthropic-ai/claude-code` version 2.1.88. Anthropic accidentally published a `cli.js.map` source map with that npm release (March 2026), which contained the original TypeScript sources in `sourcesContent`. This repo reconstructs those ~700,000 lines of source into a readable directory tree for research purposes — studying the CLI architecture, command system, MCP integration, and feature-flag mechanism of Claude Code.

The project is **not buildable as-is** (no `package.json` at the root, no tsconfig, no build toolchain). The `node_modules/` directory and `claude-code-2.1.88.tgz` archive are included for reference. The `.claude/` folder contains Claude Code agent/skill/command definitions added by the repository maintainer.

---

## Annotated Directory Tree

```
ponponon/
├── README.md                        # Project overview, install instructions, disclaimer
├── claude-code-2.1.88.tgz           # Original npm tarball (31 MB) — source of the recovery
│
├── .claude/                         # Claude Code project config (not expanded — hidden folder)
│   ├── agents/                      # Custom agent definitions
│   ├── commands/                    # Custom slash-commands (e.g. /create-overview)
│   └── skills/                      # Reusable skill definitions
│
├── node_modules/                    # Vendored npm packages (192 top-level packages)
│
├── vendor/                          # Native addon source trees
│   ├── audio-capture-src/           # Audio capture native module source
│   ├── image-processor-src/         # Image processing native module source
│   ├── modifiers-napi-src/          # Keyboard modifier key detection (macOS FFI)
│   └── url-handler-src/             # URL scheme handler native module source
│
└── src/                             # Recovered TypeScript source (core of the repo)
    ├── main.tsx                     # Commander.js CLI definition (~800 KB, ~5600 lines)
    ├── query.ts                     # Main API query loop — streams Claude API responses
    ├── QueryEngine.ts               # Higher-level orchestrator wrapping query()
    ├── Tool.ts                      # Tool interface + registry utilities
    ├── tools.ts                     # Tool registry — assembles the active tool list
    ├── commands.ts                  # Slash-command loader / dispatcher
    ├── context.ts                   # Builds system/user context for API calls
    ├── cost-tracker.ts              # Token cost tracking
    ├── costHook.ts                  # React hook for cost state
    ├── dialogLaunchers.tsx          # Ink dialog launcher helpers
    ├── history.ts                   # Conversation history persistence
    ├── ink.ts                       # Ink render wrapper (ThemeProvider injection)
    ├── interactiveHelpers.tsx       # Interactive terminal UI helpers (~57 KB)
    ├── projectOnboardingState.ts    # First-run onboarding state
    ├── query.ts                     # Core streaming query function
    ├── replLauncher.tsx             # REPL screen launcher
    ├── setup.ts                     # Session setup / environment validation
    ├── Task.ts                      # Task type definition
    ├── tasks.ts                     # Task management utilities
    │
    ├── assistant/                   # Assistant-side message helpers
    ├── bootstrap/                   # Module-level singletons (session ID, CWD, tokens)
    ├── bridge/                      # Remote Control / Bridge mode (feature-gated)
    ├── buddy/                       # "Buddy" feature (feature-gated)
    ├── cli/                         # CLI argument parsing helpers
    ├── commands/                    # Individual slash-command implementations
    │   │                            #   (login, mcp, review, tasks, ssh, open, …)
    ├── components/                  # React/Ink terminal UI components
    │   │                            #   (App, Messages, PromptInput, permissions, …)
    ├── constants/                   # Constant values (tool whitelist, etc.)
    ├── context/                     # Context-building sub-modules
    ├── coordinator/                 # Multi-worker coordinator (feature-gated)
    ├── entrypoints/                 # CLI entrypoints
    │   │                            #   (cli.tsx — true entry, init.ts — one-time init)
    ├── hooks/                       # React hooks for terminal state
    ├── ink/                         # Custom Ink infrastructure (components, keybindings)
    ├── keybindings/                 # Keyboard shortcut definitions
    ├── memdir/                      # Memory / notes directory management
    ├── migrations/                  # Data migrations for persisted config
    ├── moreright/                   # Additional right-panel UI components
    ├── native-ts/                   # TypeScript wrappers around native addons
    ├── outputStyles/                # Terminal output formatting styles
    ├── plugins/                     # Plugin system (install/uninstall/enable/disable)
    ├── query/                       # Query sub-modules
    ├── remote/                      # Remote session management
    ├── schemas/                     # Zod / JSON schemas
    ├── screens/                     # Full-screen Ink views (REPL, etc.)
    ├── server/                      # Local HTTP server (MCP, API proxy)
    ├── services/                    # Core business logic
    │   │                            #   (api/, mcp/, permissions/, skills/, …)
    ├── skills/                      # Built-in skill implementations
    ├── state/                       # App state management (Zustand-style store)
    ├── tasks/                       # Task sub-modules
    ├── tools/                       # Tool helper sub-modules
    ├── types/                       # TypeScript type declarations
    │   │                            #   (global.d.ts, message.ts, permissions.ts, …)
    ├── upstreamproxy/               # Upstream proxy configuration
    ├── utils/                       # Utility functions
    │   │                            #   (auth, file ops, process management, model, …)
    ├── vim/                         # Vim-mode keybinding support
    └── voice/                       # Voice mode (push-to-talk, feature-gated)
```
