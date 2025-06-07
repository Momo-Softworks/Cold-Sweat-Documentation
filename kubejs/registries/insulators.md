# Insulators

Cold Sweat allows for adding insulation items, armor, and curios using KubeJS.

### Format

```javascript
ColdSweatEvents.registries(event =>
{
    // Builder-style insulator definition
    event.addInsulator(insulator =>
        // Registers the insulator to these items
        insulator.items("minecraft:ink_sac", "#minecraft:dye")
                 // Insulation that the item provides (cold, heat)
                 .insulation(2, 2)
                 // Adaptive insulation that the item provides (insulation, adaptSpeed)
                 .adaptiveInsulation(4, 0.005)
                 // Insulation type (item, armor, or curio)
                 .slot("item")
                 // The item must match this predicate to be valid
                 .itemPredicate(itemStack => {
                    // This is a bad example because the count will always be 1
                    return itemStack.getCount() < 10
                 })
                 // The entity wearing the insulation must match this predicate
                 .entityPredicate(entity => {
                    entity.getHealth() > 5
                 })
                 // Adds an attribute modifier to the insulator
                 // This can be called multiple times for multiple modifiers
                 // operation types (1.20-): add, multiply_base, multiply_total
                 // operation types (1.21+): add_value, add_multiplied_base, add_multipled_total
                 .attribute("generic.attack_speed", 1.0, "add")
                 // Adds immunity to a temperature modifier
                 .immuneToModifier("cold_sweat:on_fire", 0.5)
                 // true: items with more than 2 total insulation will take up multiple slots
                 // false: item will take up only one slot, no matter how much insulation it has
                 // Only used for the "item" insulation type
                 .fillSlots(true)
                 // Hides the insulation & attributes this item provides from the tooltip if its predicates are unmet
                 // Otherwise, unmet insulation will show a red line through it in the tooltip
                 .hideIfUnmet(false))
})

```
