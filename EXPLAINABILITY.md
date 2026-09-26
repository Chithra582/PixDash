# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **PixDash Agent** (`pixdash-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** PixDash Agent (`pixdash-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Education / 2D Game Mechanics & Unity Component Architecture  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), FERPA, GDPR  

---

## How the Agent Decides

PixDash Agent is an autonomous 2D platformer gameplay mechanics architect and Unity C# component intelligence agent designed for **PixDash**, an open-source 2D pixel platformer game. The agent guides the implementation of responsive player kinematics, enemy patrol state machines, collision detection matrices, and modular game asset hierarchies adhering to Unity best practices.

### 1. Decision Architecture

The gameplay mechanics, physics calculations, and state evaluation process operates across a deterministic, five-stage pipeline:

```
Player Input (Keyboard / Gamepad Axis Events: Move, Jump, Dash)
    │
    ▼
[Stage 1: Input Polling & State Sampling]
    │  - Polls raw horizontal input ($x \in [-1.0, 1.0]$) inside `Update()`
    │  - Evaluates jump buffer timer (capturing jump presses $100\text{ms}$ prior to landing)
    │  - Updates coyote timer (providing $150\text{ms}$ jump grace window after leaving ledges)
    ▼
[Stage 2: Kinematic Physics & Jump Curve Calculation]
    │  - Executes strictly inside `FixedUpdate()` using `Time.fixedDeltaTime`
    │  - Computes horizontal velocity with acceleration/deceleration smoothing
    │  - Applies asymmetric vertical gravity:
    │      ├── Standard gravity when rising with jump button held
    │      ├── High fall gravity ($2.5 \times g$) when falling for punchy descent
    │      └── Low-hop gravity ($2.0 \times g$) when jump button is released early
    ▼
[Stage 3: Collision Detection & Layer Filtering]
    │  - Performs dual BoxCast2D downward ground checks filtered by `Ground` LayerMask
    │  - Detects wall contact via horizontal raycasts for wall-slide and wall-jump mechanics
    │  - Evaluates trigger overlap events with coin collectibles and level hazard spikes
    ▼
[Stage 4: Enemy Patrol & Finite State Machine (FSM)]
    │  - Evaluates enemy FSM transitions:
    │      ├── State 1: Patrol (Linear movement between waypoint markers)
    │      ├── State 2: Edge/Wall Detection (Reverses direction upon ground void or wall hit)
    │      └── State 3: Player Interaction (Damages player on side contact; defeated if stomped from above)
    ▼
[Stage 5: Game State & Level Progression Evaluation]
    │  - Updates collectible count and HUD score displays
    │  - Evaluates player health; triggers respawn state if health reaches 0 or player falls into death pit
    │  - Unlocks exit portal upon collecting required level keys/gems
    ▼
Unity 2D Viewport & Responsive Player Game Experience
```

### 2. 2D Platformer Physics & Game-Feel Rubric

To ensure gameplay feels tight, responsive, and satisfying, the agent applies explicit game-feel design rules:

| Mechanic | Implementation Logic | Target Parameter Range |
|---|---|---|
| **Variable Jump Height** | Higher gravity multiplier when jump key is released early | Fall multiplier: $2.5\times$, Low-jump multiplier: $2.0\times$ |
| **Coyote Time** | Allows jump within brief grace window after stepping off a ledge | $100\text{ms} - 150\text{ms}$ |
| **Jump Buffering** | Queues jump input pressed slightly before touching ground | $100\text{ms}$ pre-landing buffer |
| **Ground Checking** | Downward `Physics2D.BoxCast` matching player collider width | Ray length: $0.05\text{m} - 0.1\text{m}$ beyond collider edge |
| **Horizontal Drag** | Smooth linear interpolation between current velocity and target | Acceleration: $0.05\text{s}$, Deceleration: $0.08\text{s}$ |
| **Rigidbody2D Setup** | Dynamic body with continuous collision and frozen Z-axis | `Interpolate: Interpolate`, `Constraints: Freeze Rotation Z` |

### 3. Responsiveness & Quality Scoring Formula

The agent calculates a Gameplay Kinematic Quality Score $Q_{\text{game}} \in [0.0, 1.0]$:

$$Q_{\text{game}} = 0.35 \cdot S_{\text{fixed}} + 0.30 \cdot S_{\text{buffer}} + 0.20 \cdot S_{\text{gc}} + 0.15 \cdot S_{\text{hierarchy}}$$

- **Physics Loop Purity ($S_{\text{fixed}}$)**: $1.0$ if all Rigidbody2D velocity and force applications occur exclusively inside `FixedUpdate()`; $0.0$ if applied in `Update()`.
- **Game-Feel Responsiveness ($S_{\text{buffer}}$)**: Evaluates presence of coyote time and jump buffering mechanisms.
- **Zero-GC Allocation ($S_{\text{gc}}$)**: Verifies that no heap allocations (`new`, closures, LINQ queries) occur inside high-frequency update loops.
- **Folder Hierarchy Compliance ($S_{\text{hierarchy}}$)**: Validates that all scripts, prefabs, and assets reside strictly under `Assets/_Project/`.

Implementations achieving $Q_{\text{game}} \ge 0.85$ provide optimal platformer game feel.

### 4. Thresholding & Refusal Decision Criteria

PixDash Agent enforces strict architectural guardrails:
- **`_Project/` Hierarchy Violation Refusal**: Assets or scripts created outside the designated `Assets/_Project/` root are flagged for immediate relocation to prevent multi-contributor merge conflicts.
- **Monolithic God-Script Refusal**: Submissions that combine player movement, input polling, sound effects, and UI rendering into a single class are rejected; functionality must be partitioned into modular components (`PlayerMovement`, `PlayerInput`, `PlayerAudio`).
- **Unconstrained Z-Rotation Refusal**: Any Rigidbody2D setup lacking frozen Z-axis rotation is flagged, as unconstrained rotation causes character sprites to tilt and topple upon collision.
- **Direct Transform Manipulation Refusal**: Modifying `transform.position` directly on physics-driven entities is rejected in favor of `Rigidbody2D.velocity` or `Rigidbody2D.MovePosition` to prevent collider tunneling.

### 5. Fallback Decision Mechanism

PixDash Agent incorporates fault-tolerant fallbacks for robust gameplay execution:
- **Dual-Raycast Ground Check Fallback**: If primary BoxCast collision fails due to irregular tile geometry, the controller falls back to dual raycasts anchored at the bottom-left and bottom-right collider corners.
- **Level Spawn Point Fallback**: If a level scene lacks an active `PlayerSpawnPoint` transform reference, the game manager automatically instantiates the player at the coordinate origin $(0, 0, 0)$ with a safety platform.
- **Model Fallback Cascade**: Game design assistance requests utilize `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 6. Human-in-the-Loop Governance

PixDash enforces complete developer and designer sovereignty:
- **Serialized Inspector Tunability**: All physics parameters (move speed, jump force, fall multiplier, coyote duration) are exposed via `[SerializeField]` private fields, allowing level designers to tune game feel in the Unity Inspector without editing C# code.
- **Modular Prefab Overrides**: Contributors work through isolated prefab variants, ensuring changes can be reviewed, adjusted, or reverted without corrupting master scene files.
- **Contributor Scope Adherence**: Pull requests are audited against issue checklists, ensuring beginner contributors build strictly within designated issue boundaries.

---

## The Data It Uses

PixDash Agent operates under strict privacy and local-first game asset standards.

### 1. Ingested Input Data

The agent processes only local game code, project settings, and user inputs:
- **Unity C# Source Scripts**: Movement controllers, enemy AI behaviors, collectible scripts, and UI managers located in `Assets/_Project/Scripts/`.
- **Unity Project Settings**: Physics2D collision layer matrix, project input bindings, and Tag/Layer definitions.
- **Real-Time Controller Inputs**: Raw axis values and button states polled from the local keyboard or gamepad.

### 2. Configuration & Reference Data

- **Physics2D Constants**: Global gravity vector ($g = -9.81\text{ m/s}^2$), fixed physics timestep ($\Delta t = 0.02\text{s}$ / $50\text{Hz}$), and default contact offsets.
- **Collision Layer Matrix**: Explicit collision permission table isolating `Player`, `Enemies`, `Ground`, `Hazards`, and `Collectibles`.
- **Folder Topology Guidelines**: Standard directory blueprint defined in `README.md`.

### 3. Base Model & Inference Lineage

- **Deterministic Unity Physics Engine**: Box2D physics solver integrated into Unity executing all kinematics, collision responses, and trigger overlaps deterministically.
- **AI Game Design Copilot**: Frontier foundation models (Google Gemini `gemini-2.0-flash`, OpenAI `gpt-4o`, Anthropic `claude-3-5-sonnet`) utilized strictly for code architecture review and algorithmic guidance.
- **Zero Player Data Training**: Game telemetry and local contributor code are never uploaded or used for model training.

### 4. Data Privacy, Storage, and Retention

- **0-Byte External Data Transmission**: PixDash is a completely offline, local single-player game. Zero player data, inputs, or telemetry packets are transmitted over the network.
- **Local-Only Save State**: Player score, unlocked levels, and preferences are stored exclusively on the player's local device using Unity `PlayerPrefs` or local JSON save files.
- **FERPA & GDPR Compliance**: In full accordance with FERPA and GDPR (Articles 5 & 28), student contributors and players are protected with zero PII harvesting, tracking cookies, or commercial monetization.

---

## Limitations

Understanding the operational boundaries and technical constraints of PixDash Agent is essential for robust 2D game engineering.

### 1. Tunneling on High-Velocity Projectiles
- **Limitation**: Fast-moving objects (bullets, high-speed dash) can pass through thin colliders in a single physics frame if discrete collision detection is used.
- **Mitigation**: The agent prescribes `CollisionDetectionMode2D.Continuous` on all high-velocity rigidbodies, preventing tunneling artifacts.

### 2. Frame-Rate Dependent Variable Delta Time in Render Loops
- **Limitation**: Applying movement physics in `Update()` using `Time.deltaTime` results in inconsistent jump heights and velocities when frame rates fluctuate.
- **Mitigation**: The agent strictly enforces that all velocity modifications and force applications occur inside `FixedUpdate()` using `Time.fixedDeltaTime`.

### 3. Garbage Collection Spikes from Runtime String/LINQ Allocations
- **Limitation**: Frequent string concatenation (e.g. `scoreText.text = "Score: " + score;`) or LINQ queries inside update loops generate managed memory garbage, causing periodic micro-stutter when the Unity Garbage Collector runs.
- **Mitigation**: The agent advises caching string buffers, utilizing `StringBuilder`, and avoiding LINQ inside high-frequency loops.

### 4. Tilemap Seam Glitches & Ghost Collisions
- **Limitation**: Individual box colliders on adjacent tilemap cells can cause players to snag on internal tile seams while sliding across flat ground.
- **Mitigation**: The agent recommends using `TilemapCollider2D` paired with a `CompositeCollider2D` (set to `Geometry Type: Polygons`), generating seamless continuous collision contours.

### 5. Scope Boundaries
- **Limitation**: PixDash is architected as an offline, single-player 2D platformer; it does not implement client-server authoritative netcode for real-time multiplayer.
- **Mitigation**: Architecture guidelines keep game state cleanly decoupled from rendering, facilitating future multiplayer extensions via dedicated netcode packages.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Physics & jump dynamics rubric | Section 2 | Verified |
| - Responsiveness & quality scoring formula | Section 3 | Verified |
| - Thresholding & refusal decision criteria | Section 4 | Verified |
| - Fallback decision mechanism | Section 5 | Verified |
| - Human-in-the-loop governance & inspector tuning | Section 6 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested C# scripts & controller inputs | Section 1 | Verified |
| - Configuration & Physics2D constants | Section 2 | Verified |
| - Base model lineage & deterministic Box2D engine | Section 3 | Verified |
| - Data privacy, 0-byte transmission & FERPA/GDPR | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Tunneling on high-velocity entities | Section 1 | Verified |
| - Frame-rate dependent variable delta time | Section 2 | Verified |
| - Garbage collection spikes & memory allocation | Section 3 | Verified |
| - Tilemap seam glitches & composite colliders | Section 4 | Verified |
| - Scope boundaries & local offline design | Section 5 | Verified |
