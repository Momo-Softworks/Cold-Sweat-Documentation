---
description: The essentials of how temperature works
---

# Temperature Basics

## Traits

There are several types of temperature that affect entities in differing ways. Every entity has a separate instance of all of these temperature traits.

* `world`: The ambient temperature that the entity experiences\
  **Range: -∞ → ∞**
* `core`: The entity's core temperature. This is the main component of the entity's body temperature. \
  **Does not support attribute modifiers.**\
  **Range: -150 → 150**
* `base`: An offset applied on top of the entity's core temperature, i.e. when eating a soul sprout.\
  **Range: -150 → 150**
* `body`: Represents the entity's core and base traits added together. \
  **This trait can be read, but not assigned.**\
  **Does not support temperature modifiers or attribute modifiers.**\
  **Range: -150 → 150**
* `rate`: The entity's current rate of overheating (positive) or freezing (negative).\
  **Range: -∞ → ∞**
* `freezing_point`: The temperature at which the entity will start to freeze.\
  **Range: -∞ → ∞**
* `burning_point:` The temperature at which the entity will start to overheat.\
  **Range: -∞ → ∞**
* `cold_resistance`: Resistance to damage taken from freezing.\
  **Range: 0.0 → 1.0**
* heat\_resistance: Resistance to damage taken from overheating.\
  **Range: 0.0 → 1.0**
* `cold_dampening`: Controls the rate at which the player freezes. Positive values decrease the rate; negative values increase it.\
  **Range: -∞ → 1.0**
* `heat_dampening`: Controls the rate at which the player overheats. Positive values decrease the rate; negative values increase it.\
  **Range: -∞ → 1.0**

***

## Functions

The <mark style="color:yellow;">`Temperature`</mark> utility class has several functions to manipulate and read an entity's temperature. The following are some of the basic functions that pertain to entity temperature:



#### get()

```java
get(LivingEntity entity, Trait trait)
```

Returns the chosen temperature trait on this entity.

<mark style="color:blue;">`trait`</mark> specifies which temperature should be fetched from the entity (world, body, etc).



**set()**

```java
set(LivingEntity entity, Trait trait, double value)
```

Sets the entity's temperature to the given value. <mark style="color:blue;">`trait`</mark> is used to denote what aspect of the entity's temperature is being affected.

This method also has an overload method with an extra boolean parameter that controls whether or not the new temperature will by synced to the client automatically via a packet (if the entity is a player). In the default implementation, the new temperature is always synced.



**add()**

```java
add(LivingEntity entity, Trait trait, double value)
```

Shorthand for adding to the entity's temperature. Negative values are also accepted.

***

## Using Real-World Values <a href="#using-real-world-values" id="using-real-world-values"></a>

Cold Sweat uses Minecraft's biome temperature scale when dealing with world Temperatures. This is good because it standardizes all operations to use Minecraft's built-in temperature system.&#x20;

However, this new temperature scale isn't easy to work with, as typical biome temperature values range from **0 to 2**, with 0 being a snowy tundra and a 2 being the nether (there are some rare examples outside these values; see the [Minecraft Wiki page on biomes](https://minecraft.fandom.com/wiki/Biome#list-of-overworld-climates) for more information).

Cold Sweat has a function for converting real units of temperature (Celsius, Fahrenheit) to Minecraft's units. The `CSMath` utility class has a method that does just that:

#### convertUnits() <a href="#convertunits" id="convertunits"></a>

```java
Temperature.convert(double value, Units from, Units to, boolean absolute);
```

This returns a double value representing the temperature specified, converted to the desired unit of measurement.

* <mark style="color:blue;">`from`</mark> - The unit of measurement you are converting _from_
* <mark style="color:blue;">`to`</mark> - The unit of measurement you are converting _to_
* <mark style="color:blue;">`absolute`</mark> - Whether the specified temperature is absolute or relative. For example, the temperature of a room would be in absolute units, but adding 10 degrees to the room's temperature is a relative measurement, so it would not be absolute.&#x20;

<mark style="color:orange;">`Units`</mark> is an enum inside the Temperature class that lists the three units of measurement:

* <mark style="color:orange;">`F`</mark> - Fahrenheit
* <mark style="color:orange;">`C`</mark> - Celsius
* <mark style="color:orange;">`MC`</mark> - Minecraft units

{% hint style="warning" %}
**Note:** Body temperature uses its own set of "units" separate from normal temperature values. It ranges from -150 to 150, with -100 being freezing, 100 being burning, and 0 being neutral.
{% endhint %}
