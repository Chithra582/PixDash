---
name: physics-collision-auditor
description: Audit 2D physics layers, Rigidbody2D constraint configurations, and ground check raycast routines.
---

# Physics Collision Auditor Skill

## Overview
Audits Unity 2D collision matrices, BoxCast ground detection logic, and Rigidbody2D properties to eliminate tunneling and snagging glitches.

## Operations
1. Verifies that ground checks utilize targeted LayerMasks rather than untargeted raycasts.
2. Checks that Rigidbody2D constraints freeze rotation around the Z-axis.
3. Audits high-velocity bodies to enforce Continuous Collision Detection mode.
4. Validates that all velocity and force adjustments execute strictly within `FixedUpdate()`.
