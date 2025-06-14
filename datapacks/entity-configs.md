---
description: The format of all data-driven JSON configs for entities
---

# Entity Configs

## Spawn Biomes

`/entity/spawn_biome/`

Adds entities to the pool of entities that can spawn in given biomes. This setting is meant to mirror the "Goat Spawn Biomes" and "Chameleon Spawn Biomes" settings, but this can technically be used to add any entity to a biome.

#### Format

```json
{
  "required_mods": [
    "alexsmobs"
  ],
  // A list of biomes, or biome tags
  "biomes": [
    "#minecraft:is_mountain"
  ],
  // A list of entities, or entity type tags
  "entities": [
    "#minecraft:skeletons",
    "alexsmobs:bear"
  ],
  // The spawn weight of the entities in the given biomes.
  // Higher numbers have a greater chance of spawning
  "weight": 5,
  // The mob category these entities belong to.
  // This determines things like that maximum number of mobs allowed, what blocks they can spawn on, etc.
  "category": "creature"
}
```

## Insulating Mounts

`/entity/mount/`

Enables entities to provide insulating properties to the player riding them. Can also grant (partial) immunity to certain temperature modifiers.

#### Format

<pre class="language-json"><code class="lang-json">{
  // An <a data-footnote-ref href="#user-content-fn-1">entity requirement</a> for the mount entity
  "entity": {
    "entities": [
      "minecraft:horse"
    ]
  },
  // An <a data-footnote-ref href="#user-content-fn-1">entity requirement</a> for the player riding the entity
  "rider": {
    "team": "blue"
  },
  // Amount of cold insulation (0 to 1) 
  "cold_insulation": 0.5,
  // Amount of heat insulation (0 to 1)
  "heat_insulation": 0.75,
  // Map of temperautre modifier immunities
  "immune_temp_modifiers": {
    // 100% immune to water
    "cold_sweat:water": 1.0
  }
}
</code></pre>

## Entity Temperature

`/entity/entity_temperature/`

Allows entities to emit temperature, optionally under certain conditions. This temperature can affect nearby entities or players, as well as (optionally) the target entity itself.&#x20;

Below is a slightly modified version of the temperature emitted when an entity is on fire:

#### Format

<pre class="language-json"><code class="lang-json">{
  "required_mods": [
    // none
  ],
  // <a data-footnote-ref href="#user-content-fn-1">Entity requirement</a>. Entity must be on fire
  "entity": {
    "flags": {
      "is_on_fire": true
    }
  },
  // Temperature to emit. This is added to the world temperature of nearby entities
  "temperature": 15.0,
  // Temperature units to use ("f", "c", "mc")
  "units": "f",
  // Range of effect (weakens as distance from emitting entity reaches this value)
  "range": 6.0,
  // Should temperature also increase for emitting entity?
  "affects_self": true
}
</code></pre>

[^1]: [https://mikul.gitbook.io/cold-sweat/datapacks/requirements/entity-requirement](https://mikul.gitbook.io/cold-sweat/datapacks/requirements/entity-requirement)
