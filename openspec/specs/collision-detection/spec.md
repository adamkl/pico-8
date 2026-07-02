## Purpose

Defines how an entity's bounding box is checked against the tile map to resolve movement collisions and update ground/wall contact state. Backfilled from the implementation in `wip/wip.p8`.

## Requirements

### Requirement: Solid Tile Detection
The system SHALL treat a map tile as solid when the sprite drawn in that tile has collision flag 0 set.

#### Scenario: Checking a bounding box point against the map
- **WHEN** a bounding-box check point's pixel coordinates are converted to a map tile coordinate
- **THEN** the tile at that coordinate counts as solid if its sprite has flag 0 set, and any solid point registers a collision for that bounding box side

### Requirement: Directional Bounding Boxes
The system SHALL check the entity's top, bottom, left, and right bounding-box edges independently each frame, skipping an edge whose direction opposes the entity's current velocity.

#### Scenario: Moving downward skips the top edge
- **WHEN** vertical velocity is positive (moving down)
- **THEN** the top edge is not checked for collision this frame

#### Scenario: Moving upward skips the bottom edge
- **WHEN** vertical velocity is negative (moving up)
- **THEN** the bottom edge is not checked for collision this frame

#### Scenario: Moving right skips the left edge
- **WHEN** horizontal velocity is positive (moving right)
- **THEN** the left edge is not checked for collision this frame

#### Scenario: Moving left skips the right edge
- **WHEN** horizontal velocity is negative (moving left)
- **THEN** the right edge is not checked for collision this frame

### Requirement: Swept Collision Resolution
The system SHALL sweep each checked edge along the entity's velocity vector in one-pixel steps to find the furthest position before a solid tile is reached, and clamp velocity to stop at that position.

#### Scenario: Colliding partway through a frame's movement
- **WHEN** a checked edge's sweep encounters a solid tile before covering the full velocity distance
- **THEN** the corresponding velocity component is reduced so the entity stops just before the solid tile

#### Scenario: No collision along the full sweep
- **WHEN** a checked edge's sweep covers the full velocity distance without encountering a solid tile
- **THEN** the velocity component for that edge is left unchanged

### Requirement: Ground and Wall Contact State
The system SHALL update the entity's on_ground flag from the bottom-edge check and its on_wall flag from the left/right-edge checks.

#### Scenario: Landing on a solid tile
- **WHEN** the bottom edge is checked and finds a solid tile
- **THEN** on_ground is set to true

#### Scenario: Leaving the ground
- **WHEN** the bottom edge is checked and finds no solid tile
- **THEN** on_ground is set to false

#### Scenario: Touching a wall
- **WHEN** the left or right edge is checked and finds a solid tile
- **THEN** on_wall is set to true

#### Scenario: Leaving a wall
- **WHEN** the left or right edge is checked and finds no solid tile
- **THEN** on_wall is set to false
