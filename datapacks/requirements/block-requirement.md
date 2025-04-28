# Block Requirement

A block requirement is a set of criteria that a block must meet. They currently support checking:

* Block IDs and tags
* NBT (if the block is a [block entity](https://minecraft.wiki/w/Block_entity))
* If the block has solid faces on certain sides
* If the block is replaceable

This requirement can also be negated, meaning everything that **doesn't** match the given checks passes.

Example:

```json
{
  // List of blocks or block tags
  "blocks": [
    "minecraft:oak_planks",
    "#minecraft:wool"
  ],
  // List of blockstate checks (boolean, int, int range, enum, or list of enums)
  "state": {
    "lit": true,
    "age": 3,
    "power": {
      "min": 1,
      "max": 3
    },
    "type": "lower",
    "facing": [
      "north",
      "south"
    ]
  },
  // If the block is a tile entity, it must have NBT that matches this reqirement
  // This section does nothing for normal blocks
  "nbt": {
    "Powered": true,
    "EnergyLevel": "3.5-11.5",
    "InnerData": {
      "NestedTag": "some_value"
    }
  },
  // Checks if the given sides of the block are solid
  "sturdy_faces": [
    "up",
    "south"
  ],
  // Checks if the block is replaceable by the player or fluids
  "replaceable": false,
  // Inverts this requirement, so it passes if any of the checks fail
  "negate": false
}
```
