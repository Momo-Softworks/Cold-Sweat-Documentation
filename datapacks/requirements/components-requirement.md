# Components Requirement

This is essentially the 1.21+ version of an NBT requirement, which checks an item's [data components](https://minecraft.wiki/w/Data_component_format). It also has similar quality-of-life improvements over Vanilla's system, but has some limitations due to the inflexibility of data components.

**These custom functions can be used to evaluate the component itself, but not any data inside the component.**

## `cs:any_of`

Checks the target NBT against a list of provided values, passing if any of the values match. This is done by specifying a compound tag, with the only key being "`cs:any_of`" and its value being the list of options.

Example:

```json
"components": {
  "minecraft:rarity": {
    "cs:any_of": [
      "rare",
      "epic"
    ]
  }
}
```

This translates to:

```json
"components": {
  "minecraft:rarity": "rare"
  or
  "minecraft:rarity": "epic"
}
```

If the item's rarity is either rare or epic, the check will pass. This is a very useful alternative to defining multiple possible component checks for the same thing.

## Numerical Ranges

It is also possible to define a range of accepted values if the tag being checked is a number.&#x20;

Example:

```json
"components": {
  "minecraft:damage": "50-100"
} 
```

Checks if an item's damage is somewhere between 50 and 100. Note that ranges are represented by a string value of two numbers separated by a hyphen ( - ). Decimal values are also accepted.
