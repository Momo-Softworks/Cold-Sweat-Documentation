---
description: An in-depth explanation of the fundamentals of temperature automation
---

# TempModifiers

A player's temperatures are affected using TempModifiers. Individual instances of these modifiers can be applied to/removed from a player and affect their temperatures in various ways.

## Creation

Each TempModifier is defined in its own class, extending the abstract <mark style="color:yellow;">`TempModifier`</mark> class. For a standard implementation, there is only one method to be overridden:



#### `calculate()`

```java
Function<Double, Double> calculate(LivingEntity entity, Trait trait)
```

This is the method where the calculation of the TempModifier occurs. The output is a function that takes a double representing the entity's current temperature, and outputs the new temperature.

* <mark style="color:blue;">`entity`</mark> - The entity that this TempModifier instance is attached to&#x20;
* <mark style="color:blue;">`trait`</mark> - The trait that this TempModifier is being applied for. This is useful if a TempModifier is applicable to multiple traits.

{% hint style="info" %}
It's preferable to keep as much logic outside the returned function as possible if it doesn't need to be performed every tick.&#x20;

TempModifiers can be configured to run calculations on a slower interval. See [tickRate()](tempmodifiers.md#tickrate) for more information.
{% endhint %}

For example:

```java
@Override
public Function<Double, Double> calculate(LivingEntity entity, Trait trait)
{
    final int entityY = entity.blockPosition().getY();
    return temperature -> temperature + entityY;
}
```

By creating a final variable and passing it into the function, <mark style="color:purple;">`player.blockPosition().getY()`</mark> is only called when the TempModifier is recalculated (which might be every 5 ticks, 10, 20, etc.). Otherwise, the already-calculated value of  <mark style="color:blue;">`entityY`</mark> is used, requiring no more computation. This can be very useful for complex TempModifiers that would be extremely expensive to run every tick.



### Example Class:

```java
public class MyTempModifier extends TempModifier
{
    @Override
    public Function<Double, Double> calculate(LivingEntity entity, Trait trait)
    {
        // Getting the time of day every single tick is not necessary
        int time = entity.level.getDayTime();
        
        // This function takes in a Temperature (temp) and divides
        // it by the time of day.
        // The temperature will be lower as the day passes
        return temp -> temp / time;
    }
}
```



### The Logic of Functions

As has been stated, TempModifiers produce a `Function<Double, Double>` when <mark style="color:purple;">`calculate()`</mark> is called, then store it.

Functions are used instead of static numbers because TempModifiers can be stacked, and the output of a TempModifier relies on the output of the one before it.&#x20;

Functions allow for the interactions between them to be dynamic, better support TempModifiers with different tick rates, and can adapt when TempModifiers are added or removed from the entity.



### In-Line Builders

When creating a new instance of a TempModifier, there a few functions that can modify their behavior on a per-instance basis.&#x20;

#### `expires()`

```java
TempModifier#expires(int ticks)
```

This gives the TempModifier instance a "lifespan", and it will be automatically removed from the entity after the specified number of ticks. If needed, this duration can be changed after the modifier has been created calling <mark style="color:purple;">`TempModifier#expires(int ticks)`</mark> on it again.&#x20;

This expire time can also be read by using <mark style="color:purple;">`TempModifier#getExpireTicks()`</mark>.



#### `tickRate()`

```java
TempModifier#tickRate(int ticks)
```

This changes the functionality of the TempModifier instance to only call <mark style="color:purple;">`calculate()`</mark> at the specified interval (every _X_ ticks). On every other tick that does not land on the interval, the modifier will use the last function it generated.

This can save massively on performance if your TempModifier does not have to be updated every single tick (TempModifiers in vanilla Cold Sweat rarely do).

{% hint style="info" %}
**Note:** `calculate()` is always called when a TempModifier is queried for the first time, even if the current tick does not land on the chosen interval.
{% endhint %}



These methods can be chained together along with a TempModifier instance. For example:

{% code fullWidth="false" %}
```java
MyTempModifier modifier = new MyTempModifier().expires(10).tickRate(5);
```
{% endcode %}

Here, we are creating a new `MyTempModifier` that will expire automatically after 10 ticks and calls <mark style="color:purple;">`calculate()`</mark> to update its function every 5 ticks.&#x20;



## Usage

TempModifiers can be manipulated, added, removed, etc. with the use of helper methods. These methods directly interface with the entity's capability, as well as send update packets when necessary.



#### `addModifier()`

{% code fullWidth="false" %}
```java
Temperature.addModifier(LivingEntity entity, TempModifier modifier, 
                        Temperature.Trait trait, Placement.Duplicates duplicates
```
{% endcode %}

* <mark style="color:blue;">`entity`</mark> - The Player entity that this `TempModifier` instance will be attached to
* <mark style="color:blue;">`modifier`</mark> - An instance of the `TempModifier` that should be attached to the entity
* <mark style="color:blue;">`trait`</mark> - The type of temperature on the entity that the modifier should be affecting
* <mark style="color:blue;">`duplicates`</mark> - Controls how duplicates are handled when adding a TempModifiers.

With the possibility of having multiple instances of the same TempModifier on an entity, <mark style="color:yellow;">`Placement.Duplicates`</mark> is a way to flag how duplicates will be handled. There are 3 possible duplicate policies:

* <mark style="color:orange;">`ALLOW`</mark>: No check for duplicates
* <mark style="color:orange;">`BY_CLASS`</mark>: Passes only if a TempModifier of the same class exists
* <mark style="color:orange;">`EXACT`</mark>: Passes only if A TempModifier of the same class _**and**_ NBT data exists

### Placements

For more granular control over the ordering of `TempModifier`s on an entity (since the order in which `TempModifier`s are applied matters greatly), there is a version of this method that takes in a <mark style="color:yellow;">`Placement`</mark> object that indicates where in the stack the modifier should be placed:

{% code fullWidth="false" %}
```java
Temperature.addModifier(LivingEntity entity, TempModifier modifier, Trait trait, 
                        Placement.Duplicates duplicates, int maxCount, Placement placement)
```
{% endcode %}

A placement compares each `TempModifier` on the target entity with a `Predicate<TempModifier>`, then inserts the `TempModifier` at that location when a match is found. `maxCount` indicates the maximum number of additions or replacements allowed for this operation.&#x20;

Placements have 4 modes:

* <mark style="color:orange;">`BEFORE`</mark>: Indicates that the new TempModifier should be inserted before the first TempModifier instance that matches the predicate.
* <mark style="color:orange;">`AFTER`</mark>: Places the TempModifer after the first match.
* <mark style="color:orange;">`REPLACE`</mark>: Replaces the first matched TempModifier with the new one. If a match isn't found, this operation fails and the new TempModifier is not added.
* <mark style="color:orange;">`REPLACE_OR_ADD`</mark>: Similar to replace, but the TempModifier will be added to the end of the stack if a match isn't found.

Placements also have 2 orders:

* <mark style="color:orange;">`FIRST`</mark>: Iterates through the TempModifier stack in sequential order, starting from index 0.
* <mark style="color:orange;">`LAST`</mark>: Starts from the last index, and iterates through the TempModifier stack in reverse order.

**Example:**

```java
Temperature.addModifier(entity, 
                        new UndergroundTempModifier().tickRate(10), 
                        Temperatyre.Trait.WORLD,
                        Placement.of(Mode.AFTER, Order.FIRST, 
                                     mod -> mod instanceof BiomeTempModifier));
```

Note how <mark style="color:purple;">`Placement.of()`</mark> is used to create a new Placement instance.

This code adds a new UndergroundTempModifier instance directly after the first instance of a BiomeTempModifier in the stack. \
<mark style="color:blue;">`mod -> mod instanceof BiomeTempModifier`</mark> is the predicate that compares every TempModifier on the entity and selects a spot to insert the new TempModifier.

For convenience, `Placement.AFTER_LAST` and `BEFORE_FIRST` are static Placement instances that simply place the TempModifier at the beginning or end of the stack.



#### `removeModifiers()`

```java
Temperature.removeModifiers(LivingEntity entity, Temperature.Trait trait, int maxCount, Predicate<TempModifier> condition, Placement.Order order)
```

Removing a TempModifier from the stack also uses a `Predicate<TempModifier>` to evaluate which TempModifiers should be targeted. A `Placement.Order` is also used to determine the order in which the elements of the TempModifier stack will be compared.

**Example:**

```java
Temperature.removeModifiers(entity, Trait.BASE, 4, Temperature.Placement.Order.FIRST, modifier -> modifier.getTickRate() < 10)
```

This example removes the first 4 TempModifiers that have a tick rate of less than 10 from the entity's BASE temperature trait.

### Other Operations

Aside from the basic adding and removing of modifiers, there are some other helpful functions for TempModifier-related logic:



#### `getModifier()`

```java
TempHelper.getModifier(Player, Temperature.Type type, Class<? extends TempModifier> modClass)
```

Returns the first found instance of&#x20;

#### `getModifiers()`

```java
Temperature.getModifiers(LivingEntity entity, Temperature.Trait trait)
```

Returns a list of TempModifiers of the specified <mark style="color:blue;">`trait`</mark> that are attached to the given entity. The returned list should NOT be directly used to add TempModifiers because it would circumvent the synchronization present in <mark style="color:purple;">`addModifier()`</mark>, which could have unintended side-effects.



#### `hasModifier()`

```java
Temperature.hasModifier(LivingEntity player, Temperature.Trait trait, Class<? extends TempModifier> modClass)
```

This is a simple boolean method that returns true if any modifiers for the given trait that extend <mark style="color:blue;">`modClass`</mark> are present on the given entity.&#x20;



#### `forEachModifier()`

```java
Temperature.forEachModifier(LivingEntity entity, Temperature.Trait trait, Consumer<TempModifier> action)
```

Performs the given <mark style="color:blue;">`action`</mark> on every modifier of the specified <mark style="color:blue;">`type`</mark> on the given <mark style="color:blue;">`player`</mark>.&#x20;

A typical lambda <mark style="color:yellow;">`Consumer`</mark> parameter for this method might look something like this:

```java
modifier -> 
{
    if (modifier instanceof MyTempModifier)
    {
        System.out.println("Hey, that's my TempModifier!")
    }
}
```

In this example, the system will print <mark style="color:green;">`"Hey, that's my TempModifier!"`</mark> for every instance of <mark style="color:yellow;">`MyTempModifier`</mark> that is found on the  entity.



### Data Storage

Each TempModifier instance has its own CompoundTag that can hold extra information for the TempModifier. NBT should be used instead of instance fields if possible, since NBT is saved when the entity is unloaded, and can be synchronized with <mark style="color:purple;">`Temperature.updateModifiers()`</mark>.



{% hint style="warning" %}
**Note:** This data is not synchronized automatically, like for ItemStacks or Entities. <mark style="color:purple;">`updateModifiers()`</mark>must be called if the data is needed on the client.
{% endhint %}



## Registration

TempModifiers need to be registered in order to be properly applied, stored, and read. To do this, we need to hook into the <mark style="color:yellow;">`TempModifierRegisterEvent`</mark> event.&#x20;

This event fires when the server (or internal server in singleplayer) is loaded. It compiles all registered TempModifiers into a <mark style="color:yellow;">`Map<String, TempModifier>`</mark>, which can then be used to look up a <mark style="color:yellow;">`TempModifier`</mark> class based on its internal ID.

This event fires on the <mark style="color:orange;">`FORGE`</mark> event bus, and is not <mark style="color:yellow;">`Cancellable`</mark>.

### Syntax

```java
@SubscribeEvent
public static void onModifiersRegister(TempModifierRegisterEvent event)
{
    // MyMod.MOD_ID is a string representing your mod's ID
    event.register(new ResourceLocation(MyMod.MOD_ID, "my_modifier"), () -> new MyTempModifier());
}
```

In this example, we are registering <mark style="color:yellow;">`MyTempModifier`</mark> by passing a TempModifier supplier into <mark style="color:purple;">`event.register()`</mark>. The supplier should always generate a new instance when called, or you risk having multiple entities share the same TempModifier instance, which will have very unpredictable side-effects.&#x20;

<mark style="color:purple;">`event.register()`</mark> can be called multiple times for different TempModifiers if needed.&#x20;

Registering a TempModifier multiple times will simply overwrite the existing TempModifier with the new one, which has essentially no effect.



## Adding By Default

There may be a TempModifier that you wish to have applied automatically when an entity spawns. The standard way to do this is through <mark style="color:yellow;">`GatherDefaultTempModifiersEvent`</mark>.&#x20;

As the name suggests, this event will gather the "default" TempModifiers supplied by all mods (including Cold Sweat itself) and add them to the given entity immediately upon spawning.&#x20;

This event is fired once for every temperature [Trait](attributes.md), so ensure your TempModifier is being added to the correct trait.

This event fires on the <mark style="color:orange;">`FORGE`</mark> event bus, and is not <mark style="color:yellow;">`Cancellable`</mark>.

### Syntax

```java
@SubscribeEvent
public static void onEntitySpawn(GatherDefaultTempModifiersEvent event)
{
    // Add the TempModifier to every player's WORLD trait
    if (event.getEntity() instanceof Player && event.getTrait() == Temperature.Trait.WORLD)
    {
        // Add TempModifiers like this
        // This will add our TempModifier to the front of the list, given there isn't one already
        event.addModifier(new MyTempModifier(), Placement.Duplicates.BY_CLASS, Placement.BEFORE_FIRST);
        // This method adds a modifier based on its registered ID, rather than a direct reference
        // Useful if your modifier references code that might not be loaded
        if (OtherMod.isLoaded())
        {
            // If the ID isn't found, nothing will happen
            event.addModifierById(new ResourceLocation(MyMod.MOD_ID, "deferred_modifier"));
        }
    }
}
```

{% hint style="warning" %}
**Notes:**

* This event is fired **EVERY TIME** an entity joins the world, even after relogging, changing dimensions, etc. Check to ensure you are not unintentionally adding duplicate TempModifiers.
* This event fires after the entity has been constructed from NBT, meaning any TempModifiers it previously had (if any) will be loaded by this point.
* It is discouraged to use <mark style="color:yellow;">`EventPriority.HIGHEST`</mark>, as this is when Cold Sweat's default TempModifiers are applied. Adding TempModifiers before this point might cause issues.
{% endhint %}

{% hint style="info" %}
For more info on how use the event system in Forge, see the official documentation [**here**](https://mcforge.readthedocs.io/en/latest/events/intro/).

For a more in-depth explanation of the logic behind events, have a look at the unofficial Forge Community Wiki's detailed explanation [**here**](https://forge.gemwire.uk/wiki/Events).
{% endhint %}
