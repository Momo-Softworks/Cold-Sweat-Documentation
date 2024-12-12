---
description: Caches Forge capability data for faster access
---

# CapabilityCache

Retrieving a capability from an entity, item, etc. is generally an expensive operation, especially if it is performed frequently, as it must be retrieved and deserialized from NBT.&#x20;

Forge documentation suggests caching the retrieved capability so that future queries will reference the already-gotten capability, which is the purpose of this class.

## Usage

### Definition

Setting up a capability cache is very simple:

```java
public static CapabilityCache<MyCapInterface, OwnerObjectType> CAP_CACHE = new CapabilityCache<>(MyCapabilities.CAPABILITY);
```

Or a more specific example for an entity capability:

```java
public static CapabilityCache<ITemperatureCap, Entity> CAP_CACHE = new CapabilityCache<>(ModCapabilities.ENTITY_TEMPERATURE);
```

**Let's break this down:**

The first type parameter, `ITemperatureCap`, is the interface which our capability extends.&#x20;

The second type parameter, `Entity`, is the type of object that this capability will be attached to. In this case, the temperature capability belongs to entities. Anything that allows for attaching capabilities can be used.

The constructor parameter used for defining the cache, `ModCapabilities.ENTITY_TEMPERATURE`, is the `CapabilityToken` that represents this capability. This is what you would typically pass to the `getCapability()` method in Forge.

### Operations

To retrieve a capability, simply call `get()` and supply the capability owner object. Note that this method returns a `LazyOptional`, which might be empty if the object does not have the capability.

<pre class="language-java"><code class="lang-java"><strong>get(T capabilityOwner) // T represents the type of the capability owner
</strong></code></pre>

As was stated earlier, unused entries will be automatically removed from the cache. In the event that the cache must be cleared manually, though, `clear()` can be called:

```java
clear()
```

To perform an action only if an object's capability is already cached, two methods are available. One method gives a consumer containing the capability object itself:

```java
ifPresent(K key, Consumer<C> consumer)
```

The other method supplies the LazyOptional that wraps the capability:

```java
ifLazyPresent(K key, Consumer<LazyOptional<C>> consumer)
```

## Functionality

Internally, a capability cache manages a Map that links objects to their capabilities.&#x20;

When a capability is retrieved and it is not in the map, getCapability() is called to generate a `LazyOptional` that potentially contains the capability instance.

A listener is also automatically added to the resulting `LazyOptional` that removes the capability from the map if it is invalidated (the object holding the capability unloads).

## SidedCapabilityCache

Extends CapabilityCache, and holds two separate caches for client and server capabilities attached to an object. This exists because it is possible to [reach across logical sides](https://docs.minecraftforge.net/en/latest/concepts/sides/#reaching-across-logical-sides) in singleplayer (even though it is highly discouraged).&#x20;

Additionally, it might be desirable to handle capabilities differently on each side, which this data structure facilitates.

A sided capability cache behaves identically to a normal cache, and grants access to the server or client cache depending on what logical side the current thread belongs to. In other words, it "knows" if it is being accessed on the server or client, and will expose the appropriate cache.
