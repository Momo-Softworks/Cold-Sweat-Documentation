# DynamicHolder

A `DynamicHolder` typically holds config setting data. It can be thought of a supplier that caches its own value upon being queried for the first time. They also support serialization/deserialization via NBT.

The value of a DynamicHolder is obtained via the following methods:

```java
get()
```

### Registry Access

Some holders that rely on Minecraft registries (biomes, dimensions, structures, etc.) require an instance of <mark style="color:blue;">`RegistryAccess`</mark> in order to be retrieved, and will throw an error if one is not supplied:

```java
get(RegistryAccess registryAccess)
```

An instance of `RegistryAccess` can be retrieved from any <mark style="color:blue;">`MinecraftServer`</mark>, <mark style="color:blue;">`Entity`</mark>, or <mark style="color:blue;">`Level`</mark> object (if it is not on the client-side). In the event that none of these sources are available, Cold Sweat provides a method for obtaining an instance of `RegistryAccess`:&#x20;

```java
RegistryHelper.getRegistryAccess()
```

Using this method is generally discouraged, as it uses potentially unreliable methods of retrieving an instance of  `RegistryAccess` from a static context. Only use this if there is no alternative and you know what you are doing.

{% hint style="warning" %}
`RegistryAccess` is only reliably present after the integrated server has loaded. Attempting to retrieve it before this point could result in `null`.
{% endhint %}
