# NBT Requirement

Cold Sweat uses a custom NBT checker to evaluate a target item, entity, or block. The general syntax remains the same, but Cold Sweat's NBT system also supports defining multiple accepted values for a given NBT tag. This is done by using custom structures.

## `cs:any_of`

Checks the target NBT against a list of provided values, passing if any of the values match. This is done by specifying a compound tag, with the only key being "`cs:any_of`" and its value being the list of options.

Example:

```json
"nbt": {
  "HeatLevel": {
    "cs:any_of": [
      "Medium",
      "High"
    ]
  }
}
```

This translates to:

```json
"nbt": {
  "HeatLevel": "Medium"
  or
  "HeatLevel": "High"
}
```

If HeatLevel matches either of the given values, the check will pass. This is a very useful alternative to defining multiple possible NBT structures for the same thing.

## Numerical Ranges

It is possible to define a range of accepted values if the tag being checked is a number.&#x20;

Example:

```json
"nbt": {
  "Damage": "50:100"
} 
```

Checks if an item has lost between 50 and 100 durability. Note that ranges are represented by a string value of two numbers separated by a colon ( : ). Decimal values are also accepted.

It is also possible to omit the lower or upper limit. For example:

```json
"nbt": {
  "Damage": "50:", // Checks if "Damage" is 50 or more
  "Variable": ":-10" // Checks if "Variable" is -10 or less
} 
```

## List Contents

When checking the contents of a list, the Vanilla NBT system would require that the contents of the list match the test exactly. This isn't always desired, so Cold Sweat introduces two new arguments, which are formatted in a similar way to `cs:any_of`:&#x20;

### `cs:contains_any`

Checks if the list contains any, but not necessarily all, of the provided values.

Example:

```json
// Check: The list must contain either "c" or "s"
"nbt": {
  "StoredValues": {
    "cs:contains_any": [
      "c",
      "s"
    ]    
  }
}

// This NBT will pass the check:
{
  "StoredValues": [
    "a",
    "b",
    "c" // Here!
  ]
}

// This list will fail the check:
{
  "StoredValues": [
    "d",
    "e",
    "f"
    // None of these are "c" or "s"
  ]
}
```

### `cs:contains_all`

Checks if the list contains all of the given values, and fails if one or more of the specified elements are not present. The list may contain additional entries not included in the check.

Example:

```json
// Check: This chest must contain all of these items
"nbt": {
  "Items": {
    "cs:contains_all": [
      // 1 to 54 torches in slot 0 (first slot)
      {
        "Slot": 0
        "id": "minecraft:torch",
        "Count": "1-54"
      },
      // Chameleon molt in slot 5 (sixth slot)
      {
        "Slot": 5,
        "id": "cold_sweat:chameleon_molt"
      }
    ]
  }
}

// This NBT will pass the check:
{
  "Items": [
    // Here are the torches!
    {
      "Slot": 0b,
      "id": "minecraft:torch",
      "Count": 12b // This is within our allowed range
    },
    // We don't care about the rotten flesh in slot 1
    {
      "Slot": 1b,
      "id": "minecraft:rotten_flesh",
      "Count": 4b
    }
    // Here's the chameleon molt!
    {
      "Slot": 5b,
      "id": "cold_sweat:chameleon_molt",
      "Count": 64b // The count doesn't matter
    }
  ]
}
```
