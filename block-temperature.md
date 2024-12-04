# Block Temperature

In Cold Sweat, blocks can be configured to change the world's temperature for players around them, up to 7 blocks away by default.

There are two ways to configure block temperatures:

## Using Config Files

This method uses simple JSON parameters in Cold Sweat's `world_settings.toml` config file to create basic block temperatures. The syntax is as follows:

{% code fullWidth="false" %}
```json
[["block_ids", temperature, range, *max-effect, *predicates, *nbt, *temp-limit], 
 [etc...], 
 [etc...]]
```
{% endcode %}

### Arguments

* <mark style="color:blue;">`block-ids`</mark>: The ID(s) of the block(s) emitting this temperature. Should be a qualified string ID (i.e. `"minecraft:lava"`, `"cold_sweat:boiler"`).

{% hint style="info" %}
block-ids can also be a comma-separated list of blocks, allowing this effect to be applied to multiple blocks (i.e. `"minecraft:red_wool,minecraft:white_wool"`). Notice there is no space between blocks.

Using a hashtag (#) signifies a tag, allowing this effect to be applied to all blocks under the tag (i.e. `"#minecraft:logs"` includes oak logs, birch logs, etc.) See [**Tags**](https://minecraft.fandom.com/wiki/Tag#Block_tags) for more information.
{% endhint %}

* <mark style="color:blue;">`temperature`</mark>: A decimal representing the temperature of the block, in Minecraft units. \
  (1 MC unit = **42** °F or **23.3** °C)
* <mark style="color:blue;">`range`</mark>: A decimal or integer from **0**-16 representing the range of the block's effect, in blocks.
* <mark style="color:blue;">`max effect`</mark> (Optional) : A decimal or integer that applies a hard cap to how much this block can affect a player's temperature, no matter how many of the block are near the player. (i.e. If the block's `temperature` is **0.15**, and the `max effect` is **0.3**, the cap would be reached after **two** blocks, and a third block would not change the total).
* <mark style="color:blue;">`predicates`</mark> (Optional) : A list of BlockState predicates that must be true for the block to emit temperature. For example, a campfire must have `lit=true` to emit heat.
* <mark style="color:blue;">`nbt`</mark> (Optional): The NBT data that the block must have for the temperature to be applied. If the targeted block is not a [Block Entity](https://minecraft.wiki/w/Block_entity), this parameter does nothing.
* <mark style="color:blue;">`temp limit`</mark> (Optional): The maximum world temperature at which this block temp will be effective. If the world's temperature exceeds this value, the block will not emit anything.\
  If the temperature for this block temp is negative, this represents the minimum world temperature.



{% hint style="warning" %}
The following section assumes the reader has a solid grasp on Java programming and modding with Forge.
{% endhint %}

## Using BlockTemps

These must be added via 3rd party Forge mods. Because they are programmed in Java, which is more flexible than JSON, BlockTemps provide more precise control in how they work.

### Creating a BlockTemp

A BlockTemp extends the base class **`BlockTemp`**, which has several methods that control how the block(s) temperature works. The <mark style="color:purple;">**`getTemperature()`**</mark> method must be overridden for implementations:

<pre class="language-java" data-full-width="false"><code class="lang-java"><strong>@Override
</strong><strong>public double getTemperature(Level level, LivingEntity entity, 
</strong><strong>                             BlockState state, BlockPos pos, double distance)
</strong></code></pre>

#### Parameters:

* <mark style="color:blue;">`entity`</mark>: The entity that this BlockTemp is trying to affect.
* <mark style="color:blue;">`state`</mark>: The BlockState of the block associated with this BlockTemp. This can be used to make the effect conditional, such as when a furnace is lit. One might want to use <mark style="color:yellow;">**`hasBlock()`**</mark> to determine if the given block is valid for this BlockTemp to ensure this temperature isn't applied to the wrong block.
* <mark style="color:blue;">`pos`</mark>: The BlockPos, or x/y/z position, of the block associated with this BlockTemp.
*   <mark style="color:blue;">`distance`</mark>: The distance of the entity from the center of the block. This can be used to make the effect of the block weaken as distance increases, since **this does not happen by default**. A common way to calculate this is:

    ```
    CSMath.blend(<temperature>, 0, distance, 0.5, <range of the block>);
    ```

Returns a double representing the temperature of this block relative to the given player. Note that this value is in Minecraft units.

### Registering a BlockTemp

A BlockTemp must be **registered** in order to be associated with any blocks. This can be easily done by subscribing to `BlockTempRegisterEvent` (**Forge** event bus) and calling the event's <mark style="color:yellow;">**`register()`**</mark> method, passing in an instance of the BlockTemp there.

#### Constructor

BlockTemps have a constructor, provided by the **`BlockTemp`** class, that allows for passing in a comma-separated list (or **`Array`**) of blocks that will be associated with the BlockTemp. \
This can be done either in the class declaration by calling <mark style="color:yellow;">**`super()`**</mark> in a default constructor, or when a new instance of the BlockEffect is being created to pass into the <mark style="color:yellow;">**`register()`**</mark> method.

### Examples

{% code fullWidth="false" %}
```java
public class YourBlockEffect extends BlockEffect
{
    public YourBlockEffect()
    {
        // We are calling the super constructor to add FURNACE
        // to the list of blocks associated with this BlockTemp.
        super(Blocks.FURNACE);
    }
    
    @Override
    public double getTemperature(Player player, BlockState state, 
                                 BlockPos pos, double distance)
    {
        // Testing if the block is associated with this BlockEffect
        if (this.hasBlock(state.getBlock())
        {
            // Because we only registered FURNACE, we can 
            // now assume the block is a furnace.
            
            // Only affect the player if the furnace is lit
            if (state.getValue(AbstractFurnaceBlock.LIT))
            {
                // blend() is a very helpful function for interpolating between values.
                // In this case we are using it to calculate the temperature at this distance.
                
                // As distance goes from 0.5 to 7 (farther away), 
                // the returned value goes from 0.3 to 0 (weakens).
                return CSMath.blend(0.3, 0, distance, 0.5, 7);
            }
        }
    } 
}
```
{% endcode %}

{% code fullWidth="false" %}
```java
@SubsribeEvent
public static void onBlockTempsRegister(BlockTempRegisterEvent event)
{
    // We are passing in the blocks here instead of making a default constructor.
    event.register(new MyBlockEffect(Blocks.STONE, Blocks.OBSIDIAN));
    
    // We are not passing in blocks because we are
    // handling it in YourBlockEffect's default constructor.
    event.register(new YourBlockEffect());
}
```
{% endcode %}
