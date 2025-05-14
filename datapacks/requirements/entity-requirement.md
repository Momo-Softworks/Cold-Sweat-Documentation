# Entity Requirement

An entity requirement is a set of criteria that an entity must meet in order to be considered a match. They currently support checking:

* Entity IDs and tags
* Entity location:
  * x/y/z
  * x/y/z offset
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

## Format

<pre class="language-json"><code class="lang-json">{
  // The entities or entity tags
  "entities": [
    "minecraft:polar_bear",
    "#forge:animals"
  ],
  // Checks the entity's location
  "location": {
    // Position relative to the entity
    // All location checks will use this position
    "x_offset": 5,
    "y_offset": 0,
    "z_offset": 5,
    // A range of valid absolute coordinates
    "y": {
      // Entity's y level must be between 0 and 64
      "min": 0,
      "max": 64
    },
    // All of these support tags
    "biome": "minecraft:desert",
    "structure": "#minecraft:villages",
    "dimension": "minecraft:the_nether",
    // Light level at the position
    "light": {
      "min": 4,
      "max": 8
    },
    // A block requirement that checks the position
    "block": {
      "blocks": [
        "minecraft:grass_block"
      ]
    },
    // A fluid requirement that checks the position
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
  // Checks the entity's NBT (<a data-footnote-ref href="#user-content-fn-1">NBT requirement</a>)
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
  // Checks the entity's equipment (these are all <a data-footnote-ref href="#user-content-fn-2">item requirements</a>)
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
    "heat_resistance": {
      "min": 0.5,
      "max": 1.0
    },
    "world": {
      "min": 0.2,
      "max": 0.8
    }
  }
}
</code></pre>

## Type-Specific Data

The `type_data` field is used to check entity-specific data, such as player info or variant name. The format of this field varies depending on the type of entity.

### Entities with Variants

Checks the variant of the given entity, such as the color of a cat, or the personality of a panda.

```json
{
  "type_data": {
    "type": "variant",
    // The variant name
    "variant": "black"
  }
}
```

### Fishing Bobber

Checks if the fishing bobber is in an open body of water. This is probably used by the fishing loot tables to nerf loot from AFK farms.

```json
{
  "type_data": {
    "type": "fishing_hook",
    "in_open_water": true
  }
}
```

### Lightning Bolt

Checks some specific conditions when lighning strikes a location.

```json
{
  "type_data": {
    "type": "lightning_bolt",
    // The number of blocks set on fire
    "blocks_set_on_fire": {
      "min": 1,
      "max": 5
    },
    // Any of the entities struck can pass this requirement (entity requirement)
    "entity_struck": {
      "entities": [
        "minecraft:creeper"
      ]
    }
  }
}
```

### Piglin-Neutral Armor

Checks if the player is wearing any armor that pacifies piglins (i.e. gold boots).

```json
{
  "type_data": {
    "type": "piglin_neutral_armor",
    "piglin_neutral_armor": true
  }
}
```

### Players

For players, the `type_data` field should be an object with several fields:

* `game_mode`: The player's game mode (survival, creative, adventure, spectator).
* `stats`: A list of checks to perform on the player's stats.&#x20;
* `recipes`: A map of recipes that the player has unlocked. The keys of the map are the recipe IDs, and the values of the map are boolean values indicating whether they are unlocked.
* `advancements`: A map of advancements that the player must have completed overall, or have met certain criteria.
* `looking_at`: An entity requirement that is checked against the entity the player is looking at.

```json
{
  "type_data": {
    "type": "player",
    "game_mode": "adventure",
    "stats": [
      // Mined 100 to 500 stone
      {
        "type": "minecraft:mined",
        "stat": "minecraft:stone",
        "value": {
          "min": 100,
          "max": 500
        }
      }
    ],
    // Unlocked the furnace recipe
    "recipes": {
      "minecraft:furnace": true
    },
    "advancements": {
      // Completed the "sniper duel" advancement
      "minecraft:adventure/sniper_duel": {
        "complete": true
      },
      // Has visited a beach
      "minecraft:adventure/adventuring_time": {
        "minecraft:beach": true
      }
    },
    // Looking at a zombie
    "looking_at": {
      "entities": [
        "minecraft:zombie"
      ]
    }
  }
}
```

### Raider

For village raiders, such as pillagers, evokers, and witches:

* `has_raid`: Whether the raider is part of an active raid.
* `is_captain`: Whether the raider is the raid captain.
* `raid`: Data about the raider's active raid:
  * `is_over`: Whether the raid is over.
  * `is_between_waves`:Whether the raid is between waves.
  * `is_first_wave_spawned`: Whether the first wave has spawned.
  * `is_victory`: If the raid is declared as a victory.
  * `is_loss`: If the raid is declared as a loss.
  * `is_started`: The raid has begun.
  * `is_stopped`: The raid has stopped.
  * `total_health`: A decimal-number range which requires he health of all active raid mobs combined to fall within `min` and `max`.
  * `bad_omen_level`: An integer range between `min` and `max` which checks the level of bad omen used to start the raid.
  * `current_wave`: An integer range which requires the current wave number to fall between `min` and `max`.

```json
{
  "type_data": {
    "type": "raider",
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
    "type": "slime",
    "size": {
      "min": 1,
      "max": 3
    }
  }
}
```

### Snow Boots



[^1]: [https://mikul.gitbook.io/cold-sweat/datapacks/requirements/nbt-requirement](https://mikul.gitbook.io/cold-sweat/datapacks/requirements/nbt-requirement)

[^2]: [https://mikul.gitbook.io/cold-sweat/datapacks/requirements/item-requirement](https://mikul.gitbook.io/cold-sweat/datapacks/requirements/item-requirement)
