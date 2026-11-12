---
name: android-device-selector
description: Ask the user to choose an existing emulator, a new emulator, or a connected physical Android device before each app test task. Use for Android device tests, including UI tests and manual checks. Exclude tests that do not need a device.
---

# Android Device Selector

Use this skill with any agent that supports the Agent Skills format and can access Android tools.
An Android Virtual Device (AVD) is a saved emulator configuration.

## Required choice

Ask the user to select a test target before each new Android device test task.
Offer these choices:

- An existing AVD.
- A new AVD.
- A connected physical Android device.

Wait for the user's choice before you create or start an AVD, install an app, or run device tests.
This requirement also applies to running emulators and connected physical devices.
Do not carry a choice from an earlier task into a new task.
If the current request already specifies the test target, use that choice without another question.

## Before the question

Use available Android tools to inspect saved AVDs, running emulators, and connected physical devices without changes.
Show available AVD names and whether each AVD runs, when this information is available.
For physical devices, show the model, device serial, and connection status when available.
Distinguish physical devices from emulators.
Identify devices that are offline or need debugging authorization as unavailable for testing.
Include the Android version or API level when it helps the user choose.
If inspection fails, state which targets you could not check.
Do not interpret failed inspection as an empty device list.

## Ask and wait

Use the user's language for the question.
Show existing AVDs and connected physical devices with the option to create a new AVD.
If several targets exist, ask which target to use as part of the same question.
If the list contains no physical devices, state this result.
If no existing target is available, ask whether to create an AVD, connect a physical device, or stop.
If an answer tool is available, use it.
Otherwise, ask in the conversation and wait for an answer.
If the agent cannot receive an answer, stop work that needs a device until the user supplies a choice.
Never treat silence, elapsed time, or a suggested default as the user's choice.
Continue independent preparation, such as source inspection or an offline build, while you wait.

Example question in Korean:

> 이번 Android 앱 테스트에 어떤 기기를 사용할까요? 기존 에뮬레이터: {AVD 목록}. 연결된 실제 기기: {모델·시리얼·연결 상태 목록}. 새 에뮬레이터 생성도 선택할 수 있습니다.

## Apply the choice

- For an existing AVD, use the selected AVD and preserve its data.
- For a new AVD, use the app requirements to select a compatible configuration.
- For a physical device, use the selected device and preserve its data.
- If the physical device needs debugging authorization, ask the user to authorize that device before testing.
- If the selected device is offline or disconnected, wait for its connection or ask for another choice.
- Ask for missing configuration details only when they materially affect the test.
- Create the new AVD with a unique name.
- Do not replace, delete, or reset another AVD as part of this choice.
- Do not reset a physical device or delete unrelated device data as part of this choice.
- If the selected target cannot support the test, explain the problem and ask for another choice.
- Do not silently create or select a replacement target.

Specify the selected device serial for every ADB command that acts on a device.
Configure other test tools to use only the selected target.
Do not let a tool select a device automatically.

Use the choice for retries and related checks within the same test task.
Ask again when the user requests a new Android device test task.
Report the target type, name or model, device serial, and test result when the task ends.
For an AVD, also report whether you created it.
