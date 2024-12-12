---
description: A short overview of Cold Sweat's KubeJS integration
---

# KubeJS Basics

## Bindings

Cold Sweat has a custom binding that has some utility functions to make manipulating and accessing temperature easier in JavaScript.

### `coldsweat`

Contains miscellaneous utility methods for Cold Sweat.

#### `getConfigSetting()`

```javascript
coldsweat.getConfigSetting(String id)
```

Returns a [`DynamicHolder`](../utility-classes/dynamicholder.md) containing the value of the specified config setting. The resulting holder has no explicit type, meaning it must be cast.

#### `getRegistryAccess()`

```java
coldsweat.getRegistryAccess()
```

Returns an instance of RegistryAccess, or null if it is not available. See [here](../utility-classes/dynamicholder.md#registry-access) for important information regarding this method.

#### `getTemperature()`

```java
coldsweat.getTemperature(Entity entity, String trait)
```

Returns the value of the specified temperature trait for the given entity. <mark style="color:yellow;">`trait`</mark> is the trait's snake-cased ID.

#### `setTemperature()`

```java
coldsweat.setTemperature(Entity entity, String trait, double temperature)
```

Sets the value of the specified trait for the given entity. It will likely be replaced next tick for any trait other than <mark style="color:orange;">`core`</mark>.

#### `createModifier()`

```java
coldsweat.createModifier(String id)
```

Creates a new temperature modifier instance from the supplier registered to the given ID.&#x20;

#### `addModifier()`

```java
coldsweat.addModifier(Entity entity, TempModifier modifier, String trait)
```

Adds the given modifier to the given entity's specified trait. This method provides a basic way of doing this through JavaScript. For more advanced versions, use the Java-based methods instead.

#### `getTrait()`

```java
coldsweat.getTrait(String traitId)
```

Returns the <mark style="color:blue;">`Trait`</mark> enum constant associated with the given ID. This might be necessary when using Java methods in Cold Sweat.

#### `getColdInsulation()`

```java
coldsweat.getColdInsulation(Entity entity)
```

Returns the sum of all cold insulation from the given entity's armor, curios, etc.

#### `getHeatInsulation()`

```java
coldsweat.getHeatInsulation(Entity entity)
```

Returns the sum of all heat insulation from the given entity's armor, curios, etc.

#### `getBlockTemperature()`

```java
coldsweat.getBlockTemperature(BlockContainerJS block)
```

Returns the raw temperature of the given <mark style="color:blue;">`BlockContainer`</mark> object (which is what KubeJS refers to as a "block")

#### `getBiomeTemperature()`

```java
coldsweat.getBiomeTemperature(Level level, BlockPos pos)
```

Returns the biome temperature at the given location, taking into account time of day.

#### `getBiomeTemperatureAt()`

```java
coldsweat.getBiomeTemperatureAt(Level level, BlockPos pos)
```

Similar to [`getBiomeTemperature()`](kubejs-basics.md#getbiometemperature), but also takes altitude into account.

#### `getTemperatureAt()`

```java
coldsweat.getTemperatureAt(Level level, BlockPos pos)
```

Returns the overall ambient temperature at the given position, including nearby blocks, biomes, time of day, depth underground, etc.&#x20;
