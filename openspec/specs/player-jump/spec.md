## Purpose

Defines how the player jumps from the ground and performs wall jumps. Backfilled from the implementation in `wip/wip.p8`.

## Requirements

### Requirement: Ground Jump
The system SHALL launch the player upward when the jump button is pressed while the player is on the ground and not eligible for a wall jump.

#### Scenario: Jump from the ground
- **WHEN** the jump button is pressed and the player is on the ground
- **THEN** vertical velocity is set to the jump acceleration value (clamped to the maximum vertical speed), propelling the player upward

### Requirement: Wall Jump
The system SHALL launch the player up and away from a wall when the jump button is pressed while the player is touching a wall and falling.

#### Scenario: Jump off a wall while sliding down it
- **WHEN** the jump button is pressed, the player is touching a wall, and vertical velocity is positive (falling)
- **THEN** vertical velocity is set to the jump acceleration scaled by the wall-jump multiplier (clamped to the maximum vertical speed), and horizontal velocity is set to the maximum horizontal speed in the direction opposite the player's current facing direction

### Requirement: Wall Jump Precedence
The system SHALL check the wall-jump condition before the ground-jump condition, so a wall jump is performed instead of a ground jump when both could apply.

#### Scenario: Both conditions hold at once
- **WHEN** the jump button is pressed and the player satisfies both the wall-jump condition (on wall, falling) and the ground-jump condition (on ground)
- **THEN** a wall jump is performed, not a ground jump

### Requirement: Jump Is Edge-Triggered
The system SHALL trigger a jump only on the frame the jump button transitions to pressed, not while it is held.

#### Scenario: Holding the jump button
- **WHEN** the jump button remains held across multiple frames
- **THEN** only the first frame of the press triggers a jump
