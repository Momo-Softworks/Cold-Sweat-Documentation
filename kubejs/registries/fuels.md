# Fuels

Cold Sweat allows for adding fuel items for the hearth, boiler, icebox, and soulspring lamp using KubeJS.

Each fuel type has its own function, but the syntax for all of them is the same:

* `addHearthFuel()`
* `addBoilerFuel()`
* `addIceboxFuel()`
* `addSoulspringLampFuel()`

### Format

```javascript
ColdSweatEvents.registries(event =>
{
    // Builder-style fuel definition
    event.addHearthFuel(fuel =>
        // Registers the fuel data to these items
        fuel.items("minecraft:snowball", "#minecraft:ice")
            // Negative for cold hearth fuel items
            // Should be positive for all other fuel types
            .fuel(-20)
            // The item must match this predicate to be valid
            .itemPredicate(itemStack => {
                return itemStack.getTag() != null && item.getTag().getBoolean("Wow")
            }))
})
```

