# CLAUDE.md

Guidance for Claude/Codex/OpenClaw style agents working in this repository.

## Repository goal

Expose J-Link operations as composable, agent-friendly primitives.

## Do first

```bash
python3 -m pip install -e .
python3 -m jlink_agent_core.cli --help
python3 -m jlink_agent_core.cli probe --json
```

## Golden command examples

```bash
python3 -m jlink_agent_core.cli flash \
  --device <MCU_DEVICE> \
  --serial <JLINK_SN> \
  --firmware <path/to/firmware.hex> \
  --json

python3 -m jlink_agent_core.cli rtt-addr \
  --map <path/to/firmware.map> \
  --json

python3 -m jlink_agent_core.cli rtt-capture \
  --device <MCU_DEVICE> \
  --serial <JLINK_SN> \
  --address <0xADDR> \
  --out /tmp/rtt.log \
  --duration 20 \
  --json
```

## Agent coding standards

- Keep functions small, explicit, side-effect-limited.
- Preserve backward compatibility for existing JSON output fields.
- Add new fields instead of renaming/removing old fields.
- Bubble tool failures as clear `error` strings.

## What not to do

- Do not commit personal serial numbers, local absolute paths, or credentials.
- Do not assume a single board/vendor layout.
- Do not add UI-heavy abstractions before lock/profile primitives exist.

## Validation checklist before commit

- `python3 -m jlink_agent_core.cli --help`
- At least one dry-run style command succeeds (`probe --json`)
- README examples still match actual CLI arguments
- No sensitive strings in diff
