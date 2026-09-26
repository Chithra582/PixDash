# Duties & Role Segregation: PixDash Agent

To deliver responsive gameplay and clean Unity architecture, PixDash Agent segregates responsibilities across four distinct roles.

## 1. Gameplay Systems Architect (`maker`)
- Authors player kinematic controllers, jump physics formulas, dash mechanics, and wall-sliding algorithms.
- Configures input mappings supporting both modern Unity Input System and legacy axes.
- Formulates enemy finite state machines (FSM) for patrol, detection, and attack states.

## 2. Scene & Prefab Integrator (`executor`)
- Structures and validates prefab hierarchies under `Assets/_Project/Prefabs/`.
- Configures 2D Tilemap collision boundaries, CompositeCollider2D setups, and tile palletes.
- Integrates sprite animations, Mecanim state transitions, and audio triggers.

## 3. Physics & Performance Checker (`checker`)
- Audits Rigidbody2D and Collider2D collision layer matrices to eliminate unnecessary collision calculations.
- Verifies that all physics operations occur inside `FixedUpdate()` with proper interpolation.
- Inspects code for garbage collection allocations (boxing, closures, LINQ) inside high-frequency update loops.

## 4. Gamedev Pedagogy Auditor (`auditor`)
- Reviews contributor pull requests against issue specifications, ensuring tasks remain beginner-scoped.
- Validates student learning privacy (FERPA/GDPR), ensuring zero user telemetry or unauthorized tracking.
- Maintains comprehensive documentation of game mechanics and inspector parameters.
