# AGENTS.md

Agent onboarding for `jlink-agent-core`.

## What this repo is

A small, deterministic CLI layer over SEGGER J-Link tools so coding agents can reliably run hardware-in-the-loop steps.

Core commands:
- `probe`
- `flash`
- `rtt-addr`
- `gdbserver-start`
- `gdbserver-stop`
- `rtt-capture`

## Fast start (agent path)

```bash
python3 -m pip install -e .
python3 -m jlink_agent_core.cli probe --json
```

## Typical agent workflow

1. Build firmware in target project.
2. Flash with `flash`.
3. Resolve RTT address from `.map` via `rtt-addr`.
4. Capture logs via `rtt-capture`.
5. Parse logs and decide pass/fail.

Always prefer `--json` so downstream logic is stable.

## Safety / correctness rules

- Never flash without explicit `--serial` and `--device`.
- Treat empty RTT logs as infra issue first (probe mapping / RTT address / timing).
- Avoid hardcoded board serial numbers in committed code.
- Keep output machine-readable; avoid changing JSON keys without version note.

## Development notes

- File layout:
  - `jlink_agent_core/cli.py` -> CLI argument handling
  - `jlink_agent_core/jlink.py` -> tool wrappers
- Keep wrappers thin and deterministic.
- Return structured dicts with `ok` and key diagnostic fields.

## Roadmap priorities

1. Per-SN process lock (prevent multi-agent contention)
2. Precise gdbserver stop by SN/port (replace coarse kill)
3. Profile-driven command (`--profile`)
4. Unified `run` pipeline (flash + RTT + classification hooks)
