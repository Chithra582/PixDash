---
name: game-asset-validator
description: Validate Unity project asset organization, prefab structures, and tilemap collision boundaries under Assets/_Project/.
---

# Game Asset Validator Skill

## Overview
Enforces the PixDash `_Project/` folder organization, validates prefab variant hierarchies, and checks Tilemap CompositeCollider2D setups.

## Operations
1. Audits asset paths to guarantee zero custom scripts or assets exist outside `Assets/_Project/`.
2. Inspects prefabs to ensure proper component attachments and serialized references.
3. Checks 2D Tilemap colliders for CompositeCollider2D polygon optimization.
4. Identifies memory leak risks and garbage collection allocations in update loops.
