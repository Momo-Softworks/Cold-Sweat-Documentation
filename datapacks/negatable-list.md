# Negatable List

### Negatable List

A negatable list is a collection of **requirements** and **exclusions** that can be used in place of most conditional parameters in Cold Sweat. Using them, it is possible to perform multiple checks at once, or utilize exclusions to define conditions in which the check should fail.

{% hint style="info" %}
Fields that support negatable lists will usually be labeled as such, but it is safe to assume that any "requirement" (item, entity, block, NBT, etc.) is probably negatable.
{% endhint %}

Minimal example:

```json
{
  "require": [
    // A list of checks that must pass for this requirement to succeed
  ],
  "exclude": [
    // A list of checks that FAIL this requirement if they pass
  ]
}
```

For example, here is a negatable list of entity requirements:

```json
{
  "entity": {
    // -- Requirements --
    "require": [
      // requirement 1: must be on fire and have some NBT data
      {
        "flags": {
          "on_fire": true
        },
        "nbt": {
          "SomeBooleanTag": 1
        }
      },
      // requirement 2: must have regeneration 2
      {
        "effects": {
          "minecraft:regeneration": {
            "amplifier": 1
          }
        }
      }
    ],
    // -- Exclusions --
    "exclude": [
      {
        // exclusion: can't be wearing an iron helmet
        "equipment": {
          "head": {
            "items": [
              "minecraft:iron_helmet"
            ]
          }
        }
      }
    ]
  }
}
```

#### Optional Parameters

By default, a negatable list of requirements will succeed if:

* At least one requirement passes
* None of the exclusions pass

However, it is possible to change this behavior using two optional parameters:

* **`require_all`**\
  ALL requirements must be match the target for the list to succeed. If one requirement does not match, the entire negatable list will fail and exclusions will also be skipped.
* **`exclude_all`**\
  ALL exclusions must match the target for the list to fail. If one exclusion does not match, the rest of the exclusions will be ignored.

```json
{
  "requirements": [], // List of requirements
  "require_all": true,
  "exclusions": [], // List of exclusions
  "exclude_all": true
  // Both of these parameters are "false" by default
}
```
