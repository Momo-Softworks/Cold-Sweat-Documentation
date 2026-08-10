# Dimension Temperature

Cold Sweat allows for adding dimension temperatures via KubeJS

### Format

```json
ColdSweatEvents.registries(event =>
{
    event.addDimsneionTemperature(
        50, // minimum (midnight) temperature
        80, // maximum (noon) temperature
        "F", // Units; either "F", "C", or "MC"
        // Dimension IDs (varargs array)
        "minecraft:overworld", "#forge:has_ceiling"
    )
    // A single temperature value can also be applied, which stands in for both min/max
    event.addDimensionTemperature(75, "F", "minecraft:overworld", "#forge:has_ceiling")
}
```

Dimension temperature offsets are defined in the same way, such as:

```json
ColdSweatEvents.registries(event =>
{
    event.addDimensionOffset(-20, 10, "F", "minecraft:overworld")
}
```
