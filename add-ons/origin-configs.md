---
description: The format of all data-driven JSON configs for Origins modifiers
---

# Origin Configs

{% hint style="warning" %}
**This page is for the** [**Cold Sweat: Origins Addendum**](https://www.curseforge.com/minecraft/mc-mods/cold-sweat-origins-addon) **addon mod**
{% endhint %}

Origins can be configured to give the player custom attribute values via the [Cold Sweat: Origins Addon](https://legacy.curseforge.com/minecraft/mc-mods/cold-sweat-origins-addon) mod. This mod has its own datapack system that works very similarly to Cold Sweat's, but operates independently of Cold Sweat's monolithic datapack loader.

Origins can be customized by creating Origins Modifier files, which store all additional attributes and attribute modifiers that are to be added to the player upon selecting the specified origin.

These files can be wrapped into datapacks:

`data/<yourmod>/cold_sweat/origin_modifier/*`

or put in the config folder:

`config/coldsweat/data/origin_modifier/*`



### Format

Origins Modifiers have a few custom data structures that should be known first:

#### Attributes

The `"attributes"` section defines the base values of the player's attributes, overriding their default base values:

```json
"attributes": [
  {
    "id": "cold_sweat:heat_dampening",
    "value": 1
  },
  {
    "id": "cold_sweat:heat_resistance",
    "value": 1
  },
  {
    "id": "cold_sweat:cold_dampening",
    "value": -1
  }
]
```



#### Attribute Modifiers

The "modifiers" section defines a list of attribute modifiers, which are applied on top of the attributes' base values. These attribute modifiers will be given the name "Origins Attribute" and assigned a random UUID upon creation:

```json
"modifiers": [
  {
    "id": "cold_sweat:freezing_point",
    "operation": "addition",
    "value": -0.2
  },
  {
    "id": "cold_sweat:burning_point",
    "operation": "addition",
    "value": 4
  }
]
```

\
Given this information, we can break down the format for Origin Modifiers as a whole:

```json
{
  // These settings are applied to the Blazeborn origin
  "origin": "origins:blazeborn",
  "attributes": [
    {
      // Gives 100% heat dampening
      "id": "cold_sweat:heat_dampening",
      "value": 1
    },
    {
      "id": "cold_sweat:heat_resistance",
      "value": 1
    },
    {
      // Doubles freezing speed
      "id": "cold_sweat:cold_dampening",
      "value": -1
    }
  ],
  "modifiers": [
    {
      // ID of the attribute this modifier is being applied to
      // Increases the max habitable temperature by 4 MC (180 F/100 C)
      "id": "cold_sweat:burning_point",
      "operation": "addition",
      "value": 4
    }
  ],
  // These TempModifiers cannot be applied to this origin
  "immune_temp_modifiers": [
    // Doesn't heat up when on fire
    "cold_sweat:on_fire",
    // Doesn't cool down from the soulspring lamp
    "cold_sweat:soulspring_lamp",
    // Doesn't get soaked by water
    "cold_sweat:water"
  ]
}
```
