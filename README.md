# 7Pilot

A free Roblox Studio copilot. It reads your whole place, writes Luau across files,
and proves the change with a playtest instead of claiming it works.

**Download:** [`7pilot-sfx.exe`](./7pilot-sfx.exe) — 33.5 MB, Windows x64. No installer.

## Run it

```
7pilot-sfx.exe init          # pick a model, register with your agent, install the plugin
7pilot-sfx.exe serve         # start the bridge, prints a Studio pairing code
```

`init` asks once. After that, open Roblox Studio, enable the 7Pilot plugin, and pair
it with the code `serve` prints. A second terminal running `chat` talks to the
built-in agent; or skip it entirely and let Claude Code, Codex, Cursor or Windsurf
drive Studio through the MCP server.

That exe is a self-extracting launcher: a 2.4 MB stub carries the zstd-compressed
runtime, unpacks to your cache on first run, and reuses it after. Warm start costs
about 130 ms over a raw binary.

## Free, and here is what that means

No account. No server. No credits, no metering, no telemetry — `"telemetry": false`
in `src/config.js`, and the only hosts the binary contacts are the model provider
you pick.

Two routes cost nothing at all:

**A local model.** Ollama and LM Studio need no API key:

```
7pilot-sfx.exe init --provider ollama      # qwen2.5-coder:14b by default
7pilot-sfx.exe init --provider lmstudio    # anything on 127.0.0.1:1234
```

**Or no model.** The MCP server never constructs a provider, so if you already run
Claude Code or Codex, 7Pilot adds nothing to your bill:

```
7pilot-sfx.exe mcp-config
{"mcpServers":{"7pilot":{"command":"...\\7pilot-sfx.exe","args":["mcp"]}}}
```

## What's inside

**Four provider connectors**, each speaking its own wire protocol: OpenAI-compatible
(also Groq, Together, OpenRouter, DeepSeek, Mistral, xAI, vLLM), Anthropic Messages,
Google Gemini `generateContent`, and Ollama `/api/chat`. Anthropic gets a top-level
`system` string and alternating roles; Gemini gets `systemInstruction`, `role: "model"`
and JSON Schema with `additionalProperties` stripped; Ollama gets inlined tool calls
with no API key.

**Eleven skill packs** inlined into the system prompt — server authority, DataStores,
remotes, UI, maps, animation, VFX, sound, perf, testing, and the playtest subagent
runbook. They encode the conventions that make Roblox code survive a real playtest.

**Twenty-seven Studio tools** over MCP: read the tree, grep scripts, patch across
files, create instances, run and verify a playtest, capture a screenshot.

**Merge-safe registration** into Claude Code, Claude Desktop, Cursor, Windsurf, Codex
CLI and a generic JSON entry. Existing servers in your config are preserved exactly,
and a corrupt config is refused rather than overwritten.

## Platform

This is the Windows build. Linux and macOS launchers with the same toolchain, plus
the full source, tests and build pipeline, are available on request.

## License

MIT.
