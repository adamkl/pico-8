## Purpose

Defines how the player entity's velocity changes each frame in response to input, friction, and gravity, and how that velocity is applied to its position. Backfilled from the implementation in `wip/wip.p8`.

## Requirements

### Requirement: Horizontal Acceleration
The system SHALL accelerate the player's horizontal velocity while the left or right direction button is held, up to a maximum horizontal speed, and SHALL update the player's facing direction to match.

#### Scenario: Holding right accelerates rightward
- **WHEN** the right button is held
- **THEN** horizontal velocity increases by the run acceleration each frame, clamped to the maximum horizontal speed, and facing direction is set to right

#### Scenario: Holding left accelerates leftward
- **WHEN** the left button is held
- **THEN** horizontal velocity decreases by the run acceleration each frame, clamped to the maximum horizontal speed in the negative direction, and facing direction is set to left

### Requirement: Horizontal Friction
The system SHALL reduce the player's horizontal speed toward zero by a fixed friction amount every frame, regardless of ground contact or input.

#### Scenario: Decelerating without input
- **WHEN** no horizontal direction button is held and horizontal velocity is non-zero
- **THEN** horizontal velocity's magnitude decreases by the friction amount each frame, without changing sign, and does not overshoot past zero

#### Scenario: Friction applies while airborne
- **WHEN** the player is airborne
- **THEN** horizontal friction is still applied each frame, the same as while grounded

### Requirement: Gravity
The system SHALL accelerate the player's vertical velocity downward by a fixed gravity amount every frame, except that gravity is reduced while the player slides down a wall.

#### Scenario: Falling in open air
- **WHEN** the player is not touching a wall, or is touching a wall but moving upward
- **THEN** vertical velocity increases by the full gravity amount each frame

#### Scenario: Sliding down a wall
- **WHEN** the player is touching a wall and moving downward
- **THEN** vertical velocity increases by only 20% of the normal gravity amount each frame, producing a slower wall-slide fall

### Requirement: Position Update
The system SHALL move the player by its velocity each frame, after collision resolution has clamped that velocity.

#### Scenario: Position advances by resolved velocity
- **WHEN** the player's velocity has been resolved against the map for the current frame
- **THEN** the player's x and y position are incremented by the resolved horizontal and vertical velocity respectively
