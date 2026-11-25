# Awesome Agent Phone Use [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

Ways for AI agents to operate real phones and mobile apps.

Most GUI-agent lists are paper lists, and most of those papers run on the Android Emulator. This one covers how you actually get an agent onto a phone: app APIs, software control of a real handset, device clouds, emulators, and robots that tap the screen.

Almost all of the research talks to an Android emulator over adb. For a real iPhone the options are much thinner (XCUIAutomation, WebDriverAgent, idb, a few MCP servers, some commercial farms), and a real SIM is another problem on top. For papers, see the research lists at the bottom.

## Contents

- [Start here](#start-here)
- [Official app APIs](#official-app-apis)
- [Agent frameworks and MCP servers](#agent-frameworks-and-mcp-servers)
- [Models](#models)
- [Automation and device control](#automation-and-device-control)
- [Benchmarks and datasets](#benchmarks-and-datasets)
- [Device clouds](#device-clouds)
- [Emulators and virtual devices](#emulators-and-virtual-devices)
- [Hardware](#hardware)
- [Related lists](#related-lists)
- [Disclosure](#disclosure)

## Start here

Rough guide to the options. It's about how input gets to the app, not which tool is best.

<!--lint ignore table-pipe-alignment-->
| Approach | Use when | How input reaches the app | Typical device |
| --- | --- | --- | --- |
| Official app APIs | The app (or the OS) already exposes the action you need. | Documented intents, shortcuts, or on-device tools. No GUI driving. | Real or virtual, but only for APIs the app actually ships. |
| Emulator or simulator | You are developing, iterating, or running a public benchmark. | Software-generated input on a virtual device. | No physical handset. Telephony, if present, is usually simulated, not a real SIM. |
| Software on a real device | You have (or can attach) a physical phone and want an agent or script to drive it. | Injected events through adb, UI Automator, accessibility, WebDriverAgent, or idb. | A physical Android or iPhone. A real radio is possible if the handset has a SIM; that is a property of the device, not of the control method. |
| Cloud or hosted phone | You need many models or OS versions, or you do not want to keep a device library. | Almost always the same software-injected input, delivered over the network. | Sometimes a physical handset in a rack, sometimes a virtual phone. Read the vendor's device-type list. |
| Physical robot | The work depends on contact with the glass (or with buttons and sensors), typically for QA today. | A mechanism touches the screen. Computer vision often watches it. | A real physical device. Agent-focused hardware here is early, so check what actually ships. |

## Official app APIs

If the app exposes the action directly, you don't need to drive the UI. You only get what the developer chose to expose.

- [App Intents](https://developer.apple.com/documentation/appintents) - Apple's official API for exposing in-app actions to Siri and other system agents, without tapping the screen.
- [AppFunctions](https://developer.android.com/ai/appfunctions) - Experimental Android 16+ platform API (with a Jetpack library) for exposing in-app actions as on-device tools that agents can call. Similar idea to MCP, but on the device.

## Agent frameworks and MCP servers

Systems that let a model operate a phone through screenshots, UI trees, or tool calls. Most of the research ones target Android.

- [Mobilerun](https://github.com/droidrun/mobilerun) - Open-source framework for running agents on real and virtual Android and iOS devices, with a hosted cloud option. Android goes through adb or a portal app; a local iPhone is documented as software control via `mobilerun-ios` and WebDriverAgent.
- [mobile-mcp](https://github.com/mobile-next/mobile-mcp) - MCP server that lets an MCP-compatible agent drive iOS and Android emulators, simulators, and real devices from accessibility snapshots, with screenshots as a fallback.
- [AppAgent](https://github.com/TencentQQGYLab/AppAgent) - Multimodal Android agent that learns an app by exploring or watching a demo, then completes tasks through taps and swipes. A [v2 paper](https://arxiv.org/abs/2408.11824) exists; public code is for v1. ([paper](https://arxiv.org/abs/2312.13771)).
- [Mobile-Agent](https://github.com/X-PLUG/MobileAgent) - Visual Android and HarmonyOS agent family from X-PLUG, from the original multimodal agent through a multi-agent [v2](https://arxiv.org/abs/2406.01014) to [v3 / GUI-Owl](https://arxiv.org/abs/2508.15144), including the longer-horizon [Mobile-Agent-E](https://arxiv.org/abs/2501.11733) variant. iOS is not supported. ([v1 paper](https://arxiv.org/abs/2401.16158)).
- [AutoDroid](https://github.com/MobileLLM/AutoDroid) - LLM-powered Android task automation built on DroidBot, run from a host over adb. ([paper](https://arxiv.org/abs/2308.15272)).
- [AutoDroid-V2](https://github.com/MobileLLM/AutoDroid-V2) - Follow-up that drives Android GUI agents by generating code, aimed at smaller models, with public code and a reference model.
- [Open-AutoGLM](https://github.com/zai-org/Open-AutoGLM) - Open phone-agent framework and AutoGLM-Phone model weights that understand the screen and act through adb, with human takeover for logins and captchas. ([paper](https://arxiv.org/abs/2411.00820)).
- [DigiRL](https://github.com/DigiRL-agent/digirl) - Trains Android device-control agents with reinforcement learning on real-world tasks. ([paper](https://arxiv.org/abs/2406.11896)).
- [Gemini Computer Use](https://ai.google.dev/gemini-api/docs/computer-use) - API for screenshot-to-action agents, including a mobile environment with Android-oriented actions. The client still has to execute those actions on a device.

## Models

Vision-language and GUI models commonly used as the eyes and policy for phone-use agents. Several also cover desktop and web.

- [UI-TARS](https://github.com/bytedance/UI-TARS) - Native GUI agent family (v1, 1.5, and [v2](https://arxiv.org/abs/2509.02544)) that goes from screenshot to action, evaluated on mobile and desktop GUIs. ([v1 paper](https://arxiv.org/abs/2501.12326)).
- [UI-Venus](https://github.com/inclusionAI/UI-Venus) - Open foundation GUI agent for mobile, web, and desktop, with a 2026 [v2](https://arxiv.org/abs/2609.00028) release. ([v1 paper](https://arxiv.org/abs/2508.10833)).
- [CogAgent](https://github.com/zai-org/CogAgent) - Visual language model trained to read GUIs (screens, icons, text) and used as a backbone for phone and desktop control research. ([paper](https://arxiv.org/abs/2312.08914)).
- [ShowUI](https://github.com/showlab/ShowUI) - Vision-language model for GUI grounding and screenshot-to-action on mobile and desktop interfaces. ([paper](https://arxiv.org/abs/2411.17465)).
- [Aguvis](https://github.com/xlang-ai/aguvis) - Pure-vision agents for autonomous GUI interaction across mobile and desktop screens. ([paper](https://arxiv.org/abs/2412.04454)).
- [Ferret-UI](https://machinelearning.apple.com/research/ferretui-mobile) - Apple research on grounded mobile UI understanding with multimodal LLMs. A paper and write-up, not a shipping agent product. ([paper](https://arxiv.org/abs/2404.05719)).

## Automation and device control

What most of the frameworks above are built on. Also fine on their own for plain scripts.

- [Android Debug Bridge (adb)](https://developer.android.com/tools/adb) - Official command-line bridge to Android devices and emulators, and the default transport for almost every Android agent.
- [UI Automator](https://developer.android.com/training/testing/other-components/ui-automator) - Official Android API for crossing app boundaries and reading a UI tree on a device or emulator.
- [XCUIAutomation](https://developer.apple.com/documentation/xcuiautomation) - Apple's UI testing and automation API for finding elements and sending taps, swipes, and text on iOS.
- [WebDriverAgent](https://github.com/appium/WebDriverAgent) - WebDriver server for iOS, built on XCTest, and the usual way Appium and several agent tools inject input into an iPhone.
- [idb](https://github.com/meta/idb) - Meta's iOS Development Bridge: a command-line and library interface for automating simulators and real iPhones.
- [pymobiledevice3](https://github.com/doronz88/pymobiledevice3) - Pure-Python toolkit for talking to real iPhones (pairing, tunnels, diagnostics, and related services), often used under iOS automation stacks.
- [Appium](https://appium.io/docs/en/latest/) - Cross-platform UI automation on top of XCUITest / WebDriverAgent and Android UI Automator, and a common backend for agent tools. ([code](https://github.com/appium/appium)).
- [Maestro](https://github.com/mobile-dev-inc/Maestro) - YAML-based mobile UI testing for Android devices and emulators and for the iOS Simulator. A physical iPhone is not a documented first-class path. ([docs](https://docs.maestro.dev)).
- [uiautomator2](https://github.com/openatx/uiautomator2) - Python wrapper around Android UI Automator, widely used in research agents and custom Android automation.
- [DroidBot](https://github.com/honeynet/droidbot) - Input-generation and UI-exploration tool for Android, and the automation backbone under AutoDroid.
- [scrcpy](https://github.com/Genymobile/scrcpy) - Mirrors and controls an Android device from a desktop, which is useful while watching or intervening in an agent run.
- [DeviceFarmer STF](https://github.com/DeviceFarmer/stf) - Open-source web farm (formerly OpenSTF) for remote software control and screen streaming of physical Android devices.

## Benchmarks and datasets

For training and evaluating phone agents. Nearly all Android, mostly on the emulator.

- [AndroidWorld](https://github.com/google-research/android_world) - Live-emulator benchmark from Google Research: 116 hand-crafted tasks across 20 apps, dynamically instantiated. Still the benchmark most papers report. ([paper](https://arxiv.org/abs/2405.14573)).
- [MobileWorld](https://github.com/Tongyi-MAI/MobileWorld) - Harder online benchmark (ACL 2026) with longer-horizon, cross-app, agent-user, and MCP-augmented tasks. Made because AndroidWorld scores were topping out. ([paper](https://arxiv.org/abs/2512.19432)).
- [MobileSafetyBench](https://github.com/jylee425/mobilesafetybench) - Emulator testbed for the safety and helpfulness of mobile device-control agents (AAAI 2026).
- [Android in the Wild](https://github.com/google-research/google-research/tree/master/android_in_the_wild) - Large-scale dataset of Android device-control demonstrations, and a common pretraining and imitation source. ([paper](https://arxiv.org/abs/2307.10088)).
- [AndroidControl](https://arxiv.org/abs/2406.03679) - Dataset and study on how data scale affects UI-control agents, with both low-level and high-level instructions.
- [AndroidEnv](https://github.com/google-deepmind/android_env) - DeepMind RL environment that exposes the Android Emulator as a control interface. Older than the current VLM agents. ([paper](https://arxiv.org/abs/2105.13231)).
- [B-MoCA](https://github.com/jylee425/b-moca) - Benchmark for mobile-device control agents across diverse device configurations instead of one fixed emulator. ([paper](https://arxiv.org/abs/2404.16660)).
- [LlamaTouch](https://github.com/LlamaTouch/LlamaTouch) - Testbed for evaluating mobile UI automation, including how to decide a task actually succeeded. ([paper](https://arxiv.org/abs/2404.16054)).
- [SPA-Bench](https://github.com/ai-agents-2030/SPA-Bench) - Smartphone-agent benchmark with a wider mix of apps and task types. ([paper](https://arxiv.org/abs/2410.15164)).
- [AndroidLab](https://github.com/THUDM/Android-Lab) - Training and systematic benchmarking environment for Android agents, with a documented task suite. ([paper](https://arxiv.org/abs/2410.24024)).
- [AITK](https://github.com/YuxiangChai/AITK) - Maintained Android Interactive Toolkit for the Android Agent Arena benchmark (201 tasks across 20 apps). The original A3 repo is archived. ([paper](https://arxiv.org/abs/2501.01149)).

## Device clouds

Rent real or virtual phones remotely, mostly for QA. Input is software, and it's worth checking whether you're getting a physical device.

- [Firebase Test Lab](https://firebase.google.com/docs/test-lab) - Google-hosted lab for instrumented tests on real and virtual Android devices, and for XCTest on real iOS devices.
- [AWS Device Farm](https://docs.aws.amazon.com/devicefarm/latest/developerguide/welcome.html) - Amazon's device farm for automated and remote-access testing on hosted real Android and iOS devices.
- [BrowserStack App Live](https://www.browserstack.com/app-live) - Interactive manual testing on a cloud of real Android and iOS devices, with a separate product line for automated app tests.

## Emulators and virtual devices

Virtual phones, local or hosted. Fine for most development and most benchmarks. No real SIM, radio or full set of sensors.

- [Android Emulator](https://developer.android.com/studio/run/emulator) - Official Android Virtual Device. It can simulate calls, SMS, and signal strength through an emulated modem, and it does not contain a real SIM or cellular radio.
- [Xcode Simulator](https://developer.apple.com/documentation/xcode/running-your-app-on-simulated-or-physical-devices) - Official iOS Simulator: a simulated device on a Mac, not a physical iPhone. Some hardware and services exist only on a real device.
- [Genymotion](https://www.genymotion.com) - Commercial Android emulator, with a corresponding cloud and virtual-device offering, widely used in development and testing.
- [Waydroid](https://github.com/waydroid/waydroid) - Container-based Android on a Linux host, using the host kernel rather than the official Android Studio emulator.
- [Anbox Cloud](https://canonical.com/anbox-cloud) - Canonical's platform for running Android in system containers in the cloud, for streaming and automation.
- [Clousd](https://clousd.com) - Hosted Android 12-17 virtual phones you control from the browser, over adb, a REST API, a Python SDK, or an MCP server ([SDK and MCP server](https://github.com/Clousd-Android/clousd-sdk)).
- [Corellium](https://www.corellium.com) - Virtualized ARM-based iOS and Android devices, used heavily in security research. Not a conventional x86 Android emulator, and not a physical handset.

## Hardware

A machine physically touches the screen (sometimes buttons too). The phone sees real touches, not events injected through adb, accessibility or WebDriverAgent.

- [Tapster](https://tapster.io/tapster-robotics-product-page) - Delta robot that taps and swipes a phone or tablet touchscreen, with a mounted camera, aimed at functional testing and device provisioning. An earlier open-source design lives in the [tapsterbot](https://github.com/tapsterbot/tapsterbot) repo.
- [Mobot](https://www.mobot.io) - Robots that tap real iOS and Android devices for app QA, run as a managed service where people review the failures. ([FAQ](https://www.mobot.io/faq)).
- [Pichnett](https://pichnett.com) - A robot that taps a real iPhone for AI agents while a camera watches the screen. Still a prototype, not yet shown doing a live task; early access opens 15 January 2027.

## Related lists

Better places for GUI-agent papers and models.

- [OS Agents survey](https://aclanthology.org/2025.acl-long.369) - ACL 2025 oral survey of multimodal agents that operate computers, phones, and browsers. ([arXiv](https://arxiv.org/abs/2508.04482), [repo](https://github.com/OS-Agent-Survey/OS-Agent-Survey)).
- [awesome-mobile-agents](https://github.com/aialt/awesome-mobile-agents) - Research-oriented list of mobile-agent methods, datasets, and benchmarks.
- [Awesome-GUI-Agent](https://github.com/showlab/Awesome-GUI-Agent) - Broader GUI-agent catalogue covering desktop, web, and mobile, with papers and code links.

## Disclosure

We're building Pichnett (under Hardware), so we're biased. PRs adding competitors are welcome and get the same treatment as anything else.

## Contributing

Additions and corrections are welcome. See [contributing.md](CONTRIBUTING.md) for the rules: working links, one-line factual descriptions, no performance or compatibility claims, and no unreleased product presented as shipping.
