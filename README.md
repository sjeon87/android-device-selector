# Android Device Selector

A reusable skill that asks you to choose a device before each new Android app test task.
Use it with Codex, Claude Code, Hermes Agent, or another agent that supports the [Agent Skills format](https://agentskills.io/specification).

Choose from:

- An existing Android emulator.
- A new Android emulator.
- A connected physical Android device.

## Install for your user account

Keep one shared copy in `~/.agents/skills`.
Link that copy into the directories that other agents use.
These installations apply across projects for your user account.

### Shared copy

Clone the repository if the destination does not already exist:

```sh
mkdir -p ~/.agents/skills
git clone https://github.com/sjeon87/android-device-selector.git ~/.agents/skills/android-device-selector
```

If you already keep a copy elsewhere, move it to this directory or link to that copy instead.
Do not replace an existing installation without checking its contents.

### Codex

Codex reads the shared directory directly.
If the skill does not appear, restart Codex.
See the [official OpenAI documentation](https://developers.openai.com/codex/skills).

### Claude Code

Link the shared copy into the personal skills directory:

```sh
mkdir -p ~/.claude/skills
ln -s "$HOME/.agents/skills/android-device-selector" "$HOME/.claude/skills/android-device-selector"
```

See the [Claude Code documentation](https://code.claude.com/docs/en/skills).

### Hermes Agent

Link the shared copy into the default profile:

```sh
mkdir -p ~/.hermes/skills
ln -s "$HOME/.agents/skills/android-device-selector" "$HOME/.hermes/skills/android-device-selector"
hermes skills list --source local
```

For a named profile, use that profile's skills directory instead.
Start a new Hermes session, or run `/reload-skills` in the current session.
See the [Hermes skills documentation](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills).

Run each link command only when its destination does not already exist.
The link commands above require a shell with symbolic link support.
You can copy the skill directory instead, but you must update each copy separately.

### Other agents

Install this directory in the location that your agent uses for skills.
The required entry point is [SKILL.md](SKILL.md).
Your agent must support user questions and access to local Android tools.

## Requirements

- Give your agent access to Android tools and the app project.
- For emulator tests, provide Android SDK tools and a compatible system image.
- For physical device tests, connect the device and enable Android debugging.
- Authorize the debugging connection on the physical device before testing.

## Use

Use the invocation syntax for your agent:

| Agent | Prompt |
| --- | --- |
| Codex CLI or IDE extension | `$android-device-selector Test this Android app.` |
| Claude Code | `/android-device-selector Test this Android app.` |
| Hermes Agent | `/android-device-selector Test this Android app.` |

For another agent, request the skill by name:

```text
Use the android-device-selector skill to test this Android app.
```

Agents that support automatic skill selection can also use this skill when a request matches its description.
If the agent cannot ask a question, it must wait for your choice before it uses a device.
Tests that do not need a device are outside the scope of this skill.

## Selection rules

1. Inspect saved emulators and connected physical devices without changes.
2. Show available targets, including device names, serial numbers, and connection states when available.
3. Ask you to choose a target in your language.
4. Wait for your answer before creating or starting an emulator, installing the app, or running device tests.
5. Use only the selected target.
6. Report the target and test result.

If your current request already specifies a target, the skill uses that choice without another question.
Your choice applies to retries and related checks within the same task.
Each new device test task requires a new choice.

The skill preserves existing device data and does not silently select a replacement target.
Offline devices and devices without debugging authorization are unavailable for testing.
If no target is available, choose whether to create an emulator, connect a physical device, or stop.

## Files

- [SKILL.md](SKILL.md): Instructions for device selection and testing.
- [agents/openai.yaml](agents/openai.yaml): Optional interface metadata for Codex. Other agents use `SKILL.md`.

## Support

[Buy me a coffee](https://www.buymeacoffee.com/lungo)
