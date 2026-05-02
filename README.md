# Walpurgis & Faithless: Trilogy Compatibility Patch

This mod acts as a compatibility patch to allow **Faithless: Trilogy** (a high-quality Heretic expansion) and **Walpurgis** to be played together without breaking either mod's weapon systems.

## Version History

### 1.1.1
- Added remapping for items found in Treasure Chests and Strongboxes. These items now correctly use Walpurgis spawners instead of vanilla Heretic or Faithless-specific items.

## What It Does

Both Walpurgis (the Crusader's Slot 2 weapon) and Faithless: Trilogy feature a weapon named `Lightbringer`. Without this patch, GZDoom crashes due to a class name collision because two `Lightbringer` items are loaded into the engine's memory natively via ZScript and DECORATE.

This patch manually resolves the conflict by isolating the Crusader's weapon code, renaming it entirely, and writing a local override into the Walpurgis loot pool (`BlueWeapon`) specifically for those drops.

**IMPORTANT:** Because it replaces the weapon's code completely, **this patch will exclusively work with `+Walpurgis-JT-1.0-A.pk3`.** Using it on an older or newer version of Walpurgis may cause you to miss updates to the weapon or cause unforeseen errors.

**Compatibility Note regarding Walpurgis_Findus_Patch:**
While `Walpurgis_Findus_Patch.pk3` is listed as optional, this compatibility mod binds the weapons to their specific slots (Slot 3) based on the `KEYCONF` settings used in the Findus Patch. Since this mod replaces the original `Lightbringer` with `WalLightbringer`, if you choose to skip the Findus Patch, you **must manually edit the `KEYCONF` file** within this mod to assign the `WalLightbringer` to your preferred weapon slots. Otherwise, the weapon will not be selectable via the number keys.

## Load Order

To ensure the engine overwrites the spawners correctly and resolves the conflict, load your files in the following exact sequence:

1. `faithless.pk3` (Faithless: Trilogy)
2. `+Walpurgis-JT-1.0-A.pk3`
3. `Walpurgis_Findus_Patch.pk3` *(Optional)*
4. `Walpurgis_Findus_Faith_Tri.pk3` *(This compatibility mod)*

*Note: Other mods can be loaded in order depending on what you want to achieve (animations, texture packs, map packs, etc.), provided they don't overwrite these specific Walpurgis or Faithless: Trilogy weapon systems again.*

## Related Projects

- **Findus Patch:** [GitHub Repo](https://github.com/Predictabowl/Walpurgis_Findus_Patch_repo)
- **No Shield Frag:** [GitHub Repo](https://github.com/Predictabowl/Walpurgis_Findus_NoShieldFrag_repo)
- **Faithless Trilogy Compatibility:** [GitHub Repo](https://github.com/Predictabowl/Walpurgis_Findus_Faith_Tri_repo)
