# Android Device Selector

A Codex skill that asks you to choose a device before each new Android app test task.

Choose from:

- An existing Android emulator.
- A new Android emulator.
- A connected physical Android device.

## Install

Clone this repository into your local skills directory:

```sh
mkdir -p ~/.agents/skills
git clone https://github.com/sjeon87/android-device-selector.git ~/.agents/skills/android-device-selector
```

If the skill does not appear, restart Codex.
See the [official OpenAI documentation](https://developers.openai.com/codex/skills) for skill locations and invocation options.

## Requirements

- Use Codex with access to Android tools and the app project.
- For emulator tests, provide Android SDK tools and a compatible system image.
- For physical device tests, connect the device and enable Android debugging.
- Authorize the debugging connection on the physical device before testing.

## Use

Invoke the skill in Codex CLI or the IDE extension:

```text
$android-device-selector Test this Android app.
```

Codex can also select this skill when a request matches its description.
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
- [agents/openai.yaml](agents/openai.yaml): Name and description for the skill interface.
