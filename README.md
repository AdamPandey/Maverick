# Maverick

An F-16 flight and combat game made in Unreal Engine 5.6, built almost entirely in Blueprints. You fly a jet over a snowy landscape, shoot at enemy aircraft, avoid SAM sites and drop flares. The project folder is called F16Control.

This repo is the Unreal project only. There is no C++ source, no packaged build and no screenshots in it.

## Requirements

- Unreal Engine 5.6 (`EngineAssociation` in `Maverick.uproject`)
- A desktop GPU. The project is set to the Desktop hardware class with graphics performance on Maximum.
- A lot of disk space. `Content/` is about 1.6 GB and the `.git` folder is about 1.2 GB.

The project enables these plugins: Water, WaterExtras, MetaHuman, MetaHumanLiveLink, MetaHumanCharacter, MetaHumanCoreTech and MetaHumanCalibrationProcessing. No MetaHuman content is tracked, so those plugins may just be left over.

## Opening it

1. Clone the repo. Make sure the clone is complete, the asset files are large.
2. Right click `Maverick.uproject`, switch the engine version to 5.6 if it asks, then open it. There is nothing to compile because the project has no `Source/` folder.
3. The first open will compile a lot of shaders and take a while.

`Config/DefaultEngine.ini` sets both the editor startup map and the game default map to `/Game/F16Control/levels/LV_Snow`, and the game mode to `BP_GM` (`/Game/F16Control/blueprint/BP_GM`).

One problem: only `LV_Snow_BuiltData.uasset` is tracked under `Content/F16Control/levels/`. The `LV_Snow.umap` file itself isn't in the repo, so a fresh clone has no main level to open. Until that file is added you'll have to build a test level yourself, or open one of the example maps from the asset packs listed below.

## Controls

These come from the action and axis mappings in `Config/DefaultInput.ini`.

| Input | Action |
| --- | --- |
| W / S | Pitch |
| A / D | Roll |
| Q / E | Yaw |
| Space / Left Shift | Speed up / slow down |
| Left mouse | Shoot bullets |
| Right mouse | Shoot rocket |
| F | Look (also on gamepad right trigger) |
| G | Open wheels (landing gear) |
| Esc, F1 or T | Pause |

On gamepad, the left stick and d-pad handle pitch and yaw, left trigger fires a rocket and right trigger is the look action.

These are the older-style input mappings. The project also uses Enhanced Input (it is set as the default player input class), and `Content/F16Control/blueprint/` has `IMC_JetControls` plus the actions `IA_DeployFlares`, `IA_FireWeapon` and `IA_LookBack`. Those are binary assets, so the keys for flares and look-back aren't listed here. Check `IMC_JetControls` in the editor for the real bindings.

## What's in the game

This list comes from asset names, since the Blueprints are binary and weren't opened in the editor:

- Player jet: `BP_Pilot` with a skeletal mesh (`sk_Jet`) and a set of animations for pitch, roll, yaw, afterburner, wings and landing gear. Niagara effects for afterburner and vapor.
- Weapons: bullets (`BP_Bullet`), unguided rockets (`BP_rocket`) and a homing missile (`BP_PlayerHomingMissile`), with a lock-on reticle widget (`WBP_LockOnReticle`).
- Enemy aircraft: `BP_Enemy` driven by an AI controller and a behavior tree called `BT_Dogfight`, with a blackboard and a `BTS_FlightComputer` service.
- Ground threats: `BP_SAM_Site` and `BP_SAM_Missile`, plus `BP_WarningTrigger`. Flares (`BP_FlarePellet`, `NS_AngelFlares`, flare sound cues) look like the counter.
- Rings and a timer (`BP_Ring`, `M_Ring`, `WBP_Timer`). The project's collision channels include `TimeAttackRing`, `SAM_Detection`, `Targetable`, `Projectile` and `Flare`.
- UI: `UMG_menu`, `WBP_HUD_Messages`, `WBP_LockOnReticle`, `WBP_Timer`. A level sequence called `LS_Victory` is in `sequence/`.
- Audio: engine, machine gun, explosion and missile sounds, and a MetaSound called `MS_DynamicSoundtrack`.
- Environment: a snow landscape built with the `MF_RB_Land_*` material functions and `M_Landscape`, a missile launcher tower mesh, and aircraft carrier models.

How much of this is wired up and working hasn't been checked.

## Project structure

```
Maverick.uproject
Config/                        DefaultEngine, DefaultGame, DefaultInput, DefaultEditor
Content/
  F16Control/                  the actual game
    AI/                        enemy AI controller, behavior tree, blackboard
    Carrier/                   four aircraft carrier imports (carrier, carrier1, Ford, Nimitz)
    Cues/, Wave/               sound cues, MetaSound, wave files
    Niagara/                   afterburner, flares, vapor
    animation/                 jet animations
    blueprint/                 game mode, pilot, enemy, weapons, SAM, input, UI widgets
    levels/                    only the built data for LV_Snow
    materials/, textures/      jet, landscape, projectile materials and textures
    meshes/                    jet, rocket, bomb, torus, missile launcher tower
    sequence/                  LS_Victory
  M5VFXVOL2/                   VFX pack: Niagara systems, particles, fire Blueprints, 6 demo maps
  MWLandscapeAutoMaterial/     landscape auto-material pack: materials, textures, 2 example maps
Git_Reference.md               see below
```

`M5VFXVOL2` and `MWLandscapeAutoMaterial` look like imported asset packs going by the naming and the demo maps in them. Where they came from and what their licenses say hasn't been checked. The three of them together are about 1,300 tracked files; 774 of those are in `F16Control` and 655 of those are the carrier models.

## Repo notes

- There is no `.gitignore`. `Saved/`, `Intermediate/` and `DerivedDataCache/` aren't tracked right now, but nothing stops them being added.
- The last commit is called "Setup Git LFS for large assets", but there is no `.gitattributes` and `git lfs ls-files` lists nothing, so the `.uasset` and `.umap` files are stored as normal Git blobs. That's why the history is over a gigabyte. Individual textures are up to roughly 50 MB.
- The whole project was added in that single last commit (2025-12-11). The 49 commits before it only add one line each to `Git_Reference.md`, with messages like "V1.4.0: Synchronized Missile Homing Physics ...". They don't correspond to real changes in the project files, so don't use the log to work out what was built when.
- `Git_Reference.md` is just a list of those version lines (V0.0.0 to V2.0.4, dated 16 Nov to 11 Dec 2025). It has no instructions in it.
- Two audio assets are named after commercial songs ("Danger Zone" and a "Main Titles" track, in `Cues/` and `sequence/`). There's no licensing info in the repo for them.
- Config also has a packaging setting of `PPBC_Shipping`, but there is no packaged output or packaging instructions in the repo.

## Known gaps

- `LV_Snow.umap` is missing, see above.
- No gameplay documentation beyond the key table, and no screenshots or video.
- Unused plugins (MetaHuman) are still enabled in the uproject.
- Git LFS is not actually set up.

## Author

AdamPandey
