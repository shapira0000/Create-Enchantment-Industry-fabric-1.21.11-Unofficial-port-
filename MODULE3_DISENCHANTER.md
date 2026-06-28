# Module 3 — The Disenchanter (complete, buildable batch)

This batch adds the full Disenchanter: a block you can place, that strips non-curse
enchantments from items and fills its internal tank with Liquid Experience.

## New Java files
- content/disenchanter/Disenchanting.java        (logic — already tested clean)
- content/disenchanter/DisenchanterItemHandler.java   (item input side)
- content/disenchanter/DisenchanterBlockEntity.java    (the processing core)
- content/disenchanter/DisenchanterBlock.java          (the block)
- registry/CeiBlocks.java                              (block + block-item registration)
- registry/CeiBlockEntities.java                       (block-entity-type registration)
- registry/CeiConfig.java                              (tank capacity)
- EnchantmentIndustry.java                             (updated: wires the above in)

## New assets
- blockstates/disenchanter.json, models/block + item, 2 textures
- loot_table/blocks/disenchanter.json  (note: 1.21 uses singular "loot_table")
- lang/en_us.json updated with the block name

## How to test in-game once it builds
1. runClient
2. Creative menu → search "Disenchanter" → place it
3. Put an enchanted item on top (right-click with it), or feed it via a Create belt
4. It should disenchant the item and fill with green Liquid Experience
5. Attach a Create fluid pipe/tank to a side to pull the experience out

## Likely build errors to expect (and the fix is quick)
This is a big batch touching new 1.21.11 systems I can't compile here. Most probable:

1. **ValueInput/ValueOutput package** (DisenchanterBlockEntity imports):
   I used `net.minecraft.world.storage.ValueInput` / `ValueOutput`.
   If "cannot resolve", it's likely `net.minecraft.world.level.storage.*`.
   Send me the error and I'll correct the import.

2. **`method_66473`** in DisenchanterBlockEntity — I kept this intermediary name for the
   block-removal hook because its Mojang name was ambiguous. If it errors as "cannot resolve
   method", tell me and I'll swap to the Mojang name (likely `preRemoveSideEffects` or similar).

3. **Block/Item Properties methods** — `setId`, `useBlockDescriptionPrefix`, `ofFullCopy`.
   These are 1.21.11 names verified against Create Fly's own AllBlocks/AllItems, should be fine.

4. **`AllShapes.CASING_13PX`**, **`ComparatorUtil.levelOfSmartFluidTank`**, **`IBE.onRemove`** —
   verified present in Create Fly. Should resolve.

5. Override signatures on the block (`useItemOn`, `getShape`, `onRemove`, `getAnalogOutputSignal`,
   `hasAnalogOutputSignal`, `isPathfindable`) — Mojang 1.21.11 names. If any signature mismatch,
   send the error.

Don't be alarmed by a batch of errors — that's expected for a jump this size. Send them all
and I'll work through them. The logic underneath is faithful to the original.

## Deferred (coming later, per our plan)
- Absorbing XP from nearby players/orbs
- Advancements + goggle tooltip
- The custom DisenchantRecipe override system
- The block-entity RENDERER (the item floating + spinning above the block). Without it,
  the block works but the item being processed won't show visually yet.
