# Item Requirement

An item requirement is a set of criteria that an item must meet. They currently support checking:

* Item IDs and tags
* Item count (though most things don't check this)
* durability
* enchantments (including enchanted books)
* potion type (for potion items)
* nbt (1.20-) or components (1.21+)

These are modeled after Vanilla's item predicates, and are structured like so:

<pre class="language-json" data-full-width="false"><code class="lang-json">{
  // The items or item tags
  "items": [
    "minecraft:iron_chestplate",
    "#forge:armors/helmets"
  ],
  // Isn't used in most cases, but Vanilla's item predicates have it
  "count": {
    "min": 2,
    "max": 8
  },
  // Remaining durability on the item
  "durability": {
    "min": 100,
    "max": 150
  },
  // Also supports tags, but there aren't any enchantment tags in Vanilla
  "enchantments": [
    {
      "enchantment": "minecraft:flame",
      "levels": {
        "min": 1,
        "max": 2
      }
    },
    // "levels" field is optional. Any level of "infinity" is accepted
    {
      "enchantment": "minecraft:infinity"
    }
  ],
  // The item's remaining durability must be within these bounds
  "durability": {
    "min": 1,
    "max": 100
  },
  // The item must be a lengthened fire resistance potion
  "potion": "minecraft:long_fire_resistance",
  // (1.20-) The item must have these NBT tags
  "nbt": {
    "SomeTag": true,
    "SomeOtherTag": "1-10"
    // NBT tags can be nested
    "SomeNestedCompoundTag": {
      "ThisTagValue": 42,
      "ThisTagName": "Ramphord Yortold"
    }
  },
  // (1.21+) The item must have these components
  "components": {
    // Item has between 1 and 50 durability lost
    "minecraft:damage": "1-50"
  }
<strong>}
</strong></code></pre>

