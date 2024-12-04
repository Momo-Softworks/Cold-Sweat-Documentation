---
description: The format of all data-driven JSON configs for entities
---

# Entity Configs

## Spawn Biomes

`/entity/spawn_biome/`

Adds entities to the pool of entities that can spawn in given biomes. This setting is meant to mirror the "Goat Spawn Biomes" and "Chameleon Spawn Biomes" settings, but this can technically be used to add any entity to a biome.

### Format

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
