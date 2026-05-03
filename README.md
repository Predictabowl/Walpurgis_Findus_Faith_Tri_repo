# Walpurgis & Faithless: Trilogy Compatibility Patch

This mod acts as a compatibility patch to allow **Faithless: Trilogy** (a high-quality Heretic expansion) and **Walpurgis** to be played together without breaking either mod's weapon systems.

## Version History

### 1.3.0

- Added Warlock Collision Fix (see "Warlock Collision Fix & Softlock Prevention").

### 1.2.0

- Added a compatibility fix for the Torch artifact (see "Torch Fix & Softlock Prevention").

### 1.1.1

- Added remapping for items found in Treasure Chests and Strongboxes. These items now correctly use Walpurgis spawners instead of vanilla Heretic or Faithless-specific items.

## What It Does

### Weapon Conflict Resolution

Both Walpurgis (the Crusader's Slot 2 weapon) and Faithless: Trilogy feature a weapon named `Lightbringer`. Without this patch, GZDoom crashes due to a class name collision because two `Lightbringer` items are loaded into the engine's memory natively via ZScript and DECORATE.

This patch manually resolves the conflict by isolating the Crusader's weapon code, renaming it entirely, and writing a local override into the Walpurgis loot pool (`BlueWeapon`) specifically for those drops.

### Torch Fix & Softlock Prevention

In **Faithless: Trilogy**, certain maps use ACS scripts that check the player's inventory for a `PowerTorch` to unlock doors or reveal paths. Because Walpurgis replaces the vanilla Torch with its own custom version (`TorchW`) that uses dynamic lighting actors instead of the internal `PowerTorch` powerup, these scripts would fail to trigger, resulting in the player being **softlocked**.

To resolve this, we implemented a hybrid solution:

- Using the Walpurgis Torch now grants the engine's `PowerTorch` item for **12 seconds**.
- This window is sufficient for the map's ACS triggers to detect the item and open relevant doors/sectors.
- An invisible "Limiter" actor then automatically removes the `PowerTorch` from the player's inventory after the delay.
- This prevents the vanilla screen-wide full brightness from overriding Walpurgis's atmospheric lighting for the remainder of the torch's actual duration, ensuring players can progress without ruining the visual experience.

### Warlock Collision Fix & Softlock Prevention

In **Faithless: Trilogy**, specific maps (notably **M2E1**) feature a "Final Fight" sequence where monsters are spawned in tiny 32x32 unit teleport closets. Floors are then lowered by the map script, and monsters are expected to float/walk across a line trigger to enter the arena.

Because the **Warlock of D'Sparil** (`WarlockW`) inherits from the `PainElemental`, it possessed a **Radius of 31**. Using a monster with a diameter of 62 in a 32-unit wide room caused it to become physically wedged in the geometry. Since the map script waits for these monsters to leave or die before proceeding, the map would **softlock**.

To resolve this:

- We redefined `WarlockW_FaithFix` to override the original.
- The **Radius was reduced to 16**, matching the vanilla **Disciple of D'Sparil** (Wizard) hitbox.
- This allows the monster to navigate the tight closets and reliably trigger the teleport linedefs, allowing the map scripts to complete.

**Compatibility Note regarding Walpurgis_Findus_Patch:**
While `Walpurgis_Findus_Patch.pk3` is listed as optional, this compatibility mod binds the weapons to their specific slots (Slot 3) based on the `KEYCONF` settings used in the Findus Patch. Since this mod replaces the original `Lightbringer` with `WalLightbringer`, if you choose to skip the Findus Patch, you **must manually edit the `KEYCONF` file** within this mod to assign the `WalLightbringer` to your preferred weapon slots. Otherwise, the weapon will not be selectable via the number keys.

## Installation & Setup

### Technical Dependency

**IMPORTANT:** Because this patch replaces core weapon code, **it will exclusively work with `+Walpurgis-JT-1.0-A.pk3`.** Using it with an older or newer version of Walpurgis may cause missing weapon updates or game-breaking errors.

### Load Order

To ensure the engine correctly overwrites spawners and resolves asset conflicts, you must load the files in this specific sequence:

1. `faithless.pk3` (Faithless: Trilogy)
2. `+Walpurgis-JT-1.0-A.pk3`
3. `Walpurgis_Findus_Patch.pk3` *(Optional)*
4. `Walpurgis_Findus_Faith_Tri.pk3` *(This compatibility mod)*

*Note: Other mods can be loaded in order depending on what you want to achieve (animations, texture packs, map packs, etc.), provided they don't overwrite these specific Walpurgis or Faithless: Trilogy weapon systems again.*

## Related Projects

- **Findus Patch:** [GitHub Repo](https://github.com/Predictabowl/Walpurgis_Findus_Patch_repo)
- **No Shield Frag:** [GitHub Repo](https://github.com/Predictabowl/Walpurgis_Findus_NoShieldFrag_repo)
- **Faithless Trilogy Compatibility:** [GitHub Repo](https://github.com/Predictabowl/Walpurgis_Findus_Faith_Tri_repo)
