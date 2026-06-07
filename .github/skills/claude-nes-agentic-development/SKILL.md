---
name: Claude NES Agentic Development
description: Use this skill for LLM-driven NES game development in 6502 assembly with iterative planning, coding, testing, and debugging.
---

# Claude NES Agentic Development

## Purpose
Guide an LLM agent (Claude-compatible) to build NES games in 6502 assembly with
small, testable increments and emulator-driven feedback loops.

## Core References
- NESDev Programming Guide: https://www.nesdev.org/wiki/Programming_guide
- NES Starter Kit: https://github.com/igwgames/nes-starter-kit

## Recommended Tooling
- Assembler/linker stack: `cc65` (`ca65`, `ld65`)
- Emulator for debugging: `Mesen 2` or `FCEUX`
- Graphics/audio tools as needed:
  - `Aseprite` (sprites/tiles)
  - `NEXXT` or equivalent CHR editor
  - `FamiTracker` (music/SFX pipeline, if used by project)
- Build automation: Makefile or project scripts from the starter kit

## Agent Workflow
1. Read and summarize current ROM/assembly structure before edits.
2. Plan one small feature at a time (input, movement, rendering, audio, etc.).
3. Implement in narrow scope with labels/constants and clear memory-map usage.
4. Build ROM, run emulator, verify expected behavior, and capture regressions.
5. Iterate with tiny commits and explicit verification notes.

## Prompt Template for Claude
Use this as a reusable system/task prompt for NES work:

```text
You are an expert NES (6502) assembly development agent.
Priorities:
1) Correctness on NES hardware constraints (PPU/APU timing, NMI, memory limits).
2) Minimal, surgical code changes.
3) Build + run verification on every change.

Required process:
- Inspect existing code paths and memory map before editing.
- Propose a short plan with step-by-step feature slices.
- Implement one slice at a time in assembly.
- After each slice: build ROM, run emulator checks, report exact outcomes.
- If failure occurs: include root cause and smallest safe fix.

Use these references when deciding architecture/tooling:
- https://www.nesdev.org/wiki/Programming_guide
- https://github.com/igwgames/nes-starter-kit
```

## Guardrails
- Keep PRs small and reversible.
- Avoid broad refactors while introducing gameplay features.
- Prefer deterministic initialization/reset behavior.
- Preserve stable NMI/update-loop structure when adding features.
