# Disenchanter Renderer — the visuals (fluid inside + spinning item)

This is the big one. It reproduces the original 1.20.1 visuals on 1.21.11's new
render-state pipeline, mirroring Create Fly's own ItemDrainRenderer.

## Files
- client/DisenchanterRenderer.java        (NEW — the renderer)
- client/EnchantmentIndustryClient.java   (UPDATED — registers the renderer)

## What it draws
1. Fluid level inside the casing (you'll "see into the block" with green XP)
2. The held item moving across the internal belt and SPINNING while processed (1:1 with 1.20.1)
3. A swelling experience box around the item during processing

## EXPECTED: this may need a fix round or two
This renderer uses ~25 of 1.21.11's brand-new render classes. I verified all the CLASS
names against the mappings, and mirrored Create Fly's working ItemDrainRenderer for the
method calls. But a few Mojang method/field names on the newest render classes I could not
100% confirm without compiling. The most likely things to need adjustment:

| In the code | If it errors, likely fix |
|---|---|
| `SubmitNodeCollector` / `submitCustomGeometry` | Mojang name for the render queue submit; mirror ItemDrainRenderer's `queue.method_73483` equivalent |
| `SubmitNodeCollector.CustomGeometryRenderer` | the `class_11660` "Custom" interface — exact Mojang name |
| `BlockEntityRenderState.extractRenderState(...)` static | the `method_74399` helper name |
| `state.pos` / `state.lightCoords` | BlockEntityRenderState field names (pos/blockPos, lightCoords/lightmapCoordinates) |
| `CameraRenderState.pos` | the camera position field |
| `ItemModelResolver.updateForTopItem(...)` | the `method_65596` "update" name; could be `updateForTopItem` |
| `ItemStackRenderState.isGui3d()` / `render(...)` | the render + side-lit accessor names |
| `context.getItemModelResolver()` | `comp_4536` accessor on the context |
| `BlockEntityRenderer.CrumblingOverlay` | the `class_11792` inner class path |

Send me ALL the errors from the build and I'll map each one — they're isolated name
swaps, not logic problems. The logic mirrors ItemDrainRenderer exactly.

## After it compiles
Place a Disenchanter, feed it an enchanted item (belt or right-click). You should see
the item rise and spin while green experience swells around it, and the tank fill with
visible green fluid inside the block.
