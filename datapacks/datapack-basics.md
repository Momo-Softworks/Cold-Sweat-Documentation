---
description: >-
  A guide to setting up your workspace for Cold Sweat's data system and some
  important data structures
---

# Datapack Basics

Cold Sweat has several data systems in place that allow developers or modpack creators more granular control over the mod's config settings. These systems provide advanced functionality that is not accessible through the traditional TOML files.&#x20;



## Workspace Setup

Data files for Cold Sweat are stored in <mark style="color:green;">`data/<yourmod>/cold_sweat/*`</mark> where "yourmod" is the ID of your mod. If you are making a traditional datapack, this can be anything.&#x20;

So far, there are 4 categories of data: `item`, `block`, `world`, and `entity`. Each of these are a dedicated folder within the datapack directory. These directories support using sub-directories for organization.

Data-driven JSON configs can also be put in the mod's config folder in the game directory: <mark style="color:green;">`config/coldsweat/data/*`</mark>. These allow users to use the more advanced JSON system without having to make a datapack.

{% hint style="info" %}
**Notes for 1.16:**

Mods for this version must define Cold Sweat data in <mark style="color:yellow;">`/data/cold_sweat/configs/*`</mark>.&#x20;

World datapacks (in the `datapacks` folder in world files) are not currently supported, but users can define custom settings in the <mark style="color:yellow;">`config/coldsweat/data`</mark> folder as in other versions.
{% endhint %}



## Notes & Important Constructs

This section contains some miscellaneous information about various aspects of Cold Sweat's config system. Interacting with most of these features is optional.

### Required Mods

All JSON configs in Cold Sweat have a "required mods" field that allows for configs to be conditionally loaded depending on what mods are present in the current instance. If any of the required mods aren't met, the config is fully prevented from being parsed, at the first step of the loading process.

This can be useful if the config contains IDs for items, blocks, etc. that belong to a particular mod that might not be present.&#x20;

{% hint style="warning" %}
If the JSON contains mod-specific data and the mod(s) are not added to `required_mods`, Minecraft will attempt to parse the file anyway, and the missing data will **likely cause Minecraft's data loading process to fail entirely**.&#x20;
{% endhint %}

The format for required mods is very simple:

```json
{
  "required_mods": [
    "toughasnails",
    "create",
    "etc"
  ]
}
```

Required mods can also use a negatable list, allowing conditions to be inverted, meaning the config will only load if a mod is **not** present:

```json
{
  "required_mods": {
    "require": [
      "create",
      "thirst"
    ],
    // If Tough as Nails is present, this config will not be loaded
    "exclude": [
      "toughasnails"
    ]
  }
}
```

### Negatable List

See the dedicated page for negatable lists [here](negatable-list.md).

### Default Configs

The file names of some of Cold Sweat's default JSON configs start with the word "default". This makes them have the lowest possible priority in the config loading process, so other configs can override them. For example, the default [temp region](block-world-configs.md#temperature-regions) file uses this so that user-defined temp regions will always have priority.

**Prefixing your own configs with "default" will have the same effect, so it's typically not advisable to do this unless that's your intention**. Note that many configs support stacking multiple entries, such as those for insulation. For them, this mechanic does not apply.
