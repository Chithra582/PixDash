# 🎮 PixDash

[![OpenGAP Spec 0.1.0](https://img.shields.io/badge/OpenGAP-0.1.0-blue.svg)](https://opengitagent.org)
[![GitAgent Passport](https://img.shields.io/badge/GitAgent%20Passport-Ready-brightgreen.svg)](https://app.hidevs.xyz/passport/submit)
[![Category](https://img.shields.io/badge/Category-Education-purple.svg)](https://app.hidevs.xyz/passport/submit)
[![Compliance](https://img.shields.io/badge/Compliance-FERPA%20%7C%20GDPR-orange.svg)](EXPLAINABILITY.md)

**PixDash** is a beginner-friendly **2D pixel platformer** built using **Unity**.

PixDash focuses on:

* Learning **core game development concepts**
* Helping even **non-game-devs** build their first unity game

---

## 🚀 What Does PixDash Do?

PixDash is a classic 2D platformer where the player:

* Runs and jumps across platforms
* Escapes enemies
* Collects coins
* Unlocks exits to complete levels

---

## 🧷 Getting Started

### Prerequisites

* Git
* Unity Hub
* Unity Editor **6.3 LTS(6000.3.2f1)**

---

### 🔧 Setup Instructions

1. **Fork** this repository
2. **Clone** your fork locally
3. Open **Unity Hub**
4. Click **Add → Add project from disk**
5. Select the cloned `PixDash` folder
6. Open the project

> ⚠️ Do **NOT** create a new Unity project.
> You must open the cloned repository directly.

---

## 🛠 Tech Stack

* **Game Engine:** Unity (2D)
* **Language:** C#
* **Version Control:** Git & GitHub

---

## 📁 Unity Folder Structure

All PixDash content lives inside `_Project`:

```
Assets/
└── _Project/
    ├── Scripts/
    │   ├── Player/
    │   ├── Enemies/
    │   ├── Platforms/
    │   ├── UI/
    │   ├── Camera/
    │   └── Managers/
    │
    ├── Prefabs/
    │   ├── Player/
    │   ├── Enemies/
    │   ├── Platforms/
    │   ├── Collectibles/
    │   └── UI/
    │
    ├── Scenes/
    ├── Art/
    ├── Audio/
    ├── UI/
    └── Settings/
```

> Contributors **must follow this structure** strictly.

---

## 🧩 How Issues Work

Each GitHub issue:

* Explains **what to build**
* Specifies **which folder to use**
* Tells you **what GameObjects to create**
* Lists **components to add**

* Do **not** add extra features beyond the issue scope

---

## 🤝 How to Contribute

1. Pick an open issue
2. Comment on the issue to claim it
3. Work strictly within the folders mentioned in the issue
4. Test your changes in Unity
5. Commit with a clear message
6. Create a Pull Request


---

## 📢 Communication

If you are:

* Stuck on an issue
* New to Unity
* Unsure about a task

Ask questions on the **Discord channel**.
We are happy to help 😊

---

---

## GitAgent Passport Qualification

This repository is fully compliant with the **OpenGAP Spec 0.1.0** standard and qualified for the **HiDevs GitAgent Passport**:

- **Checkpoint 1 (Validate):** Verified OpenGAP spec 0.1.0 compliance via [`agent.yaml`](agent.yaml), [`SOUL.md`](SOUL.md), [`skills/`](skills/), and [`tools/`](tools/).
- **Checkpoint 2 (Explain):** Comprehensive 2D platformer kinematics governance and Unity component architecture report in [`EXPLAINABILITY.md`](EXPLAINABILITY.md) detailing jump physics, coyote timers, BoxCast ground checks, and folder discipline.
- **Checkpoint 3 (Export):** Cross-framework export compatibility tested across OpenAI SDK, CrewAI, Claude Code, and Lyzr.
- **Target Category:** **`Education`** (2D Game Mechanics & Unity Component Architecture).

---

## 🏁 Project Goal

By the end of PixDash, contributors will have:

* Built a **complete 2D platformer**
* Learned Unity fundamentals
* Understood real-world game dev workflows
* Contributed to an open-source project

---

