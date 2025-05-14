# Location Requirement

A location requirement checks a given position in the world. It supports the following:

* Position
* Offset position to be checked by x/y/z
* Biome at position
* Structure at position
* Dimension
* Light level at position
* Block at position
* Fluid at position
* Temperature at position

## Format

```json
{
  // (optional) Position relative to the location
  // All checks will use this offset position
  "x_offset": 5,
  "y_offset": 0,
  "z_offset": 5,
  // (optional) A range of valid coordinates. X and Z can be checked in the same way
  // Can also use a single int ("x": 128)
  "y": {
    // Location's y level must be between 0 and 64
    "min": 0,
    "max": 64
  },
  // (optional) All of these support tags. Biome/structure/dimension must be at this location
  "biome": "minecraft:desert",
  "structure": "#minecraft:villages",
  "dimension": "minecraft:the_nether",
  // (optional) Light level at the position. 
  // Can also use a single int ("light": 4)
  "light": {
    "min": 4,
    "max": 8
  },
  // (optional) A block requirement that checks the position
  "block": {
    "blocks": [
      "minecraft:grass_block"
    ]
  },
  // (optional) A fluid requi rement that checks the position
  "fluid": {
    // (optional) List of valid fluid types. Accepts tags
    "fluids": [
      "minecraft:lava"
    ],
    // (optoinal) "Block state" of the fluid
    "state": {
      "falling": true
    }
    // (optional) Whether this fluid is a source
    "is_source": true
  },
  // (optional) Temperature at the position, in MC units
  // "min" or "max" is optional, and defaults to - or + infinity
  "temperature": {
    "min": 1.0,
    // Also supports preset names: freezing, cold, cool, temperate, warm, hot, burning
    // These values range between the configured freezing and burning points
    "max": "warm"
  }
}
```
