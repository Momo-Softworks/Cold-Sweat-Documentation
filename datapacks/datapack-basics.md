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



### NBT

#### Multiple Accepted Values

Wherever NBT is used to ensure an entity, item, etc. has the correct NBT tag, Cold Sweat's NBT system supports defining multiple accepted values for a given NBT tag. This is done by using the custom `cs:any_of` argument.

Example:

```json
"nbt": {
  "HeatLevel": {
    "cs:any_of": [
      "Medium",
      "High"
    ]
  }
}
```

This translates to:

```json
"nbt": {
  "HeatLevel": "Medium"
  or
  "HeatLevel": "High"
}
```

If HeatLevel matches either of the given values, the check will pass. This is a very useful alternative to defining multiple possible NBT structures for the same thing.

#### Numerical Ranges

It is also possible to define a range of accepted values if the tag being checked is a number.&#x20;

Example:

```json
"nbt": {
  "Damage": "50-100"
} 
```

Checks if an item's damage is somewhere between 50 and 100. Note that ranges are represented by a string value of two numbers separated by a hyphen ( - ). Decimal values are also accepted.

#### List Contents

When checking the contents of a list, the Vanilla NBT system would require that the contents of the list match the test exactly. This isn't always desired, so Cold Sweat introduces two new arguments:&#x20;

* **`cs:contains_any`**

Checks if the list contains any of the provided values.&#x20;

Example:

```json
// Check: The list must contain either "c" or "s"
"nbt": {
  "StoredValues": {
    "cs:contains_any": [
      "c",
      "s"
    ]    
  }
}

// This NBT will pass the check:
{
  "StoredValues": [
    "a",
    "b",
    "c" // Here!
  ]
}

// This list will fail the check:
{
  "StoredValues": [
    "d",
    "e",
    "f"
    // None of these are "c" or "s"
  ]
}
```

* **`cs:contains_all`**

Checks if the list contains all of the given values (but the list can have additional values not specified)

Example:

```json
// Check: This chest must contain all of these items
"nbt": {
  "Items": {
    "cs:contains_all": [
      // 1 to 54 torches in slot 0 (first slot)
      {
        "Slot": 0
        "id": "minecraft:torch",
        "Count": "1-54"
      },
      // Chameleon molt in slot 5 (sixth slot)
      {
        "Slot": 5,
        "id": "cold_sweat:chameleon_molt"
      }
    ]
  }
}

// This NBT will pass the check:
{
  "Items": [
    // Here are the torches!
    {
      "Slot": 0b,
      "id": "minecraft:torch",
      "Count": 12b // This is within our allowed range
    },
    // We don't care about the rotten flesh in slot 1
    {
      "Slot": 1b,
      "id": "minecraft:rotten_flesh",
      "Count": 4b
    }
    // Here's the chameleon molt!
    {
      "Slot": 5b,
      "id": "cold_sweat:chameleon_molt",
      "Count": 64b // The count doesn't matter
    }
  ]
}
```
