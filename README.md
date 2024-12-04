---
description: The essentials of how temperature works
---

# Temperature Basics

## Traits

There are several types of temperature that affect entities in differing ways. Every entity has a separate instance of all eleven of these temperature types. These types are:\


<mark style="color:orange;">**`World`**</mark>\
Defines what the area around the entity feels like. This is unique to each entity and is usually affected by biomes in the vicinity, time of day, nearby blocks, etc.

* Value can be read/changed (though changing it is pointless since it is overwritten every tick)
* Value will be overwritten every tick by a newly calculated value\


<mark style="color:orange;">**`Core`**</mark>\
Defines the entity's internal temperature. This value will rise and fall depending on the ambient temperature.

* Value can be read/changed; though in most cases, body temperature should be read instead\


<mark style="color:orange;">**`Base`**</mark>\
A static offset applied to the entity's core temperature. For example, a value of 10 would cause an entity's body temperature to default to 10 when at rest, rather than 0.\


<mark style="color:orange;">**`Body`**</mark> \
This is simply the entity's body and base temperatures added together. This starts at 0 (neutral) and can range from -150 to 150. Values as extreme as ±100 or more are deadly. \
Note that this is not an actual type of temperature that is stored on an entity, but rather shorthand for <mark style="color:orange;">`Core`</mark>`+`<mark style="color:orange;">`Base`</mark>.

* Value can be read, but not changed.
* Value is not stored (there is no need)
* TempModifiers cannot be applied to this value. They should be applied to Core or Base instead\


<mark style="color:orange;">**`Rate`**</mark> \
This value controls the speed at which the entity's total temperature reaches dangerous levels. For example, leather/insulated armor makes this process slower.\
It is retrieved from

* Value cannot be read or changed
* Value is not stored\


<mark style="color:orange;">**`Freezing Point`**</mark>  \
Stores the temperature, in Minecraft units, at which the entity will begin freezing to death.&#x20;

* Value can be read and changed (though changing it will not do anything, as it resets each tick)\


<mark style="color:orange;">**`Burning Point`**</mark> \
Stores the temperature, in Minecraft units, at which the entity will begin overheating to death.&#x20;

* Value can be read and changed (though changing it will not do anything, as it resets each tick)\


<mark style="color:orange;">**`Cold Resistance`**</mark> \
Determines the entity's resistance to freezing damage. Ranges from 0 to 1, with 0 being no resistance and 1 being full resistance. Negative values do nothing.

* Value can be read and changed (though changing it will not do anything, as it resets each tick)\


<mark style="color:orange;">**`Heat Resistance`**</mark> \
Determines the entity's resistance to overheating damage. Ranges from 0 to 1, with 0 being no resistance and 1 being full resistance. Negative values do nothing.

* Value can be read and changed (though changing it will not do anything, as it resets each tick)\


<mark style="color:orange;">**`Cold Dampening`**</mark> \
Changes the rate at which the entity freezes (decreases in body temperature). Values 0 to 1 slow the speed, with 1 stopping freezing altogether. Negative values speed up freezing.

* Value can be read and changed (though changing it will not do anything, as it resets each tick)



All of these are different **temperature traits** that are stored on and applied to the player.&#x20;



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
