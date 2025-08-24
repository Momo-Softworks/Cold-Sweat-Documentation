---
description: The format of all data-driven JSON configs for items
---

# Item Configs

## Insulation Items

`/item/insulator/`

Insulation items are the most advanced config settings in Cold Sweat. This system allows for insulation items to add attribute modifiers to the entity, and can have a condition that the wearing entity must match for the insulation to be active.

### Format

First, insulation items have a few unique data structures of their own:

#### Slot Type

There are 3 insulation slot types that determine how the insulation item is used:

* <mark style="color:blue;">`item`</mark> : A normal insulation item that is applied to armor
* <mark style="color:blue;">`armor`</mark> : An armor item that provides insulation directly when worn
* <mark style="color:blue;">`curio`</mark> : Exclusive to items that can be worn as curios via the Curios mod

The slot type is simply represented by a string, i.e. `"slot": "armor"`



#### Insulation Type

There are also the 2 insulation types:

* <mark style="color:blue;">`static`</mark> : Provides a static amount of heat and cold insulation when worn
* <mark style="color:blue;">`adaptive`</mark> : Adapts to the player's environment to provide a varying amount of hot and cold insulation, like chameleon molt.

The formats for each type look like so:

For static insulation items:

```json
"insulation": {
    "hot": 6,
    "cold": 4
}
```

For adaptive insulation items:

```json
"insulation": {
    "value": 6,
    "adapt_speed": 0.01
}
```

`adapt_speed` sets the amount that the item adapts every tick. Adaptation ranges between -1 (fully cold) and 1 (fully hot).



#### Attribute Modifiers

Attribute modifiers are added to insulation items like so:

```json
"attributes": {
  // The attribute to apply modifier(s) to
  "cold_sweat:heat_resistance":
  [
    {
      // The name of the modifier. Can be anything in 1.20 and below
      // In 1.21+, this has to be in ID format (i.e. mymod:super_awesome_modifier)
      "name": "super awesome modifier",
      "amount": 0.25,
      "operation": "addition"
    },
    {
      "name": "even better one",
      "amount": 0.5,
      "operation": "addition"
    }
  ]
  "generic.max_health":
  [
    {
      "name": "increase health",
      "amount": 2,
      "operation": "multiply_base"
    }
  ]
}
```

**Operation Types:**

* **1.20-** \
  `add`, `multiply_base`, `multiply_total`
* **1.21+**\
  `add_value`, `add_multiplied_base`, `add_multiplied_total`

<h4 align="center">For more info on how attribute modifiers work, see <a href="https://minecraft.wiki/w/Attribute#Modifiers">the Minecraft wiki</a>.</h4>



#### Final Example

Given all of this information, we can take a look at an example insulation item:

<pre class="language-json"><code class="lang-json">{
  // A list of mods that must be loaded for this insulation item to be registered.
  // Useful for adding inter-mod compat that would break if one mod isn't loaded
  "required_mods": [
    "thirst",
    "sereneseasons"
  ],
  // This item gives insulation when worn directly
  "type": "armor",
  // The insulation this item provides
  "insulation": {
    "heat": 6,
    "cold": 4
  },
  // An item requirement (see <a data-footnote-ref href="#user-content-fn-1">Datapack Basics</a>)
  "item": {
    // A list of items that will have this insulation. 
    // Also accepts tags (prefixed with #)
    "items": [
      "minecraft:iron_chestplate",
      "#forge:armors/helmets"
    ],
    "enchantments": [
      {
        "enchantment": "minecraft:protection",
        "levels": {
          "min": 1,
          "max": 2
        }
      }
    ]
  },
  // The predicate that the wearing entity must pass for the insulation to work
  // An entity requirement (see <a data-footnote-ref href="#user-content-fn-2">Datapack Basics</a>)
  "entity": {
    "stepping_on": {
      "block": {
        "blocks": [
          "minecraft:iron_block"
        ]
      }
    },
    "equipment": {
      "mainhand": {
        "items": [
          "minecraft:stick"
        ]
      }
    }
  },
  // Attribute modifiers that are applied to the entity when the item is worn
  // For a list of Cold Sweat attributes, see <a data-footnote-ref href="#user-content-fn-3">Attributes</a>
  "attributes": {
    "generic.movement_speed": [
      {
        "name": "super speed",
        "amount": 0.3,
        "operation": "addition"
      }
    ]
  },
  // Provide protection against certain TempModifiers, reducing their effectiveness
  // A value of 1.0 is full protection
  // See <a data-footnote-ref href="#user-content-fn-4">List of TempModifiers</a>
  "immune_temp_modifiers": {
    "cold_sweat:blocks": 0.5,
    "sereneseasons:season": 1.0
  }
}
</code></pre>

The above example does the following:

* Checks if both Thirst Was Taken and Create are loaded
* Gives 6 heat insulation and 4 cold insulation when worn in armor slots
* Applies to the iron chestplate, or any helmet, that is enchanted with protection 1 or 2
* Only works if the entity is standing on an iron block and holding a stick
* Increases the player's walking speed by 0.3 blocks per tick when worn
* Reduces block temperature effects by 50% and season temperature effects by 100%



## Fuel Items

`/item/fuel/`

Fuel items apply to the icebox, boiler, hearth, and soulspring lamp. They do not currently have extra functionality over what is achievable through normal configs.

### Format

The format for fuel items is as follows:

<pre class="language-json"><code class="lang-json">{
  // This fuel item is for the hearth
  "type": "hearth",
  // Negative values indicate cold fuel (hearth only)
  "fuel": -100,
  // Items to be assigned this fuel value
  // An item requirement (see <a data-footnote-ref href="#user-content-fn-1">Datapack Basics</a>)
  "item": {
    "items": [
      "minecraft:slimeball"
    ]
  }
}
</code></pre>



## Food Items

`/item/food/`

Food items change the entity's body temperature when eaten. They support item requirements and entity requirements (for the entity eating the item).

### Format

<pre class="language-json"><code class="lang-json">{
  "required_mods": [
    "aether"
  ],
  // An item requirement (see <a data-footnote-ref href="#user-content-fn-1">Datapack Basics</a>)
  "item": {
    "items": [
      "minecraft:carrot",
      "#minecraft:piglin_food"
    ]
  },
  // Positive values increase temperature, negative values decrease
  "value": 20,
  // The entity must be in the overworld for this food to change its temperature
  // An entity requirement (see <a data-footnote-ref href="#user-content-fn-1">Datapack Basics</a>)
  "entity": {
    "location": {
      "dimension": "minecraft:overworld"
    }
  }
}
</code></pre>



## Inventory Items

`/item/item_temp/`

Items can be configured to affect the player's temperature when being carried in their inventory. This system can check any inventory slots, including armor slots and [curios](https://www.curseforge.com/minecraft/mc-mods/curios).

### Format

<pre class="language-json"><code class="lang-json">{
  "required_mods": [
    "twilightforest"
  ],
  // An <a data-footnote-ref href="#user-content-fn-5">item requirement</a> that the item must meet to emit temperature
  "item": {
    "items": [
      "minecraft:lava_bucket"
    ]
  },
  // slot name, or range of slot IDs
  // Slot names: hand, feet, legs, chest, head
  "slots": [
    "hand",
    // hotbar slots
    {
      "min": 36,
      "max": 44
    }
  ],
  // The temperature of the item
  "temperature": 0.5,
  // The temperature trait that this item will apply to (see <a data-footnote-ref href="#user-content-fn-3">Attributes</a>)
  "trait": "world",
  // Limits the temperature change this item can cause, even when stacking
  "max_effect": 4,
  // An <a data-footnote-ref href="#user-content-fn-6">entity requirement</a> that the entity must meet to be affected
  "entity": {}
}
</code></pre>



## Drying Items

`/item/drying_item/`

"Drying items" are items that can be used to dry the player off when right-clicked.

### Format

```json
{
  "required_mods": [
    // nothing!
  ],
  // An item requirement that the item must meet to be usable
  "item": {
    "items": [
      "minecraft:sponge"
    ]
  },
  "result": {
    "id": "minecraft:wet_sponge",
    "tag": {
      "Display": {
        "Name": "{\"text\":\"I Am a Wet Sponge\",\"color\":\"blue\"}"
      }
    }
  },
  // An entity requirement that the entity must meet in order to use the item
  "entity": {
    "entities": [
      "minecraft:player"
    ]
  },
  // Sound that is played upon using the item
  "sound": "minecraft:block.wet_grass.step"
}
```

[^1]: [https://mikul.gitbook.io/cold-sweat/datapacks/datapack-basics#item-requirements](https://mikul.gitbook.io/cold-sweat/datapacks/datapack-basics#item-requirements)

[^2]: [https://mikul.gitbook.io/cold-sweat/datapacks/datapack-basics#entity-requirements](https://mikul.gitbook.io/cold-sweat/datapacks/datapack-basics#entity-requirements)

[^3]: [https://mikul.gitbook.io/cold-sweat/attributes#list-of-attributes](https://mikul.gitbook.io/cold-sweat/attributes#list-of-attributes)

[^4]: [https://mikul.gitbook.io/cold-sweat/list-of-tempmodifiers](https://mikul.gitbook.io/cold-sweat/list-of-tempmodifiers)

[^5]: [https://mikul.gitbook.io/cold-sweat/datapacks/requirements/item-requirement](https://mikul.gitbook.io/cold-sweat/datapacks/requirements/entity-requirement)

[^6]: [https://mikul.gitbook.io/cold-sweat/datapacks/requirements/entity-requirement](https://mikul.gitbook.io/cold-sweat/datapacks/requirements/entity-requirement)
