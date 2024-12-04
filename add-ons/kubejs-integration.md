# Cold Sweat KubeJS Integration

> KubeJS integration that allows modifying Cold Sweat's systems through JavaScript code

## Overview 

Cold Sweat's KubeJS integration provides a powerful API for modifying temperature mechanics, adding custom behaviors, and configuring various aspects of the mod through JavaScript code. This allows pack developers to customize Cold Sweat without needing Java development.

## Global Binding

The integration adds a global `coldsweat` binding that provides utility methods for working with temperatures and entities:

```js
// Example usage
coldsweat.getTemperature(player, "world") // Get world temperature
coldsweat.setTemperature(player, "base", 0.5) // Set base temperature
coldsweat.getColdInsulation(player) // Get total cold insulation
coldsweat.getHeatInsulation(player) // Get total heat insulation
coldsweat.getBlockTemperature(block) // Get block temperature
coldsweat.getBiomeTemperature(level, pos) // Get biome temperature
coldsweat.getTemperatureAt(level, pos) // Get temperature at position
```

## Events

### Registry Events

#### `ColdSweatEvents.registries` 
Fired during mod initialization to register custom content.

```js
// Example: Adding a block temperature effect
ColdSweatEvents.registries(event => {
    event.addBlockTemperature(0.5, "MC", builder => {
        builder.blocks("minecraft:furnace")
               .range(4)
               .maxEffect(2.0)
               .blockPredicate(block => block.getBlockState().getValue("lit"))
    })
})
```

Methods available:
- `addBlockTemperature(temperature, units, builder)` - Add temperature effect to blocks
- `addInsulator(builder)` - Add insulation to items/armor
- `addFoodTemperature(builder)` - Add temperature effects to foods
- `addHearthFuel/addBoilerFuel/addIceboxFuel(builder)` - Add fuel items
- `addCarriedItemTemperature(builder)` - Add temperature effects to carried items
- `addBiomeTemperature(biomeId, min, max, units)` - Set biome temperatures
- `addDimensionTemperature(dimId, temp, units)` - Set dimension temperatures
- `addStructureTemperature(structureId, temp, units)` - Set structure temperatures
- `addTempModifier(id, constructor)` - Register custom temperature modifier

### Temperature Events

#### `ColdSweatEvents.temperatureChanged`
Fired when an entity's temperature changes.

```js 
ColdSweatEvents.temperatureChanged(event => {
    // Cancel temperature changes above 100
    if (event.getTemperature() > 100) {
        event.cancel()
    }
})
```

#### `ColdSweatEvents.addModifier`
Fired when a temperature modifier is added to an entity.

```js
ColdSweatEvents.addModifier(event => {
    // Prevent heat modifiers in certain biomes
    if (event.getTrait() === "heat") {
        event.cancel()
    }
})
```

### Insulation Events

#### `ColdSweatEvents.applyInsulation`
Fired when insulation is applied to armor.

```js
ColdSweatEvents.applyInsulation(event => {
    // Modify insulator item
    event.setInsulator(newInsulator)
})
```

## Builder Classes

The integration provides builder classes for constructing various Cold Sweat objects:

### BlockTempBuilder
Configure block temperature effects:
- `blocks(String...)` - Add blocks by ID
- `blockTag(String)` - Add blocks from tag
- `maxEffect(double)` - Set maximum temperature change
- `range(double)` - Set effect range
- `blockPredicate(Predicate)` - Add conditions

### InsulatorBuilder  
Configure insulation items:
- `items(String...)` - Add items by ID
- `itemTag(String)` - Add items from tag
- `insulation(cold, heat)` - Set static insulation values
- `adaptiveInsulation(value, speed)` - Set adaptive insulation
- `slot(String)` - Set equipment slot
- `attribute(id, amount, operation)` - Add attribute modifier

### FoodBuilder
Configure food temperature effects:
- `items(String...)` - Add items by ID
- `temperature(double)` - Set temperature change
- `duration(int)` - Set effect duration
- `itemPredicate(Predicate)` - Add conditions

### CarriedItemBuilder  
Configure carried item temperatures:
- `items(String...)` - Add items by ID
- `temperature(double)` - Set temperature change
- `trait(String)` - Set affected trait
- `slots(int...)` - Set inventory slots
- `maxEffect(double)` - Set maximum effect

### FuelBuilder
Configure fuel items:
- `items(String...)` - Add fuel items
- `temperature(double)` - Set fuel value
- `itemPredicate(Predicate)` - Add conditions

## Temperature Units
Temperature values can be specified in different units:
- `"MC"` - Minecraft units (0-2 range)
- `"F"` - Fahrenheit
- `"C"` - Celsius

## Temperature Traits
Available temperature traits:
- `"world"` - Environmental temperature
- `"core"` - Internal temperature
- `"base"` - Static temperature offset
- `"body"` - Total body temperature
- `"rate"` - Temperature change rate

## Example Snippets

### Adding Block Temperature

```js
ColdSweatEvents.registries(event => {
    // Add heat to furnaces when lit
    event.addBlockTemperature(120, "F", builder => {
        builder.blocks("minecraft:furnace")
               .range(4)
               .maxEffect(2.0)
               .blockPredicate(block => block.getBlockState().getValue("lit"))
    })
})
```

### Adding Insulation Item

```js
ColdSweatEvents.registries(event => {
    // Add insulation to leather armor
    event.addInsulator(builder => {
        builder.items("minecraft:leather_chestplate")
               .insulation(4, 2)  // cold=4, heat=2
               .slot("armor")
               .attribute("cold_sweat:cold_resistance", 0.25, "addition")
    })
})
```

### Custom Temperature Modifier

```js
ColdSweatEvents.registries(event => {
    // Add time-based temperature modifier
    event.addTempModifier("mymod:time_modifier", data => {
        const time = data.entity.level.getDayTime()
        return temp => temp * (time / 24000)
    })
})
```

### Dimension Temperature

```js
ColdSweatEvents.registries(event => {
    // Set nether temperature
    event.addDimensionTemperature("minecraft:the_nether", 120, "F")
    
    // Add temperature offset to end
    event.addDimensionOffset("minecraft:the_end", -20, "F") 
})
```
