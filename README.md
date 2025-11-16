# L4D2 Missiles

Missiles in Left 4 Dead 2.  
Survivors can fire dumbfire and homing rockets from their guns in **Coop** and **Versus**.  
Special infected (and even commons) can sometimes answer back with their own missiles.

Plugin file: `l4d2_missile.sp` → `l4d2_missile.smx`.

---

## What it does

- Survivors get a small stock of missiles and earn more by killing infected.
- **Fire a dumbfire missile:** hold **Use** and shoot.
- **Fire a homing missile:** crouch + **Use** + shoot.
- Homing missiles:
  - Lock onto visible enemies within a configurable range.
  - Show hint text and a lock-on beep to the target.
  - Optionally **highlight the shooter** to all infected so they can hunt them.
  - Disappear if the shooter dies before impact.
  - Can also lock onto and intercept enemy missiles.
- Missiles can be **shot down**: if you shoot a missile, it explodes early.
- There is a **cooldown** between missile launches (configurable, default 5 seconds).  
  If you try too soon, you get a hint telling you how many seconds are left.
- Optional AI fun:
  - Commons can occasionally fire counter-missiles when survivors launch.
  - Specials can launch missiles on key actions (spit, charge, drag, witch aggro, tank rock, etc.).
- Other plugins can optionally hook in to decide if a **specific player** is allowed to fire missiles at all (e.g. talents/perks system).  
  If nothing hooks it, everyone in the right team/gamemode is allowed by default.

Use `!m` (`sm_m`, `sm_missilehelp`, or `sm_missiles`) in chat/console to open the in-game help panel.

---

## Requirements

- Left 4 Dead 2 server (should also work on L4D1 with compatible entities).
- Metamod:Source
- SourceMod 1.10+ with `sdkhooks` enabled.

---

## Gameplay details

- **Missile stock**
  - Survivors start each round with **1 missile**.
  - Earn missiles by killing infected:
    - Commons: 1 missile per `l4d2_missile_kills` kills (default **30**).
    - Specials: **3** kills’ worth.
    - Special headshot: **5** kills’ worth.
  - Inventory is capped by `l4d2_missile_limit` (default **3**).

- **Weapons that can fire missiles**
  - Controlled by `l4d2_missile_weapon_*` cvars:
    - `l4d2_missile_weapon_rifle`
    - `l4d2_missile_weapon_sniper`
    - `l4d2_missile_weapon_shotgun`
    - `l4d2_missile_weapon_magnum`
    - `l4d2_missile_weapon_smg`
    - `l4d2_missile_weapon_pistol`
    - `l4d2_missile_weapon_grenade`
  - `1` = enabled, `0` = disabled.

- **Homing logic**
  - Lock range: `l4d2_missile_radar_range` (default **1500.0** units).
  - Turn/steer speed: `l4d2_missile_tracefactor` (default **1.5**).
  - Keeps scanning for a valid enemy target in front of the missile.
  - Maintains line-of-sight; if LoS is lost, it will stop tracking.
  - Target gets:
    - Hint text like “Missile locked on you!”.
    - Lock beep sound.
  - Shooter sees distance feedback in hints.
  - Shooter highlight for infected:
    - Controlled by `l4d2_missile_highlight_shooter` (default **1** = enabled).
    - Draws a ring around the shooter so infected players know who to focus.

- **Missile destruction**
  - If the shooter dies while their homing missile is still flying:
    - The missile is automatically destroyed and explodes where it is.
  - If a missile is shot by any gun:
    - It detonates immediately at that point.

- **Cooldown**
  - `l4d2_missile_cooldown` (default **5.0** seconds).
  - If a player tries to fire before the cooldown is over:
    - They get a hint text telling them how many seconds remain until they can shoot again.
    - The missile is not fired.

---

## Game modes

- Designed for **Coop** and **Versus**.
- Plugin checks current gamemode and only enables itself in allowed modes.
- Per-player permission check:
  - There is a small API hook that lets other plugins say “this player can/can’t launch missiles”.
  - If nothing overrides it, everyone in the right team/gamemode is allowed to fire.

---

## Installation

1. Copy `l4d2_missile.sp` into:
   - `addons/sourcemod/scripting`
2. Compile:
   ```bash
   ./spcomp l4d2_missile.sp -o l4d2_missile.smx
