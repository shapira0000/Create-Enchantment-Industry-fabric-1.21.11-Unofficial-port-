# Module 1: Liquid Experience (נוזל הניסיון)

This is the first module — the fluid base that everything else builds on.

## Files in this module
- `EnchantmentIndustry.java` — main mod initializer
- `registry/CeiFluids.java` — registers Liquid Experience + Hyper Experience
- `content/fluids/experience/ExperienceFluid.java` — the fluid + XP-orb conversion logic
- `content/fluids/experience/HyperExperienceFluid.java` — hyper variant with effects

## How to test
1. Drop these files into the project.
2. Run `gradlew build` (or let IntelliJ build).
3. Send me ALL compile errors you get.

## Names I translated from Yarn → Mojang that may need fixing

Because the project uses Mojang mappings and I can't compile here, these specific
method/field names are my best translation. If the build complains about any of them,
copy the error and I'll correct it. Most likely candidates:

| Used in code | What it does | If it errors, likely real name |
|---|---|---|
| `ExperienceOrb.getExperienceValue(int)` | round amount to orb size | `getExperienceValue` is the classic Mojang name — should be right |
| `ExperienceOrb.tryMergeToExisting(ServerLevel, Vec3, int)` | merge into nearby orb | could be `tryMergeToExisting` (verify) |
| `ExperienceOrb.repairPlayerItems(ServerPlayer, int)` | apply XP to Mending gear | NOTE: signature changed to ServerPlayer in 1.21.x |
| `MobEffects.NIGHT_VISION` | night vision effect | should be right |
| `MobEffects.MOVEMENT_SPEED` | speed effect | **might be `SPEED`** in 1.21.11 |

## What this module does NOT include yet (coming next)
- Fluid textures / rendering (still + flow) — the fluid will be invisible/untextured until added
- Bucket item + item-fluid storage (filling bottles, buckets)
- The custom HyperExperienceOrb entity (currently uses a scaled normal orb)
- Tags, language entries

These come in later modules. For now we only verify the fluid REGISTERS and the
project COMPILES against Create Fly. Once this builds clean, we move to the next piece.
