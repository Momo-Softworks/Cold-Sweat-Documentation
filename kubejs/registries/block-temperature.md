# Block Temperature

Cold Sweat has two methods for registering block temperatures, depending on how granular the developer's control must be:

### Base Configuration

```javascript
ColdSweatEvents.registries(event =>
{
    // First two parameters are block temperature and units
    event.addBlockTemperature(25.0, "f",
        // Third parameter is a builder-style block temperature definition
        blockTemp =>
        // Registers the block temperature to these blocks
        blockTemp.blocks("minecraft:bee_nest", "minecraft:bee_hive", "#minecraft:logs")
                 // Same as maxEffect from the configs.
                 .maxEffect(50)
                 // Max environment temperature in which the block will be effective
                 .maxTemperature(200)
                 // Range of the block
                 .range(3)
                 // The block must have this state to be valid. Can be a boolean, integer, or string
                 .state("lit", true)
                 // Causes the block temperature to not fade depending on the player's distance
                 .fades(false)
                 // Predicate for the block to match to be considered valid
                 // This "block" is a BlockContainerJS, which also holds world, position, state, and block-entity data
                 .blockPredicate(block => {
                    return block.getLevel().isDay()
                 }))
})

```

### Custom Temperature Definition

If more control is needed over how the block calculates its temperature, a more advanced version of this method can be used:

```javascript
ColdSweatEvents.registries(event =>
{
    event.addBlockTemperature(
        // First parameter is the builder
        blockTemp =>
        // Registers the block temperature to these blocks. Tags can be added via blockTag()
        blockTemp.blocks("minecraft:torch", "minecraft:wall_torch")
                 // Units must be defined before any other temperature parameter
                 // Possible values: F, C, or MC
                 .units("f")
                 // Same as maxEffect from the configs. This is in MC units only for now
                 .maxEffect(20)
                 // Max environment temperature in which the block will be effective
                 .maxTemperature(250)
                 // Min environment temperature. If it is below this value, the block will be ineffective
                 .minTemperature(0)
                 // Range of the block
                 .range(3),
        // Second parameter is a function that gives the current world, affected entity, 
        // state, position, and distance to the entity
        (level, entity, state, pos, distance) => {
            // Freedom to perform any necessary calculations, then return the block's temperature
            // Heat up low-health players, cool down high-health players
            var health = entity.getHealth()
            if (health < 10) 
               return 10
            else 
               return -10
        })
})

```
