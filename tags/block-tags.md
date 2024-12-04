# Block Tags

### <mark style="color:blue;">`blocks/ignore_sleep_check`</mark>

Blocks that are able to be slept on, and should allow the player to sleep even if the ambient temperature is freezing or sweltering.

```json
// By default, this tag is empty
{
  "replace": false,
  "values": [
  ]
}
```

### <mark style="color:blue;">`blocks/soul_sand_replaceable`</mark>

Used for world generation when determining which blocks can be replaced by the soul sand disk that forms under soul sprouts.

```json
{
  "replace": false,
  "values": [
    "#minecraft:base_stone_nether",
    "#minecraft:nylium"
  ],
  "remove": [
    "minecraft:basalt",
    "#cold_sweat:may_place_on/soul_stalk"
  ]
}
```



## May Place On

Tags in this directory determine which blocks are safe for certain blocks to be placed on.&#x20;

### <mark style="color:blue;">`blocks/may_place_on/soul_stalk`</mark>

```json
{
  "values":
  [
    "#minecraft:soul_fire_base_blocks"
  ]
}
```



## Hearth Tags

Block tags related to hearth functionality.&#x20;

### <mark style="color:blue;">`blocks/hearth/spread_whitelist`</mark>

Allows specified blocks to bypass Cold Sweat's calculations for what is deemed "transparent" for when the hearth's area of effect is expanding. This tag is referenced alongside the config setting.

```json
// By default, this tag is empty
{
  "replace": false,
  "values": [
  ]
}
```

### <mark style="color:blue;">`blocks/hearth/spread_blacklist`</mark>

Allows specified blocks to bypass Cold Sweat's calculations for what is deemed "solid" for when the hearth's area of effect is expanding. This tag is referenced alongside the config setting.

```json
// By default, this tag is empty
{
  "replace": false,
  "values": [
  ]
}
```

