# Registry Removals

`/remove/`

Cold Sweat has a special type of config that allows for the "removal" of certain other configs. For example, a registry removal can be used to remove the insulation from chameleon molt, or to remove the temperature increase caused by being on fire.

Registry removals can target TOML, JSON, and KubeJS registries.

{% hint style="info" %}
N**ote:** Registry removals do not actually remove the configuration settings. They prevent them from being applied at the final stage of the config loading process.
{% endhint %}

### Format

```json
{
  // Configs to be removed
  // The registry name of each config is given on its respective docs page
  "registry": "entity/entity_temp",
  // Config formats to check ("toml", "json", "kubejs")
  // Can also be a single element: "config_type": "json"
  "config_type": [
    "json"
  ],
  // List of IDs to check. These IDs are registered by Minecraft when datapacks are parsed
  // Only JSON registries have IDs, so this does not work for TOML or KubeJS configs
  "entries": [
    "cold_sweat:on_fire"
  ],
  // Any configs matching the given data structure(s) are removed
  // These are NBT requirements, so Cold Sweat's special NBT functions also work here
  "matches": [
    {
      "flags": {
        "is_baby": false
      },
      "nbt": {
        "Burning": 1
      }
    },
    {
      // ..more checks
    }
  ]
}
```
