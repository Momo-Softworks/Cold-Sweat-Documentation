# Item Tags

### <mark style="color:blue;">`not_insulatable`</mark>

Equippable items (armor) that should not accept insulation. This may be useful in instances where an item is equipable in the armor slots, but would not logically be able to be insulated. Some examples of this might be the elytra or Create's goggles.

```json
// By default, this tag is empty
```

### <mark style="color:blue;">`boiler_valid`</mark> and <mark style="color:blue;">`icebox_valid`</mark>

Used to determine which items are allowed to be placed into the boiler/icebox's waterskin slots. Previously, this used to be hardcoded, but this is now a tag to allow mods to, for example, use the icebox as a cooler to prevent food items from spoiling.

```json
"cold_sweat:waterskin",
"cold_sweat:filled_waterskin"
```

### <mark style="color:blue;">`boiler_craftable_deepslate`</mark>

Determines which items can be used as the "deepslate" component for the boiler's crafting recipe. Processed variants of deepslate, like deepslate bricks, are not part of this tag.

```json
"minecraft:deepslate",
"minecraft:polished_deepslate",
"minecraft:cobbled_deepslate"
```

### <mark style="color:blue;">`encases_smokestack`</mark>

Items that can be used to encase a smokestack by right-clicking it. The item will be consumed when used.

```json
"#forge:cobblestone/normal"
```

### <mark style="color:blue;">`grows_soul_stalk`</mark>

Items that can be used to "bonemeal" soul stalk. The item will be consumed when used.

```json
"#forge:dusts/glowstone"
```

## Chameleon Item Tags

There are several tags that dictate what items can be used for certain chameleon behaviors. Items in these tags will be deemed "edible" by chameleons, and they will eat them if they can when thrown by a player.

### <mark style="color:blue;">`chameleon/taming`</mark>

Items that can be used to tame a chameleon or regenerate its health.

```json
"minecraft:spider_eye",
"minecraft:fermented_spider_eye",
"#minecraft:fishes"
```

### <mark style="color:blue;">`chameleon/find_cold_biomes`</mark>

Items that tell the chameleon to find the nearest biome that's temperature is ≤ 0.2 (if it trusts the player that threw the item).

```json
"minecraft:snowball",
"minecraft:beetroot",
"minecraft:sweet_berries"
```

### <mark style="color:blue;">`chameleon/find_hot_biomes`</mark>

Items that tell the chameleon to find the nearest biome with a temperature of ≥ 1.5. Includes biomes like deserts and badlands.

```json
"minecraft:dead_bush",
"minecraft:nether_wart",
"minecraft:cactus"
```

### <mark style="color:blue;">`chameleon/find_humid_biomes`</mark>

Items that tell the chameleon to find the nearest biome with a downfall of ≥ 0.85. Includes biomes like swamps and jungles.

```json
"minecraft:slime_ball",
"minecraft:fern",
"minecraft:large_fern",
"minecraft:cocoa_beans"
```
