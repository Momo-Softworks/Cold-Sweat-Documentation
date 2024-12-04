---
description: A list of all default TempModifiers and their functions
---

# List of TempModifiers

<mark style="color:blue;">`cold_sweat:blocks`</mark>\
Detects nearby blocks and determines how they affect the player's temperature. See [Block Temperature](block-temperature.md) for more info.

<mark style="color:blue;">`cold_sweat:biomes`</mark>\
Detects the temperature of nearby biomes, taking into account altitude, time of day, and humidity, and applies it to the player.

<mark style="color:blue;">`cold_sweat:underground`</mark>\
Handles temperature changes according to the player's depth underground. Also detects underground biomes and applies their temperature to the player if applicable.

<mark style="color:blue;">`cold_sweat:armor`</mark> \
Handles armor insulation.

<mark style="color:blue;">`cold_sweat:mount`</mark> \
Handles insulation granted by the player's currently mounted entity.&#x20;

<mark style="color:blue;">`cold_sweat:waterskin`</mark> \
Handles the change in temperature applied by the Waterskin item.

<mark style="color:blue;">`cold_sweat:soulspring_lamp`</mark> \
Handles the cooling effect applied by the Soulspring Lamp item.

<mark style="color:blue;">`cold_sweat:water`</mark> \
Handles the cooling effect of water and rain, as well as the dripping particles when the player is wet.

<mark style="color:blue;">`cold_sweat:air_conditioning`</mark> \
Handles the warming/cooling provided by the Warmth and Chill effects, given by the hearth, boiler, and icebox.

<mark style="color:blue;">`cold_sweat:food`</mark> \
Handles temperature changes applied by eating configured food items.

<mark style="color:blue;">`cold_sweat:freezing`</mark> \
Handles the chilling effect of the entity standing in powder snow.

<mark style="color:blue;">`cold_sweat:on_fire`</mark> \
Handles the heating effect of the entity being on fire.

<mark style="color:blue;">`cold_sweat:soul_sprout`</mark> (extension of `cold_sweat:food`)\
Handles the effect of eating a Soul Sprout item. Also spawns soul particles for the duration of the Soul Sprout's effect.

<mark style="color:blue;">`cold_sweat:inventory_items`</mark> \
Handles temperature changes applied by items in an entity's inventory.

<mark style="color:blue;">`cold_sweat:entities`</mark> \
Handles the effect of nearby temperature-emitting entities.

{% hint style="info" %}
The following TempModifiers are registered automatically if their respective mods are loaded. They are supplied by the Cold Sweat mod for compatibility.
{% endhint %}

<mark style="color:blue;">`sereneseasons:season`</mark>\
Handles the temperature of the current season if [Serene Seasons](https://www.curseforge.com/minecraft/mc-mods/serene-seasons) is installed.

<mark style="color:blue;">`armorunder:lining`</mark>\
Handles temperature effects from special liner items in [Armor Underwear](https://www.curseforge.com/minecraft/mc-mods/armor-underwear-mod).

<mark style="color:blue;">`weather2:storm`</mark>\
Handles temperature changes caused by weather and storm systems from [Weather, Storms, & Tornadoes](https://www.curseforge.com/minecraft/mc-mods/weather-storms-tornadoes).

<mark style="color:blue;">`curios:curios`</mark>\
Handles the temperature effects of any equipped [Curios](https://www.curseforge.com/minecraft/mc-mods/curios).

<mark style="color:blue;">`valkyrienskies:ship_blocks`</mark> (extension of `cold_sweat:blocks`)\
Handles the temperature of nearby blocks that are part of a [Valkyrien Skies](https://www.curseforge.com/minecraft/mc-mods/valkyrien-skies) ship.
