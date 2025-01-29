---
description: A list of Cold Sweat's custom attributes, with descriptions
---

# Attributes

Cold Sweat adds several [attributes](https://minecraft.wiki/w/Attribute), which can be manipulated via commands and datapacks to modify many aspects of an entity's temperature.

These attributes map directly to the entity's [temperature traits](https://mikul.gitbook.io/cold-sweat#traits), modifying or overriding their values.



## List of Attributes

#### <mark style="color:blue;">`world_temperature`</mark>

Controls the entity's world temperature.&#x20;

This attribute defaults to `NaN`.&#x20;

Setting the attribute's base value to anything else will lock the entity's world temperature to a static value.

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`WORLD`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`base_temperature`</mark>

Controls the entity's base body temperature, (AKA an offset to the entity's body temperature).&#x20;

This attribute defaults to `NaN`.&#x20;

Setting the attribute's base value to anything else will apply a static offset to the entity's body temperature, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`BASE`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`burning_point`</mark>

Controls the temperature at which the entity begins to overheat, in MC units.

This attribute defaults to `NaN`.

Setting the attribute value to anything else will set the entity's maximum habitable temperature to a static value, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`BURNING_POINT`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`freezing_point`</mark>

Controls the temperature at which the entity begins to freeze, in MC units.

This attribute defaults to `NaN`.

Setting the attribute value to anything else will set the entity's minimum habitable temperature to a static value, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`FREEZING_POINT`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`heat_resistance`</mark>

Controls the entity's resistance to incoming overheating damage. Represents the percent of incoming damage to be blocked.

This attribute defaults to `NaN`, and ranges between 0 and 1.

Setting the attribute value to anything else will set the entity's resistance to overheating damage to a static value, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`HEAT_RESISTANCE`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`cold_resistance`</mark>

Controls the entity's resistance to incoming freezing damage, including that from non-temperature-related sources like powder snow. Represents the percent of incoming damage to be blocked.

This attribute defaults to `NaN`, and ranges between 0 and 1.

Setting the attribute value to anything else will set the entity's resistance to freezing damage to a static value, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`COLD_RESISTANCE`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`heat_dampening`</mark>

Controls the entity's rate of overheating. Higher values decrease overheating speed, and negative values increase the speed.

This attribute defaults to `NaN`, and caps out at a value of 1, but can be infinitely negative. A value of 1 prevents the player's temperature from increasing above 0.

Setting the attribute value to anything else will set the entity's overheating rate to a static value, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`HEAT_DAMPENING`</mark> trait, even if the attribute base value is unset.\


#### <mark style="color:blue;">`cold_dampening`</mark>

Controls the entity's rate of freezing. Higher values decrease freezing speed, and negative values increase the speed.

This attribute defaults to `NaN`, and caps out at a value of 1, but can be infinitely negative. A value of 1 prevents the player's temperature from decreasing below 0.

Setting the attribute value to anything else will set the entity's freezing rate to a static value, overriding any applied TempModifiers.&#x20;

Attribute modifiers can modify the entity's corresponding <mark style="color:orange;">`COLD_DAMPENING`</mark> trait, even if the attribute base value is unset.
