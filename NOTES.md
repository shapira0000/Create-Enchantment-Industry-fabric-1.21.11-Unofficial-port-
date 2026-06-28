# Cutout transparency fix (1.21.11)

## The root cause (you were exactly right)
In 1.21.11 on Fabric, the `render_type` field in the block model JSON is NOT enough —
Minecraft determines transparency per-quad and you must register the block's render layer
explicitly via Fabric API's BlockRenderLayerMap. Without it, the transparent (cutout) pixels
in disenchanter_top.png render as solid/default instead of see-through.

Your texture is a proper cutout (192 opaque + 64 fully-transparent pixels, 0 partial), so
the correct layer is CUTOUT.

## The fix
Added to EnchantmentIndustryClient.onInitializeClient():
    BlockRenderLayerMap.putBlock(CeiBlocks.DISENCHANTER, BlockRenderLayer.CUTOUT);

## If it doesn't compile — two names I couldn't 100% verify:
1. `net.minecraft.client.renderer.BlockRenderLayer` — the Mojang package for the enum
   (Yarn: net.minecraft.client.render.BlockRenderLayer). If "cannot resolve", Ctrl+click
   to find the real package, or use autocomplete on `BlockRenderLayer.`.
   The value CUTOUT should exist on it.
2. `BlockRenderLayerMap.putBlock(block, layer)` — Fabric API. If the signature differs,
   it may be `BlockRenderLayerMap.put(layer, block)` or `.INSTANCE.putBlock(...)`.
   Type `BlockRenderLayerMap.` + Ctrl+space to see the available method.

Both are isolated name swaps. Send me any error and I'll map it.

## After it works
Looking straight down: the top is solid BY DESIGN (matches Forge original).
The transparent cutout area lets you see the fluid through the top hole / inner cavity,
and from an angle you'll see the green experience inside — exactly like the original.
