# AzurPilot personal live patches

This branch is the only maintained source for personal AzurPilot patches.
It must stay based on the current upstream `master`.

## Patch set

1. Keep `Alas.Emulator.Serial` configured as `auto`
   - Auto detection updates only the active connection.
   - It does not persist a temporary emulator serial back to config.
   - On macOS, running MuMu Pro instances are rediscovered after restarts.

2. Preserve the configured event deadline
   - `_disable_tasks()` may keep its normal task-disable behavior.
   - It must not reset `EventGeneral.EventGeneral.TimeLimit`.
   - Deadline checking and manual deadline editing remain unchanged.

## Update and reapply policy

Do not disable, replace, or bypass AzurPilot's updater.

The launcher is allowed to run `reset --hard origin/master`, which removes
local code patches. After an AzurPilot update completes:

1. Fetch this fork.
2. Rebase or rebuild this branch on the new upstream `master` if needed.
3. Apply the two patch commits manually.
4. Set `Alas.Emulator.Serial` to `auto`.
5. Hot-reload the Alas service without running the launcher update again.

## Verification

- `git diff --check` passes.
- The two changed code files compile.
- Runtime config still reports `Alas.Emulator.Serial = auto`.
- Device detection selects the available emulator without rewriting config.
- `EventGeneral.EventGeneral.TimeLimit` is unchanged after activity-task
  disable handling.
- The scheduler can start an activity task normally.
