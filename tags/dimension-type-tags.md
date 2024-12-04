# Dimension Type Tags

### <mark style="color:blue;">`dimension_type/soulspring_lamp_valid`</mark>

Dimensions in this tag will allow the use of the soulspring lamp, like in the Nether.

```json
// By default, this tag is empty
{
  "replace": false,
  "values": [
  ]
}
```

***

The following tags are added by Cold Sweat to the common tag list. They are empty by default, but are populated with all applicable dimension types when the game is launched. These tags can also be added to manually; though it is usually not necessary.

{% hint style="info" %}
**Note:** In NeoForge for 1.21+, the `forge:` tag category has been replaced with the universal `c:` tag category. This applies to the following tags as well (i.e. `c:has_ceiling`)
{% endhint %}

### <mark style="color:blue;">`forge:has_ceiling`</mark>

Denotes dimensions which have a bedrock roof, like the Nether.

### <mark style="color:blue;">`forge:has_sky`</mark>

Denotes dimensions which do not have a bedrock roof.

### <mark style="color:blue;">`forge:natural`</mark>

Denotes dimensions which are considered "natural". In Vanilla, only the Overworld is categorized this way.

### <mark style="color:blue;">`forge:unnatural`</mark>

Denotes dimensions which are considered "unnatural" (inverse of `natural`).

### <mark style="color:blue;">`forge:ultrawarm`</mark>

Denotes dimensions that are extremely hot, like the Nether.

### <mark style="color:blue;">`forge:has_skylight`</mark>

Denotes dimensions  which have ambient light.&#x20;

### <mark style="color:blue;">`forge:overworld_like`</mark>

Denotes dimensions that are similar to the overworld. This is a combination of `natural`, `has_sky`, `has_skylight`, and **not** `ultrawarm`.

### <mark style="color:blue;">`forge:has_raids`</mark>

Denotes dimensions in which pillagers raids can occur.

### <mark style="color:blue;">`forge:bed_works`</mark>

Denotes dimensions in which a bed is usable without exploding.

### <mark style="color:blue;">`forge:respawn_anchor_works`</mark>

Denotes dimensions in which a [respawn anchor](https://minecraft.wiki/w/Respawn_Anchor) is usable without exploding.

### <mark style="color:blue;">`forge:piglin_safe`</mark>

Denotes dimensions in which piglins will not "zombify".
