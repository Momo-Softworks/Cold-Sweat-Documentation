---
description: The format of all data-driven JSON configs for blocks and the world
---

# Block/World Configs

## Temperature Units

Most world-related configs support using different units depending on the user's preference.There are 3 supported units of temperature for use with biome and dimension settings:

* <mark style="color:orange;">`F`</mark> : Fahrenheit
* <mark style="color:orange;">`C`</mark> : Celsius
* <mark style="color:orange;">`MC`</mark> : Minecraft Units. These are used in biome generation, and typically range from 0 (freezing) to 2 (the Nether). With default settings, perfectly habitable is about 1.

Internally, these are all converted to MC units, since these are the units that are used by Minecraft and other mods for their biomes



## Block Temperature

`/block/block_temp/`

You might be familiar with BlockTemps from [previous sections](https://mikul.gitbook.io/cold-sweat/block-temperature). They can also be defined in JSON format, which has a couple advantages over traditional configs. The main reason they can be defined this way, though, is because of the modularity of JSON rather than defining them in the monolithic TOML files.

### Format

<pre class="language-json"><code class="lang-json">{
  "blocks": [
    "#minecraft:all_signs"
  ],
  // The temperature of the block, in MC units
  "temperature": 0.5,
  // The maximum temperature change that this BlockTemp can cause
  // Used for capping the effect when multiple of this block are nearby
  "max_effect": 2.0,
  // Flag that causes the effect of the block to fade with range. Defaults to true
  "fade": true,
  // The radius of the block's area-of-effect
  "range": 7,
  // List of <a data-footnote-ref href="#user-content-fn-1">block requirements</a>
  "conditions": [
    // The sign must have glowing text on its front face
    {
      "nbt": {
        "front_text": {
          "has_glowing_text": true
        } 
      }
    },
    // The sign must not have a sturdy face on top
    {
      "has_sturdy_face": "up",
      "negate": true
    }
  ]
}
</code></pre>



## Biome Temperature

`/world/biome_temp/`

The temperatures of biomes can be customized through JSON data, which is useful for 3rd-party developers who wish to support their custom biomes, or for pack developers who want a custom feel.

### Format

Luckily, the format for biome temperatures is simpler than most settings:

```json
{
  "required_mods": [
    "terralith"
  ],
  "biomes": [
    "minecraft:plains",
    "#minecraft:is_ocean"
  ],
  "temperature": 70,
  // Alternatively, temperatures for noon and midnight (high and low) can be defined:
  "min_temp": 50,
  "max_temp": 90,
  // The units being used for temperature. This example is in Fahrenheit
  // Defaults to MC
  "units": "F",
  // Applies an offset to the biome's base temperature, 
  // rather than setting the biome's temperature outright
  "is_offset": false
}
```



## Dimension Temperature

`/world/dimension_temp/`

Dimension temperature configs are defined in almost exactly the same way as biome temperature. The only difference is that dimension temperatures do not support a min/max ("high/low") temperature, and instead use just one value:

### Format

```json
{
  "required_mods": [
    "origins"
  ],
  "dimensions": [
    "minecraft:the_nether",
    "#cold_sweat:soulspring_lamp_valid"
  ],
  "temperature": 50,
  // The units being used for temperature. This example is in Fahrenheit
  // Defaults to MC
  "units": "F",
  // Applies an offset to the dimension's base temperature
  // If false, all biome temperatures are overridden to the specified value
  "is_offset": true
}
```



## Structure Temperature

`/world/structure_temp/`

The temperature within the bounds of a naturally-generated structure can be modified, similar to dimensions and biomes.

### Format

```json
{
  "required_mods": [
    "idas"
  ],
  "structures": [
    "minecraft:igloo",
    "#minecraft:village"
  ],
  "temperature": 30,
  "units": "F",
  "offset": true
}
```



## Temperature Regions

`/world/temp_region/`

Cold Sweat uses a system of "temperature regions" to determine the temperature of the world at any y-level. These regions are highly configurable:

### Format

Each config can be made up of many "regions", which define the world's temperature at specific altitudes. First, we will go into detail about to makeup of a region bound:

<pre class="language-json"><code class="lang-json">// The bound, either "top" or "bottom"
"top": {
  // "depth" will be relative to this point
  // (constant, world_top, world_bottom, ground_level)
  "anchor": "ground_level",
  // 21 blocks below ground level
  "depth": -21,
  // The temperature at the top of this region will be 75 degrees Fahrenheit
<strong>  "temperature": 75,
</strong>  "units": "f"
}
</code></pre>

The `"temperature"` field also has a few optional properties:

```json

"top": {
  "temperature": {
    // Either "midpoint" or "static"
    // "midpoint" is the average of the min/max habitable temperature
    // "static" is normal
    // optional, defaults to "static"
    "type": "midpoint",
    // The temperature of the bound
    // if set to "midpoint", this field is useless and can be omitted
    "value": 10,
    // Ranges from 0 to 1
    // at 1, the temperature is fixed to the given value 
    // at 0, passes through the biome's temperature
    "strength": 0.5 
  }
}
```

Now, let's put this together into a complete region:

```json
{
  // The type of gradient between the bottom and top bounds
  // (constant, linear, exponential, logarithmic)
  "type": "linear",
  // The upper bound of this region
  "top": {
    "anchor": "ground_level",
    "depth": -1,
    "temperature": {
      "strength": 0
    }
  },
  // The lower bound of this region
  "bottom": {
    "anchor": "ground_level",
    "depth": -20,
    "temperature": 75,
    "units": "f"
  }
}
```

Finally, we can look at the full formatting for a temperature region file:

<pre class="language-json"><code class="lang-json"><strong>// This is the default depth region layout, with some tweaks to show off more features
</strong><strong>{
</strong>  "dimensions":
  [
    // Only apply this depth region set in overworld-like dimensions
    "#forge:overworld_like"
  ],
  "regions": [
    // From ground level to the world height is normal
    {
      "type": "constant",
      "top": {
        "anchor": "world_top",
        "depth": 0,
        "temperature": {
          "strength": 0
        }
      },
      "bottom": {
        "anchor": "ground_level",
        "depth": 0,
        "temperature": {
          "strength": 0
        }
      }
    },
    // At 20 blocks below, the temperature evens out a bit
    {
      "type": "linear",
      "top": {
        "anchor": "ground_level",
        "depth": -1,
        "temperature": {
          "strength": 0
        }
      },
      "bottom": {
        "anchor": "ground_level",
        "depth": -20,
        "temperature": {
          "type": "midpoint",
          "strength": 0.5
        }
      }
    },
    // At the bottom of the world, it gets hotter
    {
      "type": "exponential",
      "top": {
        "anchor": "ground_level",
        "depth": -21,
        "temperature": {
          "type": "midpoint",
          "strength": 0.5
        }
      },
      "bottom": {
        "anchor": "world_bottom",
        "depth": 0,
        "temperature": 110,
        "units": "f"
      }
    }
  ]
}
</code></pre>



[^1]: [https://mikul.gitbook.io/cold-sweat/datapacks/datapack-basics#block-requirements](https://mikul.gitbook.io/cold-sweat/datapacks/datapack-basics#block-requirements)
