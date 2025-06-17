# Carried Item Temperature

Cold Sweat allows for adding temperature properties to items when in the player's inventory using KubeJS.

### Format

<pre class="language-javascript"><code class="lang-javascript">ColdSweatEvents.registries(event =>
{
    // Builder-style food temperature definition
    event.addCarriedItemTemperature(carried =>
        // Registers the food temperature to these items
        carried.items("minecraft:blaze_powder", "#forge:rods/blaze")
            // The temperature of the item food
            .temperature(0.5)
            // The temperature trait to affect. Can be any trait
            .trait("world")
            // Specific inventory slots that the item will work in
            .slots(9, 10)
            // A range of slots that the item will work in
            // These slots are for the hotbar
            .slotsInRange(0, 8)
            // Equipment slots that the item will work in
            // Any of: head, chest, legs, feet, inventory, hotbar, <a data-footnote-ref href="#user-content-fn-1">curio</a>, hand
            .equipmentSlots("inventory", "hand", "curio")
            // The maximum amount that any quantity of this item can affect the player
            .maxEffect(2)
            // Give the item an attribute modifier when active on the player
            // Can be called multiple times to add more than one
            // Operations (1.20-): add, multiply_base, multiply_total
            // Operations (1.21+): add, add_multiplied_base, add_multiplied_total
            .attribute("generic.movement_speed", 0.5, "addition")
            // Add immunity to a temperature modifier
            // Can be called multiple times to add immunity to more than one modifier
            .immuneToModifier("cold_sweat:water", 1.0)
            // The item must match this predicate to be valid
            .itemPredicate(itemStack => {
                return !itemStack.isDamaged()
            })
            // The entity wearing the insulation must match this predicate
            .entityPredicate(entity => {
                entity.getHealth() > 5
            }))
})
</code></pre>



[^1]: [https://www.curseforge.com/minecraft/mc-mods/curios](https://www.curseforge.com/minecraft/mc-mods/curios)
