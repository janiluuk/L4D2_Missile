# L4D & L4D2 Missiles Galore

A SourceMod plugin that lets survivors fire dumbfire and homing missiles from their firearms in coop/survival (disabled in Versus/Scavenge). The script is defined in `l4d2_missile.sp` and compiles to `missile_l4d.smx`.

## Requirements
- Left 4 Dead or Left 4 Dead 2 server.
- Metamod:Source and SourceMod 1.10+ with the bundled `sdkhooks` extension.

## Gameplay
- Survivors start each round with 1 missile. Earn more by killing infected: 1 per 30 common, 3 per special infected, 5 if the special infected death was a headshot. Inventory is capped by `l4d2_missile_limit` (default 3).
- Fire with any allowed weapon by holding **Use** and shooting. Crouch + **Use** + shoot to launch a homing missile. Default allowed weapons: rifle, sniper, shotgun, magnum, SMG, pistol, grenade launcher (each gated by `l4d2_missile_weapon_*` cvars).
- Homing missiles lock onto visible enemies within `l4d2_missile_radar_range` (1500.0). Keep line of sight to maintain the lock. Targets get a hint + lock-on beep, their team is notified, and infected see a short ring highlight around the shooter. Homing missiles can also lock onto enemy missiles to intercept them.
- Counter-fire is optional: common infected can attempt anti-missile shots when survivors launch (`l4d2_missile_infected_anti`), and specials can fire on key actions (spit, charge start, drag, witch startled, tank rock throw).
- Press `!m` (`sm_m`, `sm_missilehelp`, or `sm_missiles`) in chat/console for the in-game help panel.

## Installation
1. Copy `l4d2_missile.sp` into your server’s `addons/sourcemod/scripting` folder.
2. Compile with the matching SourceMod compiler: `./spcomp l4d2_missile.sp -o missile_l4d.smx`.
3. Place `l4d2_missile.smx` in `addons/sourcemod/plugins` and changelevel or restart. Reload live with `sm plugins reload missile_l4d`.
4. A config file `cfg/sourcemod/l4d2_missile.cfg` is auto-generated on first run; defaults are summarized below.

## Configuration (cfg/sourcemod/l4d2_missile.cfg)
- Damage: `l4d2_missile_radius` 200.0, `l4d2_missile_damage` 500.0, `l4d2_missile_damage_tosurvivor` 0.0, `l4d2_missile_push` 1200.
- Safety: `l4d2_missile_safe` 1 shrinks the blast radius near survivors to reduce friendly-fire.
- Inventory: `l4d2_missile_limit` 3, `l4d2_missile_kills` 30 (kills needed per missile).
- Homing: `l4d2_missile_tracefactor` 1.5 (turn rate), `l4d2_missile_radar_range` 1500.0 (lock distance).
- Weapon toggles: `l4d2_missile_weapon_rifle`, `..._sniper`, `..._shotgun`, `..._magnum`, `..._smg`, `..._pistol`, `..._grenade` (1/0).
- Infected missile chances (percent): `l4d2_missile_infected_smoker`, `..._charger`, `..._spitter`, `..._witch`, `..._tank_throw` fire on their key ability; `l4d2_missile_infected_anti` is the common infected counter-fire chance when survivors launch.

## Credits
Authored by Yaniho; see `l4d2_missile.sp` for full metadata.
