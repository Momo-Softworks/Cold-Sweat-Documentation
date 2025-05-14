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
