---
description: >-
  A guide to setting up your workspace for Cold Sweat's data system and some
  important data structures
---

# Datapack Basics

Cold Sweat has several data systems in place that allow developers or modpack creators more granular control over the mod's config settings. These systems provide advanced functionality that is not accessible through the traditional TOML files.&#x20;

## Workspace Setup

Data files for Cold Sweat are stored in <mark style="color:green;">`data/<yourmod>/cold_sweat/*`</mark> where "yourmod" is the ID of your mod. If you are making a traditional datapack, this can be anything.&#x20;

So far, there are 4 categories of data: `item`, `block`, `world`, and `entity`. Each of these are a dedicated folder within the datapack directory.

Data-driven JSON configs can also be put in the mod's config folder in the game directory: <mark style="color:green;">`config/coldsweat/data/*`</mark>. These allow users to use the more advanced JSON system without having to make a datapack.



{% hint style="info" %}
**Notes for 1.16:**

Mods for this version must define Cold Sweat data in <mark style="color:yellow;">`/data/cold_sweat/configs/*`</mark>. \


World datapacks (in the `datapacks` folder in world files) are not currently supported, but users can define custom settings in the <mark style="color:yellow;">`config/coldsweat/data`</mark> folder as in other versions.
{% endhint %}

## Important Data Structures

Before getting into specific use cases, it should be noted how this JSON is formed. Most of the data structures used are based on existing Vanilla data structures as closely as possible. Below are the most important ideas that need to be understood to fully utilize this system:&#x20;

### Entity Requirements

An entity requirement is a set of criteria that an entity must meet.

Although Cold Sweat's entity requirements are very complex, they are designed to be structured like Vanilla Minecraft's entity predicates as much as possible, so generator tools like [Misode's](https://misode.github.io/predicate/) will work.

The entire format of entity requirements will not be explained here, but there is one limitation that, while not likely to be important, should be mentioned:

Like entity predicates, entity requirements allow for defining a requirement for the entity that this entity is mounted to, or the entity that this entity is targeting. For example, a zombie is targeting a specific player, or is riding a horse. \
\
This can work recursively, too, (i.e. checking what entity the horse is riding), **but only up to 16 layers** due to the technical limitations of how JSON is read in Minecraft.



### Item Requirements

An item requirement is a set of criteria that an item must meet.&#x20;

These are modeled after Vanilla's item predicates, which look like so:

{% code fullWidth="false" %}
```json
"data": {
  // The items that quality for this config setting
  "items": [
    "minecraft:iron_chestplate",
    // Tags can be used, too
    "#forge:armors/helmets"
  ],
  // Count isn't used in a lot of cases, but it's here for consistency with Vanilla
  "count": {
    "min": 2,
    "max": 8
  }
  // There is also a stored_enchantments field if the item is an enchanted book
  "enchantments": [
    {
      "enchantment": "minecraft:flame",
      "levels": {
        "min": 1,
        "max": 2
      }
    },
    // Enchantments don't require a "levels" block. This checks if it has infinity
    {
      "enchantment": "minecraft:infinity"
    }
  ],
  // The item's remaining durability must be within these bounds
  "durability": {
    "min": 1,
    "max": 100
  },
  // The item must be a lengthened fire resistance potion
  "potion": "minecraft:long_fire_resistance",
  // The item must have these NBT tags
  "nbt": {
    "SomeTag": true,
    "SomeOtherTag": "1-10"
    // NBT tags can be nested
    "SomeNestedCompoundTag": {
      "ThisTagValue": 42,
      "ThisTagName": "Ramphord Yortold"
    }
  }
}
```
{% endcode %}



### Block Requirements

A block requirement is a set of criteria that a block must meet.

These are a custom data structure that incorporate most of the functionality of block predicates, with some additions:

```json
{
  // List of blocks or block tags
  "blocks": [
    "minecraft:oak_planks",
    "#minecraft:wool"
  ],
  // List of blockstate checks (boolean, int, int range, or enum)
  "state": {
    "lit": true,
    "power": {
      "min": 1,
      "max": 3
    },
    "facing": "north",
    "age": 3
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
  // Checks if the given side of the block is solid
  "has_sturdy_face": "up",
  // Checks if the block's coordinates are within the world bounds
  "within_world_bounds": true,
  // Checks if the block is replaceable by the player or fluids
  "replaceable": false,
  // Inverts all checks made by this block requirement
  "negate": false
}
```



### NBT

Wherever NBT is used to ensure an entity, item, etc. has the correct NBT tag, it is possible to define a range of accepted values if the tag being checked is a number.&#x20;

Example:

```json
"nbt": {
  "Damage": "50-100"
} 
```

Checks if an item's damage is somewhere between 50 and 100. Note that ranges are represented by a string value of two numbers separated by a dash ( - ).

