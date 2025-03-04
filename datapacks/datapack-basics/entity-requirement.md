# Entity Requirement

An entity requirement is a set of criteria that an entity must meet in order to be considered a match. They currently support checking:

* Entity IDs and tags
* Entity location:
    * x/y/z
    * biome at entity's position
    * structure at entity's position
    * dimension at entity's position
    * light level at entity's position
    * block at entity's position
    * fluid at entity's position
* Block entity is standing on (same as "Entity location", but 1 block below)
* Potion effects
* NBT
* Entity flags:
    * On fire
    * Sneaking
    * Sprinting
    * Swimming
    * Invisible
    * Glowing
    * Baby
* Equipment (head, chest, legs, feet, main hand, offhand)
    * Each of these is an item requirement
* Entity-specific data:
    * Players:
        * Game mode (survival, creative, adventure, spectator)
        * Stats (blocks mined, distance walked, etc)
        * Unlocked recipes
        * Advancements
        * Entity the player is looking at
    * Entities with variants (cats, horses, etc.)
        * Variant name
    * Fishing bobber:
        * If the bobber is in an open lake
    * Lightning bolt:
        * Entities struck by the lightning
        * Blocks set on fire
    * Entity is wearing piglin-pacifying armor
    * Village raiders:
        * Is the raid captain
        * Part of an active raid
        * Active raid info
    * Slime:
        * Size of the slime
* Vehicle (entity that this entity is riding)
* Passenger (entity riding this entity)
* Target (entity being targeted by this entity, if it is a hostile mob)
* Temperature traits (thermal resistance, insulation, etc.)

These are modeled after Vanilla's entity predicates, and are structured like so:

```json
{
  // The entities or entity tags
  "entities": [
    "minecraft:polar_bear",
    "#forge:animals"
  ],
  // Checks the entity's location
  "location": {
    "x": 100,
    "y": 64,
    "z": -50,
    "biome": "minecraft:desert",
    "structure": "#minecraft:villages",
    "dimension": "minecraft:the_nether",
    "light": {
      "min": 4,
      "max": 8
    },
    "block": {
      "blocks": [
        "minecraft:grass_block"
      ]
    },
    "fluid": {
      "fluids": [
        "minecraft:lava"
      ],
      "state": {
        "falling": true
      }
    }
  },
  // Checks the block the entity is standing on
  "stepping_on": {
    "blocks": [
      "minecraft:cobweb",
      "#forge:ores/diamond"
    ]
  },
  // Checks the entity's active effects
  "effects": {
    // Also supports tags, but there aren't any potion tags in Vanilla
    "effects": [
      {
        "potion": "minecraft:strength",
        "amplifier": {
          "min": 1,
          "max": 2
        },
        "duration": {
          "min": 100,
          "max": 200
        }
      },
      // "amplifier" and "duration" fields are optional
      {
        "potion": "minecraft:invisibility"
      }
    ]
  },
  // Checks the entity's NBT
  "nbt": {
    "SomeTag": true,
    "SomeOtherTag": "1-10",
    // NBT tags can be nested
    "SomeNestedCompoundTag": {
      "ThisTagValue": 42,
      "ThisTagName": "Ramphord Yortold"
    }
  },
  // Checks the entity's flags
  "flags": {
    "is_on_fire": true,
    "is_sneaking": false,
    "is_sprinting": true,
    "is_swimming": false,
    "is_invisible": true,
    "is_glowing": false,
    "is_baby": true
  },
  // Checks the entity's equipment
  "equipment": {
    "head": {
      "items": [
        "minecraft:iron_helmet"
      ]
    },
    "chest": {
      "items": [
        "minecraft:leather_chestplate"
      ]
    },
    "legs": {
      "items": [
        "minecraft:diamond_leggings"
      ]
    },
    "feet": {
      "items": [
        "minecraft:chainmail_boots"
      ]
    },
    "mainhand": {
      "items": [
        "minecraft:iron_sword"
      ]
    },
    "offhand": {
      "items": [
        "minecraft:shield"
      ]
    }
  },
  // Checks entity-specific data, such as player info
  "type_data": "See the section below about type-specific data",
  // Checks the entity's team
  "team": "red",
  // Checks the entity's vehicle
  "vehicle": {
    "entities": [
      "minecraft:boat"
    ]
  },
  // Checks the entity's passenger
  "passenger": {
    "entities": [
      "minecraft:player"
    ]
  },
  // Checks the entity's target, if it is a hostile mob
  "target": {
    "entities": [
      "minecraft:villager"
    ]
  },
  // Checks the entity's temperature traits
  "temperature": {
    "thermal_resistance": {
      "min": 0.5,
      "max": 1.0
    },
    "insulation": {
      "min": 0.2,
      "max": 0.8
    }
  }
}
```

## Type-Specific Data

The `type_data` field is used to check entity-specific data, such as player info or variant name. The format of this field varies depending on the type of entity.

### Entities with Variants

For entities with variants, such as cats or horses, the `type_data` field should be an object with a single field named `variant`, which should be a string containing the name of the variant.

```json
{
  "type_data": {
    "variant": "black"
  }
}
```

### Fishing Bobber

For fishing bobbers, the `type_data` field should be an object with a single field named `in_open_water`, which should be a boolean indicating whether the bobber is in an open lake.

```json
{
  "type_data": {
    "in_open_water": true
  }
}
```

### Lightning Bolt

For lightning bolts, the `type_data` field should be an object with two fields: `blocks_set_on_fire` and `entity_struck`. The `blocks_set_on_fire` field should be an object with `min` and `max` fields, which should be integers indicating the minimum and maximum number of blocks set on fire by the lightning bolt. The `entity_struck` field should be an entity requirement that is checked against the entities struck by the lightning bolt.

```json
{
  "type_data": {
    "blocks_set_on_fire": {
      "min": 1,
      "max": 5
    },
    "entity_struck": {
      "entities": [
        "minecraft:creeper"
      ]
    }
  }
}
```

### Piglin-Neutral Armor

For entities wearing piglin-pacifying armor, the `type_data` field should be an object with a single field named `piglin_neutral_armor`, which should be set to `true`.

```json
{
  "type_data": {
    "piglin_neutral_armor": true
  }
}
```

### Players

For players, the `type_data` field should be an object with several fields:

* `game_mode`: The player's game mode (survival, creative, adventure, spectator).
* `stats`: An object containing the player's stats. The keys of this object should be stat IDs, and the values should be objects with `min` and `max` fields, which should be integers indicating the minimum and maximum value of the stat.
* `recipes`: An object containing the player's unlocked recipes. The keys of this object should be recipe IDs, and the values should be booleans indicating whether the recipe is unlocked.
* `advancements`: An object containing the player's advancements. The keys of this object should be advancement IDs, and the values should be objects with a single field named `complete`, which should be a boolean indicating whether the advancement is completed.
* `looking_at`: An entity requirement that is checked against the entity the player is looking at.

```json
{
  "type_data": {
    "game_mode": "adventure",
    "stats": {
      "minecraft:mined": {
        "min": 100,
        "max": 1000
      }
    },
    "recipes": {
      "minecraft:crafting_table": true
    },
    "advancements": {
      "minecraft:story/root": {
        "complete": true
      }
    },
    "looking_at": {
      "entities": [
        "minecraft:zombie"
      ]
    }
  }
}
```

### Raider

For village raiders, the `type_data` field should be an object with several fields:

* `has_raid`: A boolean indicating whether the raider is part of an active raid.
* `is_captain`: A boolean indicating whether the raider is the raid captain.
* `raid`: An object containing information about the active raid. The fields of this object are:
    * `is_over`: A boolean indicating whether the raid is over.
    * `is_between_waves`: A boolean indicating whether the raid is between waves.
    * `is_first_wave_spawned`: A boolean indicating whether the first wave has spawned.
    * `is_victory`: A boolean indicating whether the raid is a victory.
    * `is_loss`: A boolean indicating whether the raid is a loss.
    * `is_started`: A boolean indicating whether the raid has started.
    * `is_stopped`: A boolean indicating whether the raid has stopped.
    * `total_health`: An object with `min` and `max` fields, which should be doubles indicating the minimum and maximum total health of the raid.
    * `bad_omen_level`: An object with `min` and `max` fields, which should be doubles indicating the minimum and maximum bad omen level of the raid.
    * `current_wave`: An object with `min` and `max` fields, which should be integers indicating the minimum and maximum current wave of the raid.

```json
{
  "type_data": {
    "has_raid": true,
    "is_captain": false,
    "raid": {
      "is_over": false,
      "is_between_waves": true,
      "is_first_wave_spawned": true,
      "is_victory": false,
      "is_loss": false,
      "is_started": true,
      "is_stopped": false,
      "total_health": {
        "min": 100.0,
        "max": 1000.0
      },
      "bad_omen_level": {
        "min": 1.0,
        "max": 5.0
      },
      "current_wave": {
        "min": 1,
        "max": 3
      }
    }
  }
}
```

### Slime

For slimes, the `type_data` field should be an object with a single field named `size`, which should be an object with `min` and `max` fields, which should be integers indicating the minimum and maximum size of the slime.

```json
{
  "type_data": {
    "size": {
      "min": 1,
      "max": 3
    }
  }
}
```
