# Food Temperature

Cold Sweat allows for adding temperature properties to food using KubeJS.

### Format

```javascript
ColdSweatEvents.registries(event =>
{
    // Builder-style food temperature definition
    event.addInsulator(food =>
        // Registers the food temperature to these items
        food.items("minecraft:bread", "#minecraft:fishes")
            // The temperature of the food
            .temperature(-20)
            // The duration of the food, in ticks (omit for instant effect)
            .duration(1200)
            // The item must match this predicate to be valid
            .itemPredicate(itemStack => {
                return itemStack.getCount() < 10
            })
            // The entity wearing the insulation must match this predicate
            .entityPredicate(entity => {
                entity.getHealth() > 5
            }))
})
```

