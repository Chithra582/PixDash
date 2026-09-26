---
name: enemy-fsm-coordinator
description: Coordinate enemy finite state machines for linear waypoint patrols, edge detection, and player interactions.
---

# Enemy FSM Coordinator Skill

## Overview
Designs lightweight, modular finite state machines (FSM) in Unity C# for enemy AI behaviors including patrol cycles, ledge detection, and damage triggers.

## Operations
1. Implements waypoint-based horizontal patrol loops with speed modulation.
2. Performs forward/downward raycasting to detect ledge cliffs and reverse direction.
3. Manages player damage overlap triggers and vertical stomp-defeat detection.
4. Coordinates animation state transitions (Idle, Walk, Hurt, Die) with Mecanim.
