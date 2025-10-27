# Registry Modifiers

`/modifier/`

Cold Sweat has a special type of config that allows for the modification of certain other configs. For example, a registry modifier can be used to change the attributes of an insulation item, or to remove a config entirely.

Registry modifiers can target TOML, JSON, and KubeJS registries.

{% hint style="info" %}
N**ote:** Registry modifiers do not actually modify the target configuration's source. They only modify the data in memory as it is loaded.
{% endhint %}

### Format

```json
{
  // The type of config to target
  // The registry name of each config is given on its respective docs page
  "registry": "cold_sweat:entity/entity_temp",
  // Config formats to check ("toml", "json", "kubejs")
  // Can also be a single element: "config_type": "json"
  "config_type": [
    "json"
  ],
  // List of IDs to check. These IDs are registered by MC when datapacks are parsed
  // Only JSON registries have IDs, so this does not work for TOML or KubeJS configs
  "entries": [
    "cold_sweat:on_fire"
  ],
  // Any configs matching the given data structure(s) are modified
  // These are NBT requirements, so Cold Sweat's special NBT functions also work here
  "matches": [
    {
      "flags": {
        "is_baby": false
      },
      "nbt": {
        "Burning": 1
      }
    },
    {
      // ..more checks
    }
  ],
  // A list of operations to perform on the target's data
  // Explained in the section below
  "operations": [
  ]
}
```

## Operations

Each registry modifier can perform one or many operations on the target config. There are five operation types, each affecting the target data in a different way:

#### `disable`

Disables the target entirely, causing it to not load.

```json
{
  ...
  "operations": [
    {
      "type": "disable"
    }
  ]
}
```

#### `replace`

Replaces the top-level key of the target, if it exists. The contents of the new data completely replace the old data, which is removed.

```json
{
  "registry": "cold_sweat:item/insulator",
  ...
  "matches": [
    {
      "item": {
        "items": {
          "require": {
            "cs:contains_any": [
              "cold_sweat:chameleon_molt"
            ]
          }
        }
      }
    }
  ],
  "operations": [
    {
      "type": "replace",
      "data": {
        "insulation": {
          "cold": 1,
          "heat": 1
        }
      }
    }
  ]
}

// Example (initial data):
{
  "item": {
    "items": {
      "require": [
        "cold_sweat:chameleon_molt"
      ]
    },
    "nbt": {
      "SomeTag": true
    }
  },
  "insulation": {
    "adapt_speed": 0.0085,
    "value": 2
  }
}
// Result (after operation):
{
  "item": {
    "items": {
      "require": [
        "cold_sweat:chameleon_molt"
      ]
    },
    "nbt": {
      "SomeTag": true
    }
  },
  "insulation": { // "insulation" field is replaced
    "cold": 1,
    "heat": 1
  }
}
```

#### `merge`

Merges the given data with that of the target. If two keys are identical, the new data overwrites the existing data.&#x20;

If a list of elements is given, the new list will be combined with the old list.

```json
{
  "registry": "cold_sweat:world/biome_temp",
  ...
  "matches": [
    {
      "biomes": {
        "require": {
          "cs:contains_any": [
            "minecraft:forest"
          ]
        }
      }
    }
  ],
  "operations": [
    {
      "type": "merge",
      "data": {
        "biomes": [
          "minecraft:dark_forest"
        ],
        "min_temp": 55,
        "max_temp": "*=2", // Multiply value by 2
        "water_temp": 15
      }
    }
  ]
}

// Example (initial data):
{
  "biomes": {
    "require": [
      "minecraft:forest"
    ]
  },
  "min_temp": 60,
  "max_temp": 80,
  "units": "f"
}
// Result (after operation)
{
  "biomes": {
    "require": [
      "minecraft:forest",
      "minecraft:dark_forest" // Added to list
    ]
  },
  "min_temp": 55, // Set to 55
  "max_temp": 160, // Multiplied by 2
  "units": "f",
  "water_temp": 15 // Added "water_temp" field
}
```

The merge operation also supports performing basic arithmetic on the original value to produce a new value. The following operators are implemented:

* += Addition
* -= Subtraction
* \*= Multiplication
* /= Division
* ^= Exponent
* %= Modulus

#### `append`

Adds the given data to the target, without overwriting or modifying existing data at all. This operation is "deep", meaning nested data is also appended. This operation does not affect lists.

```json
{
  "registry": "cold_sweat:entity/entity_temp",
  ...
  "matches": [
    {
      "entity": {
        "flags": {
          "is_on_fire": true
        }
      }
    }
  ],
  "operations": [
    {
      "type": "append",
      "data": {
        "entity": {
          "flags": {
            "is_on_fire": false
          },
          "location": {
            "biome": "#minecraft:is_overworld"
          }
        },
        "max_effect": 30
      }
    }
  ]
  
  // Example (initial data):
  {
    "entity": {
      "flags": {
        "is_on_fire": true
      }
    },
    "temperature": 15,
    "units": "f",
    "range": 6,
    "affects_self": true
  }
  // Result (after operation):
  {
    "entity": {
      "flags": {
        "is_on_fire": true // Field already exists in target, so it is not changed
      },
      "location": { // Added nested data in the "entity" field
        "biome": "#minecraft:is_overworld"
      }
    },
    "temperature": 15,
    "units": "f",
    "range": 6,
    "affects_self": true,
    "max_temp": 30 // Added "max_temp" field
  }
}
```

#### `remove`

Removes data from the target. Also supports removing elements from a list.

```json
{
  "registry": "cold_sweat:block/block_temp",
  ...
  "matches": [
    {
      "block": {
        "blocks": {
          "require": {
            "cs:contains_any": [
              "minecraft:campfire",
              "minecraft:soul_campfire"
            ]
          }
        }
      }
    }
  ],
  "operations": [
    {
      "type": "remove",
      "data": {
        "block": {
          "blocks": {
            "require": [
              "minecraft:soul_campfire" // Remove this element from the list
            ]
          }
        },
        "state": "", // Empty quotes "" removes the field from the target
      }
    }
  ]
}

// Example (initial data):
{
  "block": {
    "blocks": {
      "require": [
        "minecraft:campfire",
        "minecraft:soul_campfire"
      ]
    }
  },
  "state": {
    "lit": true
  }
}
// Result (after operation):
{
  "block": {
    "blocks": {
      "require": [
        "minecraft:campfire"
        // "soul_campfire" removed from list
      ]
    }
  }
  // "state" field removed
}
```
