# Biome Temperature

Cold Sweat allows for adding biome temperatures via KubeJS

### Format

```json
ColdSweatEvents.registries(event =>
{
    event.addBiomeTemperature(
        50, // minimum (midnight) temperature
        80, // maximum (noon) temperature
        "F", // Units; either "F", "C", or "MC"
        // Biome IDs
        new Array("minecraft:forest", "minecraft:plains", "some_mod:modded_biome"),
        -20 // Water temperature, relative to the biome's temperature
    )
    // If water temperature is omitted, you can define the biome IDs like so (varargs):
    event.addBiomeTemperature(50, 80, "F",
        // Biome IDs
        "minecraft:forest", "minecraft:plains", "some_mod:modded_biome"
    )
}
```

Dimension temperature offsets are defined in the same way, such as:

```json
ColdSweatEvents.registries(event =>
{
    event.addBiomeOffset(-5, 10, "F", "minecraft:forest", "minecraft:plains")
}
```
