# Enchantments

This directory contains all possible enchantments that can be applied to items.

File names **MUST** be their enchantment ID (visible in the item's `custom_data` component under the `enchantments` tag).

## Levels

There are three enchantment level values; `max`, `good`, and `average`.

The `max` enchantment level is self-explanatory; it's the maximum level an enchantment can be applied at.

The `good` enchantment level is the maximum level an enchantment can obtained at through normal (non-anvil) means, such as
being dropped as an enchanted book or through the enchantment table/hex. Note that in terms of how they are displayed in game,
only levels **above** this level are shown as special!

The `average` level is usually the same value as `good`, but can be overridden for some special cases (but should be limited);
current special cases include Turbo and Vitality enchantments.

All of these values can be set to -1, with different effects based on which value is set (only the first listed case):
- If `max` is set to -1, the enchantment is always considered to be max level
- If `good` is set to -1, the enchantment is always considered average until max level
- If `average` is set to -1, the enchantment is considered average until `good + 1` 

## Enchantment Data Type Schemas

<details>
<summary>Standard</summary>

```json5
{
  "type": "standard",
  // this must match the file name of the enchantment; this is required due to legacy implementation details
  // that will eventually be addressed
  "id": "efficiency",
  // the name that this enchantment is displayed as by Hypixel in the item tooltip
  "name": "Efficiency",
  // see above level description
  "max": 5,
  "good": 5,
  "average": 5, // optional, uses the value of good if not set
  // the item types this item is applicable to - these are all relative paths for files in `../types/`
  "applicableTo": [
    "tools/pickaxes",
    "tools/shovel",
    "tools/axe",
    "tools/farming",
    "tools/shears"
  ],
  // optional array of string item IDs
  // if not empty then __only__ the items listed in this array can have this enchantment applied to it
  // note that these items must also pass the above item type criteria
  // see the ultimate enchantment below for an example use case
  "exclusiveToItems": [
  ],
  // optional, allows for changing enchantment values for certain items
  "overrides": [
    {
      // these can match by either an item ID
      "when": {
        "type": "item",
        "id": "STONK_PICKAXE"
      },
      "max": 6,
      "good": 5
    },
    {
      // or an item type
      "when": {
        "type": "item",
        "types": [
          "tools/pickaxes"
        ]
      },
      "max": 10,
      "good": 5
    }
    // there are also a few additional condition types not shown above:
    // - {type: "all", conditions: [...]}
    // - {type: "any", conditions: [...]}
    // - {type: "not", condition: ...}
    // - {type: "has", enchantment: "..."}
  ],
  "metadata": {
    // Allows for attaching arbitrary metadata for mod features
  }
}
```

</details>

<details>
<summary>Stacking</summary>

```json5
{
  "type": "stacking",
  "id": "absorb",
  "name": "Absorb",
  // the key under which the stacking value for this enchantment is stored as in the item's `custom_data` component
  "nbtKey": "absorb_logs_chopped",
  // this tiers array replaces the standard max/good/average levels; all stacking enchantments are considered
  // average until the level before max (level 9 in the case of most stacking enchantments, which go up to 10)
  "tiers": [
    // level 1 is implicitly skipped as it's the level you apply it at
    1000, // level 2 requires 1000 logs chopped
    5000, // 5000 for level 3
    25000, // and so on
    100000,
    300000,
    1500000,
    5000000,
    10000000, // level 9 - considered to be good for enchantment tooltip purposes (i.e. it gains a gold color with default settings)
    25000000
  ],
  "applicableTo": [
    "tools/axe"
  ]
}
```

</details>

<details>
<summary>Ultimate</summary>

```json5
{
  "type": "ultimate",
  "id": "ultimate_jerry",
  "name": "Ultimate Jerry",
  // ultimate enchantments only support a max level, and do not have a concept of a good or average level
  "max": 5,
  "exclusiveToItems": [
    "ASPECT_OF_THE_JERRY",
    "ASPECT_OF_THE_JERRY_SIGNATURE"
  ]
}
```

</details>
