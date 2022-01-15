# Defold tilemap collision bug example

This is a minimal example reproducing this bug: https://github.com/defold/defold/issues/6327

### Description

- Object `map.go` contains a simple tilemap and a script.
    - `map.script`: updates the tilemap tiles in the `init()` function
        - trees tile with collision is added at `(4, 6)`
        - rock tile with collision is removed from `(2, 7)`
        - rock tile with collision is added at `(2, 3)`
        - trees tile with collision is added at `(3, 3)`
- Main collection contains:
    - One instance of `map.go` placed directly on the scene
    - One factory for `map.go` prefabs
    - `controller.script`: creates a new instance of `map.go` in the `init()` function


### The bug

- The instance of `map.go` that was directly placed on the collection (on the left) has the expected collision areas.
- The instance of `map.go` that gets created by the factory (on the right) has incorrect collision on tiles that were changed in the `init()` function.
    - I have confirmed that this is not just a display bug in the physics debug viewer.
- The rock that was removed from `(2, 7)` still left behind a static collision area.
- The trees that were added at `(4, 6)` did not get a collision area.
- BUT the tile changes at `(2, 3)` and `(3, 3)`, where the tilemap was originally empty on those layers, did in fact update the collision correctly on the factory-generated map.

