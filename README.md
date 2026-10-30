# Statues Classic — Forge 1.20.1 port

**Forge port:** Mister_leocorx  
**Original mod (Fabric):** Talsumi — https://github.com/Talmori/StatuesClassic  
**Original concept:** AUTOMATIC_MAIDEN's Statues mod

Port of [Talmori/StatuesClassic](https://github.com/Talmori/StatuesClassic) (Fabric 1.18.2) to **Minecraft Forge 1.20.1**.

## Build
Requires JDK 17.

    ./gradlew build        # jar ends up in build/libs/
    ./gradlew runClient    # dev client

Dependencies: Forge 47.3.0+, [Kotlin for Forge](https://www.curseforge.com/minecraft/mc-mods/kotlin-for-forge) 4.x.
MarderLib and Fabric API are **not** needed any more.

## What changed compared to the Fabric version
* Yarn mappings -> official Mojang mappings; Fabric API -> Forge events / `DeferredRegister`.
* MarderLib is gone. Its parts used by the mod were replaced:
  * registration -> `DeferredRegister` (`ModBlocks`, `ModItems`, `ModBlockEntities`, `ModMenus`, `ModCreativeTabs`)
  * custom BlockEntity sync packet -> vanilla `getUpdateTag` / `getUpdatePacket`; inventory -> Forge `ItemStackHandler`
  * `EnhancedScreenHandler` -> `StatueCreationMenu` / `StatueEquipmentMenu`
  * `EnhancedScreen` / `BaseWidget` -> `client/content/screen/EnhancedScreen`, `client/content/widgets/BaseWidget`
* Networking: Forge `SimpleChannel` (`networking/ModNetwork`).
* Rendering: `PlayerRenderer` layers, `GuiGraphics`, JOML/`Axis` instead of `Vec3f` quaternions.
* Statue block-state is stored as an NBT block state, so **statues from 1.18.2 worlds are not migrated**.
* Recipes use Forge tags (`forge:dyes/*`, `forge:ingots/iron`, `forge:cobblestone`).
* The unused custom entity shader was dropped.

## Small behaviour fixes made while porting
* Statue drops are cleared after dropping (no duplicate drops).
* Skins are fetched with one request instead of three; missing block textures fall back to the plain skin.
* Client-supplied floats/joystick values are sanitised on the server.
* Render bounding box covers the whole 2-block statue.
