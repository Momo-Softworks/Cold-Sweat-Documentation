# Entity Requirement

An entity requirement is a set of criteria that an entity must meet. They currently support checking:

* Entity IDs and tags
* Entity location:
  * x/y/z
  * biome at entity's position
  * structure at entity's position
  * &#x20;dimension at entity's position
  * light level at entity's position
  * block at entity's position
  * fluid at entity's position
* Block entity is standing on (same as "Entity location", but 1 block below)
* Potion effects
* NBT
* Entity flags:
  * On fire
  * Sneaking
  * Sprinting
  * Swimming
  * Invisible
  * Glowing
  * Baby
* Equipment (head, chest, legs, feet, main hand, offhand)
  * Each of these is an item requirement
* Entity-specific data:
  * Players:
    * Game mode (survival, creative, adventure, spectator)
    * Stats (blocks mined, distance walked, etc)
    * Unlocked recipes
    * Advancements
    * Entity the player is looking at
  * Entities with variants (cats, horses, etc.)
    * Variant name
  * Fishing bobber:
    * If the bobber is in an open lake
  * Lightning bolt:
    * Entities struck by the lightning
    * Blocks set on fire
  * Entity is wearing piglin-pacifying armor
  * Village raiders:
    * Is the raid captain
    * Part of an active raid
    * Active raid info
* Vehicle (entity that this entity is riding)
* Passenger (entity riding this entity)
* Target (entity being targeted by this entity, if it is a hostile mob)

Entity requirements allow for defining a requirement for the entity that this entity is mounted to, or the entity that this entity is targeting. For example, a zombie is targeting a specific player, or is riding a horse. \
\
This can work recursively, too, (i.e. checking what entity the horse is riding), **but only up to 16 layers (or 4 for 1.21+)** due to the technical limitations of how JSON is read in Minecraft.
