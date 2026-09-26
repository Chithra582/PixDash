# Rules: PixDash Agent

These are immutable operational boundaries and safety constraints for PixDash Agent.

## MUST ALWAYS
1. **MUST ALWAYS execute physics inside `FixedUpdate()`**: Never apply forces, velocities, or Rigidbody2D translations inside `Update()`; use `FixedUpdate()` with `Time.fixedDeltaTime` to guarantee framerate independence.
2. **MUST ALWAYS enforce `_Project/` asset hierarchy**: All custom scripts, prefabs, audio, and scenes must reside under `Assets/_Project/` to prevent merge conflicts.
3. **MUST ALWAYS freeze Rigidbody2D Z-rotation**: Ensure 2D platformer character bodies freeze rotation around the Z-axis (`Constraints -> Freeze Rotation Z`) to prevent players from toppling over upon collision.
4. **MUST ALWAYS use LayerMasks for raycast ground checks**: Never perform untargeted raycasts; always restrict ground checks to designated `Ground` or `Platform` collision layers.
5. **MUST ALWAYS serialize variables instead of hardcoding**: Expose movement speed, jump force, and coyote timers via `[SerializeField]` so level designers can tune values in the Unity Inspector.

## MUST NEVER
1. **MUST NEVER write monolithic God-classes**: Keep PlayerController, PlayerInput, PlayerAudio, and PlayerAnimation separated into distinct, modular scripts.
2. **MUST NEVER use `FindObjectOfType` or `GameObject.Find` in `Update()`**: Avoid expensive string-based scene searches inside hot render/physics loops; cache references in `Awake()` or inject them via the Inspector.
3. **MUST NEVER apply direct `transform.position` edits on dynamic rigidbodies**: Modifying transform coordinates directly bypasses the 2D physics engine, causing tunneling through colliders.
4. **MUST NEVER commit modified third-party packages to `_Project/`**: Maintain strict boundaries between external Unity packages and custom project code.
