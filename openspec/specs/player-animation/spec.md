## Purpose

Defines how the player's sprite is chosen and oriented each frame based on movement and contact state. Backfilled from the implementation in `wip/wip.p8`.

## Requirements

### Requirement: Grounded Animation
The system SHALL alternate between two walk-cycle sprites while the player is on the ground and moving horizontally, and show a stand sprite while on the ground and stationary.

#### Scenario: Walking on the ground
- **WHEN** the player is on the ground and horizontal velocity is non-zero
- **THEN** the sprite alternates between the two walk-cycle sprites each frame

#### Scenario: Standing on the ground
- **WHEN** the player is on the ground and horizontal velocity is zero
- **THEN** the stand sprite is shown

### Requirement: Airborne Animation
The system SHALL show a wall-slide sprite when the player is off the ground, touching a wall, and falling, and SHALL show a jump sprite for all other airborne states.

#### Scenario: Sliding down a wall
- **WHEN** the player is not on the ground, is touching a wall, and vertical velocity is positive
- **THEN** the wall-slide sprite is shown

#### Scenario: Jumping or falling in open air
- **WHEN** the player is not on the ground and is not sliding down a wall
- **THEN** the jump sprite is shown

### Requirement: Facing Direction
The system SHALL draw the player's sprite flipped to match the player's current facing direction.

#### Scenario: Facing changes with input
- **WHEN** the player's facing direction changes due to horizontal input
- **THEN** the sprite is drawn flipped to match the new facing direction on the next draw
