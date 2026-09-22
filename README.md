# 🧬 The Last Light Metabolism

**The Last Light Metabolism** is a survival metabolism system for **Arma Reforger**, originally developed for **The Last Light** survival scenario.

The addon introduces Food and Water survival mechanics while integrating directly with Arma Reforger's existing **stamina, health, blood, damage, and medical systems** rather than replacing them with duplicate systems.

It is designed for multiplayer survival scenarios and provides configurable values that scenario makers can adjust without modifying the core runtime logic.

---

## 📋 Features

- 🍏 Food system
- 💧 Water system
- ⚡ Energy system integrated with vanilla stamina
- 🩸 Live vanilla Blood percentage
- ✚ Live vanilla Health percentage
- 🏃 Additional Water consumption while sprinting
- 🍖 Food and drink consumable integration
- ☠️ Starvation damage
- 💀 Dehydration damage and Energy loss
- 🔄 Multiplayer replication
- 🖥️ Dedicated-server support
- 🎨 Compact survival HUD
- ⚙️ Configurable survival balance

---

# 🍏 Food

Food ranges from:

```text
0 - 100
```

Food decreases automatically over time.

The default configuration is designed to take approximately **1 hour** to decrease from 100 Food to 0 Food without consuming any food.

Food can be restored by compatible consumable items.

## Starvation

When Food reaches:

```text
0
```

the player begins taking starvation damage.

Default starvation penalty:

```text
-3 Health every 5 seconds
```

The starvation penalty stops immediately when Food is restored above 0.

---

# 💧 Water

Water ranges from:

```text
0 - 100
```

Water decreases automatically over time.

The default passive Water drain is designed to take approximately **1 hour** to decrease from 100 Water to 0 Water.

However, physical activity can increase Water consumption.

---

## 🏃 Sprint Hydration

Sprinting consumes additional Water based on the amount of actual vanilla stamina used.

By default:

```text
10 Energy consumed while sprinting
=
1 Water consumed
```

Partial Energy consumption carries between sprints.

Example:

```text
First Sprint
6 Energy consumed

Second Sprint
7 Energy consumed

Total = 13 Energy

Result:
-1 Water

3 Energy remains toward the next Water cost
```

Only **actual Energy consumed while sprinting** contributes toward this cost.

Energy gained through recovery or consumables does not count as sprint Energy consumption.

---

# 💀 Dehydration

When Water reaches:

```text
0
```

the player begins suffering dehydration penalties.

Default dehydration penalties:

```text
-1 Energy every second
-2 Health every 5 seconds
```

These penalties stop when Water is restored above 0.

---

# ☠️ Starvation + Dehydration

Food and Water penalties are independent and can occur simultaneously.

If both Food and Water are at 0:

```text
Starvation:
-3 Health / 5 seconds

Dehydration:
-2 Health / 5 seconds

Combined:
-5 Health / 5 seconds
```

Dehydration also continues removing:

```text
-1 Energy / second
```

---

# ⚡ Energy

TLL Metabolism does **not** create a separate stamina system.

Energy is the player's **actual vanilla Arma Reforger stamina**, displayed as Energy by the TLL survival HUD.

```text
Vanilla Stamina
      ↓
  TLL Energy
```

This keeps TLL Metabolism compatible with the character's real movement and stamina behaviour.

---

## ⚡ Energy Behaviour

Sprinting consumes vanilla stamina normally.

TLL adds survival-specific recovery behaviour around the vanilla system.

Current default behaviour:

```text
Sprint
↓
Vanilla stamina decreases
↓
TLL Energy decreases
↓
Recovery delay resets to 5 seconds
```

After sprinting stops:

```text
5 second recovery delay
↓
Energy begins recovering
↓
+1 Energy per second
```

---

## 🚶 Jogging

Normal jogging does not consume additional TLL Energy.

While jogging:

```text
Energy does not drain
Energy does not recover
Recovery delay is paused
```

This means players cannot regenerate Energy simply by repeatedly tapping movement.

---

## 🥤 Energy From Consumables

Compatible drinks can restore Energy.

Energy restoration is applied directly to the player's **actual vanilla stamina**.

For example:

```text
Energy Amount = 20

Player Energy:
45 → 65
```

Consumable Energy restoration is immediate and does not need to wait for the normal recovery delay.

---

# 🩸 Blood

TLL Metabolism does **not** create a custom Blood system.

Blood is read directly from Arma Reforger's existing character damage system.

The HUD displays the player's actual Blood state as a percentage:

```text
🩸 100%
```

This allows the existing Reforger medical and damage systems to continue controlling Blood.

TLL only displays the result.

---

# ✚ Health

Health is also read directly from Arma Reforger's character damage system.

The HUD displays the player's actual Health percentage:

```text
✚ 100%
```

Starvation and dehydration damage therefore affects **real character Health** rather than a fake survival-only value.

---

# 🎨 Survival HUD

TLL Metabolism includes a compact survival HUD.

Example:

```text
🍏 100
💧 100
⚡ 100
🩸 100%
✚  100%
```

The indicators represent:

| Icon | Value |
|---|---|
| 🍏 | Food |
| 💧 | Water |
| ⚡ | Energy / Vanilla Stamina |
| 🩸 | Blood |
| ✚ | Health |

Food, Water and Energy are updated from the replicated TLL Metabolism state.

Blood and Health are read from the character's vanilla damage system.

---

# ⚙️ Default Balance

The default balance is intended for survival scenarios where food and water are reasonably obtainable.

## Passive Food Drain

```text
Metabolism Update Interval = 10 seconds
Food Drain = 0.277
```

Approximate depletion time:

```text
100 Food → 0 Food
≈ 1 hour
```

---

## Passive Water Drain

```text
Metabolism Update Interval = 10 seconds
Water Drain = 0.277
```

Approximate passive depletion time:

```text
100 Water → 0 Water
≈ 1 hour
```

Sprint Water consumption is **additional** to this passive drain.

---

## Sprint Water Consumption

Default:

```text
Sprint Energy Per Water = 10
Sprint Water Cost = 1
```

Meaning:

```text
Every 10 actual Energy consumed while sprinting
↓
-1 Water
```

---

# 🍖 Consumable System

TLL Metabolism integrates with consumables provided through the **TLL Loot System**.

Compatible consumables can restore:

```text
Food
Water
Energy
```

Each compatible item uses:

```text
TLL_ConsumableComponent
```

Individual items can define their own survival values.

---

## Example Food

```text
Consumable Type = EAT

Food Amount = 25
Water Amount = 0
Energy Amount = 0
```

Result:

```text
Food:
50 → 75
```

---

## Example Drink

```text
Consumable Type = DRINK

Food Amount = 0
Water Amount = 30
Energy Amount = 15
```

Result:

```text
Water:
50 → 80

Energy:
40 → 55
```

Energy restoration affects actual vanilla stamina.

---

# 🔧 Scenario Maker Configuration

TLL Metabolism was designed so scenario makers can adjust the important survival balance without rewriting the core system.

The main gameplay values are exposed through:

```text
TLL_MetabolismComponent
```

---

# ✅ Safe Values To Change

Scenario makers can safely adjust:

```text
Metabolism Update Interval

Food Drain

Water Drain

Sprint Energy Per Water

Sprint Water Cost
```

Consumable prefabs can also safely change:

```text
Consumable Type

Food Amount

Water Amount

Energy Amount

Use Duration
```

These values are intended for scenario balancing.

---

# ⏱️ Metabolism Update Interval

Controls how frequently the normal passive metabolism update occurs.

Default:

```text
10 seconds
```

### Important

Food Drain and Water Drain are applied per metabolism update.

Therefore:

```text
Changing Update Interval
WITHOUT
changing Food/Water Drain
```

will also change the total depletion time.

For example, reducing the update interval while keeping the same drain values will cause Food and Water to decrease faster.

---

# 🍏 Food Drain

Default:

```text
0.277
```

Controls how much Food is removed during each normal metabolism update.

Increase it for faster Food depletion.

Decrease it for slower Food depletion.

---

# 💧 Water Drain

Default:

```text
0.277
```

Controls passive Water consumption.

This does **not** include the additional Water consumed through sprinting.

---

# 🏃 Sprint Energy Per Water

Default:

```text
10
```

Controls how much actual sprint Energy must be consumed before the sprint hydration cost is applied.

Example:

```text
10
```

means:

```text
Every 10 Energy spent sprinting
→ Water cost
```

A higher threshold makes sprinting consume Water less frequently.

---

# 💧 Sprint Water Cost

Default:

```text
1
```

Controls how much Water is removed whenever the sprint Energy threshold is reached.

Default behaviour:

```text
10 Energy
↓
-1 Water
```

---

# 🍖 Configuring Consumables

Individual consumables can be balanced through:

```text
TLL_ConsumableComponent
```

Scenario makers can configure:

```text
Consumable Type
Food Amount
Water Amount
Energy Amount
Use Duration
```

This allows different scenarios to use different survival balance without changing the metabolism scripts.

---

## Example: Easier Survival Server

```text
Water Bottle

Water Amount = 40
```

---

## Example: Hardcore Survival Server

```text
Water Bottle

Water Amount = 15
```

Both configurations work with the same metabolism system.

---

# 🛠️ What Scenario Makers CAN Change

The following are intended to be customised:

- Food drain rate
- Water drain rate
- Metabolism update interval
- Sprint Energy threshold
- Sprint Water cost
- Consumable Food restoration
- Consumable Water restoration
- Consumable Energy restoration
- Consumable use duration
- HUD position
- HUD icon size
- HUD textures
- HUD font size
- HUD spacing
- HUD visual styling

These changes should not require modification of the core metabolism logic.

---

# ⚠️ What Scenario Makers SHOULD NOT Change

Unless you are intentionally creating and maintaining your own fork, avoid modifying the core runtime systems responsible for:

- Server metabolism activation
- Player lifecycle handling
- Multiplayer replication
- Consumable event routing
- Vanilla stamina integration
- Sprint stamina tracking
- Energy recovery
- Dehydration Energy drain
- Starvation damage
- Dehydration Health damage
- Vanilla Health integration
- Vanilla Blood integration
- HUD runtime updates

These systems interact with each other.

Changing one without accounting for the others may cause incorrect behaviour or client/server desynchronisation.

---

# ⚠️ Do Not Separate Energy From Vanilla Stamina

Energy is intentionally tied directly to vanilla stamina.

The intended relationship is:

```text
Player Sprint
     ↓
Vanilla Stamina
     ↓
TLL Energy
     ↓
Sprint Hydration
     ↓
Water Consumption
```

Consumables also interact with this system:

```text
Drink
  ↓
Energy Amount
  ↓
Vanilla Stamina
  ↓
TLL Energy HUD
```

Creating a second independent Energy value would break this relationship.

---

# 🎨 HUD Customisation

The HUD layout is located at:

```text
UI/Layouts/HUD/TLL_Metabolism.layout
```

Scenario makers can reposition or restyle the HUD without changing the metabolism mechanics.

---

## Required Widget Names

The HUD runtime expects the following `TextWidget` names:

```text
FoodValue
WaterValue
EnergyValue
BloodValue
HealthValue
```

### Do not rename these widgets

Unless you also modify:

```text
TLL_MetabolismDisplay.c
```

The display script searches for these names when the HUD starts.

---

## Icon Widgets

The icon widgets can be repositioned, resized or have their textures replaced without affecting metabolism calculations.

Current structure:

```text
MetabolismPanel
│
├── FoodRow
│   ├── FoodValue
│   └── FoodIcon
│
├── WaterRow
│   ├── WaterValue
│   └── WaterIcon
│
├── EnergyRow
│   ├── EnergyValue
│   └── EnergyIcon
│
├── BloodRow
│   ├── BloodValue
│   └── BloodIcon
│
└── HealthRow
    ├── HealthValue
    └── HealthIcon
```

This makes it safe to customise the visual appearance while leaving the underlying survival system untouched.

---

# 🌐 Multiplayer

TLL Metabolism is designed for multiplayer survival scenarios.

Food and Water are handled by the authoritative server and replicated to players.

The local HUD displays the replicated metabolism state for the controlled character.

Blood and Health are read directly from the character's vanilla damage system.

The system has been tested in dedicated-server gameplay.

---

# 🔄 Player Lifecycle

The metabolism system activates when a player controls their character.

It is designed to handle:

```text
Player joins
Player character becomes controlled
Metabolism activates
Values replicate
HUD displays local state
```

Character changes and respawns are detected by the HUD so the display can reconnect to the currently controlled character.

---

# 🧩 System Architecture

Simplified metabolism flow:

```text
                 TLL CONSUMABLE
                       │
                       ▼
             TLL_ConsumableComponent
                       │
                       ▼
             TLL_MetabolismComponent
                 │       │       │
                 │       │       │
               FOOD    WATER   ENERGY
                                   │
                                   ▼
                           VANILLA STAMINA
                                   │
                                   ▼
                                SPRINT
                                   │
                                   ▼
                        SPRINT WATER COST
```

Vanilla character systems remain responsible for Blood and Health:

```text
          ARMA REFORGER DAMAGE SYSTEM
                   │
             ┌─────┴─────┐
             │           │
             ▼           ▼
           BLOOD       HEALTH
             │           │
             └─────┬─────┘
                   ▼
             TLL HUD DISPLAY
```

---

# 🧩 Dependencies

TLL Metabolism currently requires:

```text
Arma Reforger
TLL Loot System
```

The **TLL Loot System** provides the consumable integration used by compatible food and drink items.

---

# 📦 Recommended Scenario Integration

For scenario makers, the recommended approach is:

1. Add **TLL Metabolism** as a dependency.
2. Add/use the required metabolism player-character setup.
3. Configure Food and Water drain rates.
4. Configure sprint hydration values.
5. Configure individual consumable values.
6. Position or restyle the HUD if desired.
7. Test the final configuration in multiplayer.
8. Test on a dedicated server before production deployment.

Avoid copying individual scripts out of the addon unless you intend to maintain a separate fork.

---

# 🔒 Core Systems

Once integrated, the following systems should generally be treated as core:

```text
TLL_MetabolismComponent
TLL_CharacterStaminaComponent
TLL_MetabolismDisplay
Player lifecycle integration
Consumable event integration
```

Scenario makers should prefer adjusting exposed configuration values instead of modifying these systems directly.

---

# 🎮 Designed For Survival Scenarios

TLL Metabolism is intended for scenarios involving:

- Persistent survival gameplay
- PvE survival
- PvPvE environments
- Loot progression
- Traders
- Food and drink scarcity
- Exploration
- Persistent characters
- DayZ-style survival concepts
- Custom survival scenarios

The default configuration is designed to provide meaningful survival management without requiring constant eating and drinking.

---

# 🌄 The Last Light

TLL Metabolism was originally created for **The Last Light**, an Arma Reforger survival environment focused on:

```text
Survival
Looting
Exploration
PvE
Optional PvP
Traders
Persistent progression
Custom survival systems
```

The addon was built to provide a reusable survival foundation while continuing to use Arma Reforger's existing character systems wherever possible.

---

# 📌 Version

## v1.0.0

Initial finished release of **The Last Light Metabolism**.

Includes:

- Food system
- Water system
- Vanilla stamina Energy integration
- Energy recovery system
- Sprint Water consumption
- Consumable Food restoration
- Consumable Water restoration
- Consumable Energy restoration
- Starvation penalties
- Dehydration penalties
- Vanilla Blood display
- Vanilla Health display
- Icon-based survival HUD
- Multiplayer replication
- Dedicated-server support
- Scenario-maker configurable balance

---

# Credits

**The Last Light Metabolism**

Created for **The Last Light** Arma Reforger survival project.

Please respect the original work when modifying, redistributing, or building upon the addon.
