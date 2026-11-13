---
name: android-device-selector
description: Select an Android test device and reuse an available choice in the same conversation or session. Use for Android device tests, including UI tests and manual checks. Exclude tests that do not need a device.
---

# Android Device Selector

Use this skill with any agent that supports the Agent Skills format and can access Android tools.
An Android Virtual Device (AVD) is a saved emulator configuration.

## Select or reuse a target

If the current request specifies a test target, use that choice without another question.
Otherwise, check the previous choice in the current conversation or session before each new Android device test task.
Use available Android tools to check that target without changes.

- For an AVD, check that the saved configuration still exists and supports the test.
- Reuse that AVD even if its emulator stopped. Start it without another selection question when the test needs it.
- For a physical device, check that the selected device is connected, authorized, and ready for testing.
- If the previous target is available and supports the test, reuse it without another selection question.

Keep the target type and AVD name or physical device serial in the conversation context.
Also record whether you created the AVD.
An emulator serial can change after a restart. Match the running emulator to the selected AVD name before using its serial.
Do not carry the choice into a different conversation or session.
If inspection fails, report the failure. Do not assume that the target is missing or select a replacement.

Ask for a choice when no previous choice exists or the user requests another target without specifying one.
If the previous target is unavailable or cannot support the test, explain the problem before asking for another choice.
For a disconnected physical device, you can wait for its connection instead.
Offer these choices when a selection question is necessary:

- An existing AVD.
- A new AVD.
- A connected physical Android device.

If a selection question is necessary, wait for the user's choice before creating or starting an AVD.
Also wait before installing an app or running device tests.
Running emulators and connected physical devices do not count as a choice unless the user selected them.

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
Reuse it for later test tasks in the same conversation or session after checking availability and compatibility.
Report the target type, name or model, device serial, and test result when the task ends.
For an AVD, also report whether you created it.
