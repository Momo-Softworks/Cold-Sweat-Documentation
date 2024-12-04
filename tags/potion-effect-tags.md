# Potion Effect Tags

For now, there is only one mob effect tag:

### <mark style="color:blue;">`mob_effect/hearth_blacklisted`</mark>

A potion item with any of the effects in this tag will be disallowed from being placed in the hearth. This can be used to prevent certain overpowered or dangerous potion effects from being used in this way. All negative Vanilla potion effects are banned by default to prevent griefing.

```json
// By default, this tag is empty
{
  "replace": false,
  "values": [
  ]
}
```
