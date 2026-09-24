<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:2b1055,100:d53a9d&height=150&section=header&text=Donu%20Task&fontSize=46&fontColor=ffffff&animation=fadeIn&desc=Organize%20your%20day.%20Just%20tell%20Donu%20what%20you%20need.&descAlignY=75&descSize=16" width="100%" alt="Donu Task" />

![iOS](https://img.shields.io/badge/iOS-000000?style=flat-square&logo=apple&logoColor=white)
![Swift](https://img.shields.io/badge/Swift-F05138?style=flat-square&logo=swift&logoColor=white)
![SwiftUI](https://img.shields.io/badge/SwiftUI-0D96F6?style=flat-square&logo=swift&logoColor=white)
![SwiftData](https://img.shields.io/badge/SwiftData-0D96F6?style=flat-square&logo=swift&logoColor=white)
![CloudKit](https://img.shields.io/badge/CloudKit-5AC8FA?style=flat-square&logo=icloud&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![Status](https://img.shields.io/badge/status-on%20the%20App%20Store-d53a9d?style=flat-square)

[![Download on the App Store](https://img.shields.io/badge/Download_on_the-App_Store-0D96F6?style=for-the-badge&logo=appstore&logoColor=white)](https://apps.apple.com/us/app/donu-task/id6749896283)

</div>

> 🔒 **The source code is private.** This repo explains what the app does, how it is built and the main decisions I made. I'm happy to walk through the real code in an interview.

## What it is

Donu Task is a task manager for iPhone with a built in AI assistant called Donu. You can tap through the app like any to do list, or just write what you need:

> _"meeting tomorrow at 3"_
> _"create a work category with my Monday tasks"_

Donu understands the sentence and the app does the work: it creates the task, sets the date and time, or moves things around.

## Features

- 🤖 **Talk to Donu.** Natural language becomes real actions inside the app.
- 📴 **Offline first.** Everything is saved on the device with SwiftData. No internet needed to use the app.
- ☁️ **Sync with iCloud.** CloudKit keeps tasks in sync between devices.
- 📅 **Today view, categories and routines** to plan the day.
- 📊 **Progress stats** with Swift Charts.
- 🧩 **Home screen widget** with WidgetKit.
- 🔔 **Reminders** with local notifications.
- 🖼️ **Share your progress** as an image with `ImageRenderer`.
- ✨ **Polish:** custom animations, haptics, onboarding and full dark mode.
- 💎 **Premium plan** with a paywall built on StoreKit.
- 🌎 **More than one language** with String Catalogs.

## How the AI works

The AI never touches the database directly. It only suggests actions, and the app decides how to run them.

```mermaid
sequenceDiagram
    actor U as User
    participant A as Donu app (SwiftUI)
    participant F as Cloud Function (Node.js)
    participant G as Gemini 2.5 Flash
    participant D as SwiftData

    U->>A: "meeting tomorrow at 3"
    A->>F: message + current task state
    F->>G: prompt with the state and the action schema
    G-->>F: JSON actions
    F-->>A: [{ "action": "createTask", ... }]
    A->>D: run it natively
    D-->>U: new task on the list
```

**Why it works this way**

- **The model gets the full task state on every request**, so it knows what already exists. "Move my Monday tasks to Friday" works because Donu can see those tasks.
- **Structured JSON output** instead of free text. The app only runs actions it knows, so a strange answer from the model does not turn into random changes.
- **The API key stays on the server.** The app only talks to the Cloud Function, never to Gemini directly.
- **Actions run in SwiftData on the device**, so the result is instant and still works with CloudKit sync.

## Architecture

**MVVM with Clean Architecture.** Data, domain and presentation are separate layers.

<p align="center">
  <img src="assets/project-structure.svg" width="100%" alt="Donu Task project structure in Xcode" />
</p>

<sub>The real folder structure of the app. Only file names are shown, the code stays private.</sub>

## Quality

- ViewModel tests for categories and items.
- Every release goes through **Swift Testing** and **TestFlight** before the App Store.

## Tech stack

| Area | Tools |
|---|---|
| UI | SwiftUI, Swift Charts, custom animations, haptics |
| Data | SwiftData (offline first), CloudKit sync |
| AI | Gemini 2.5 Flash through Firebase Cloud Functions (Node.js) |
| Extensions | WidgetKit, UserNotifications, ImageRenderer |
| Payments | StoreKit |
| Architecture | MVVM, Clean Architecture |
| Testing | Swift Testing, XCTest, TestFlight |

## Screenshots

_Coming soon._

---

<div align="center">

Built by [Charles Yamamoto](https://github.com/Lophiester) · [LinkedIn](https://www.linkedin.com/in/charles-yamamoto-26699b203/) · [yamaflare.com](https://yamaflare.com)

</div>
