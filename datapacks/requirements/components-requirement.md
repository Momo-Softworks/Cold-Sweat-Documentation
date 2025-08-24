# Components Requirement

This is essentially the 1.21+ version of an NBT requirement, which checks an item's [data components](https://minecraft.wiki/w/Data_component_format).&#x20;

{% hint style="info" %}
**Note:**&#x20;

As of version 2.4, components requirements have been rewritten to behave like NBT requirements, meaning they implement custom NBT features such as `"cs:any_of"` and numerical ranges.&#x20;

On earlier versions, component data will be strictly matched against the target. Furthermore, components requirements must follow valid component syntax, meaning no fields in the component's structure may be omitted.
{% endhint %}

Example:

```json
{
  "components": {
    "minecraft:damage": 10,
    "minecraft:custom_name": 
  }
}
```

<h4 align="center"><strong>For full documentation, see</strong> <a href="nbt-requirement.md"><strong>NBT Requirement</strong></a><strong>.</strong></h4>
