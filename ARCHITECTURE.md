# Advanced Shooter - System Architecture

## Overview

This document outlines the complete architecture for the Advanced Shooter game in GDevelop, including data structures, event organization, and system interactions.

## Data Structure Design

### Player Object Variables

```
Player
├── Position (X, Y)
├── Velocity (VelocityX, VelocityY)
├── Angle (rotation toward mouse)
├── Health (current health)
├── MaxHealth (100)
├── Stamina (0-100)
├── IsSprinting (boolean)
├── IsCrouching (boolean)
├── IsReloading (boolean)
├── IsAiming (boolean)
├── Score (total points)
├── CurrentWeapon (weapon type string)
├── Ammo (array of ammo per weapon)
│   ├── Pistol: 120
│   ├── MachineGun: 200
│   ├── Shotgun: 30
│   └── Sniper: 12
└── Inventory (picked items)
```

### Enemy Object Variables

```
Enemy
├── Position (X, Y)
├── Velocity (VelocityX, VelocityY)
├── Type (Regular, Elite)
├── Health (current)
├── MaxHealth (25-80 based on type)
├── Speed (120-200)
├── Damage (10-15)
├── AttackCooldown (1.5-2.0 seconds)
├── DetectionRange (300 pixels)
├── AttackRange (150 pixels)
├── State (Idle, Chase, Attack, Seeking Cover)
├── CanSeePlayer (boolean)
├── IsBehindCover (boolean)
├── DamageResistance (1.0 or 0.7)
├── PathIndex (current waypoint in path)
└── LastAttackTime (timer)
```

### Weapon Data Structure

```
WeaponData (Global Variable)
├── Pistol
│   ├── Damage: 20
│   ├── FireRate: 0.1s
│   ├── ReloadTime: 1.5s
│   ├── MaxAmmo: 120
│   ├── Spread: 0
│   ├── Recoil: 2
│   ├── Pellets: 1
│   └── SoundEffect: "pistol_shot.wav"
├── MachineGun
│   ├── Damage: 15
│   ├── FireRate: 0.05s
│   ├── ReloadTime: 2.0s
│   ├── MaxAmmo: 200
│   ├── Spread: 5
│   ├── Recoil: 4
│   ├── Pellets: 1
│   └── SoundEffect: "mg_shot.wav"
├── Shotgun
│   ├── Damage: 50
│   ├── FireRate: 0.8s
│   ├── ReloadTime: 2.5s
│   ├── MaxAmmo: 30
│   ├── Spread: 15
│   ├── Recoil: 8
│   ├── Pellets: 5
│   └── SoundEffect: "shotgun_shot.wav"
└── Sniper
    ├─��� Damage: 80
    ├── FireRate: 1.5s
    ├── ReloadTime: 3.0s
    ├── MaxAmmo: 12
    ├── Spread: 1
    ├── Recoil: 3
    ├── Pellets: 1
    └── SoundEffect: "sniper_shot.wav"
```

### Scene Global Variables

```
Scene Variables
├── CurrentWave: 1
├── EnemiesRemaining: 0
├── TotalEnemiesThisWave: 5
├── WaveStartTime: timestamp
├── GameTime: elapsed time
├── PauseMenuActive: boolean
├── ScreenShakeIntensity: 0-20
├── GameState (Playing, Paused, GameOver)
└── DifficultyMultiplier: 1.0 + (CurrentWave * 0.1)
```

## Event Group Organization

### Layer 1: Core Systems

```
EVENT SHEETS
├── MainScene.gdevelop
│   ├── [GROUP] Initialization
│   ├── [GROUP] PlayerController
│   │   ├── Movement
│   │   ├── Sprint
│   │   ├── Crouch
│   │   └── Camera
│   ├── [GROUP] WeaponSystem
│   │   ├── Switching
│   │   ├── Shooting
│   │   ├── Reload
│   │   └── Grenades
│   ├── [GROUP] EnemyAI
│   │   ├── Spawning
│   │   ├── Pathfinding
│   │   ├── Combat
│   │   ├── Cover
│   │   └── Death
│   ├── [GROUP] WaveSystem
│   │   ├── WaveManager
│   │   └── DifficultyScaling
│   ├── [GROUP] Destructibles
│   │   ├── Crates
│   │   └── ExplosiveBarrels
│   ├── [GROUP] UISystem
│   │   ├── HUD
│   │   ├── PauseMenu
│   │   └── DamageNumbers
│   └── [GROUP] Polish
│       ├── MuzzleFlash
│       ├── ScreenShake
│       ├── Audio
│       └── ParticleEffects
```

### Layer 2: External Layouts

```
EXTERNAL LAYOUTS
├── UILayout.gdevelop
│   ├── HealthBar
│   ├── AmmoCounter
│   ├── ScoreDisplay
│   ├── WaveCounter
│   └── WeaponDisplay
├── EnemyLayout.gdevelop
│   ├── Regular Enemy prefab
│   └── Elite Enemy prefab
├── PickupLayout.gdevelop
│   ├── HealthPack
│   ├── AmmoBox
│   └── ScoreMultiplier
└── EffectsLayout.gdevelop
    ├── MuzzleFlash
    ├── ExplosionParticles
    ├── DamageNumber
    └── ScreenShake
```

## System Interactions

### Player → Weapon → Bullet → Enemy

```
Player Input (Click)
    ↓
WeaponSystem.Shooting
    ├─ Create Bullet
    ├─ Subtract Ammo
    ├─ Play Sound
    ├─ Create MuzzleFlash
    └─ Screen Shake
         ↓
Bullet Movement
    ├─ Move toward target
    └─ Check collisions
         ↓
Bullet → Enemy Collision
    ├─ Subtract Health
    ├─ Create DamageNumber
    ├─ Flash sprite
    └─ Remove Bullet
         ↓
EnemyAI.Death (if Health <= 0)
    ├─ Play death animation
    ├─ Award points
    ├─ Drop loot
    └─ Remove enemy
```

### Enemy → AI → Player → Damage

```
Enemy Spawn
    ↓
EnemyAI.Pathfinding
    ├─ Calculate path to Player
    └─ Move along path
         ↓
EnemyAI.Combat
    ├─ Check LOS (Line of Sight)
    └─ Attack if in range
         ↓
EnemyBullet Created
    ├─ Move toward Player
    └─ Check collisions
         ↓
Bullet → Player Collision
    ├─ Subtract Health
    ├─ Create DamageNumber
    ├─ Flash sprite
    └─ Remove bullet
         ↓
PlayerHealth Check
    └─ If Health <= 0 → GameOver
```

### Wave Progression

```
Wave Start
    ↓
Calculate Difficulty
    ├─ EnemyCount = 5 + (Wave * 1.5)
    ├─ EnemyHealth = 25 + (Wave * 2)
    ├─ EnemySpeed = 120 + (Wave * 5)
    └─ DifficultyMultiplier = 1.0 + (Wave * 0.1)
         ↓
Spawn Enemies
    └─ EnemyCount × Create Enemy
         ↓
Combat Phase
    ├─ Player vs Enemies
    └─ Track EnemiesRemaining
         ↓
All Enemies Defeated?
    ├─ Yes → Wave Complete (go to Wave Start)
    └─ No → Continue
```

## Collision Matrix

```
Object A    | Object B      | Result
------------|---------------|---------------------
PlayerBullet| Enemy         | Enemy takes damage
PlayerBullet| Crate         | Crate takes damage
PlayerBullet| Barrel        | Barrel explodes
PlayerBullet| Wall          | Bullet removed
EnemyBullet | Player        | Player takes damage
EnemyBullet | Obstacle      | Bullet removed
Grenade     | Ground        | Bounces (physics)
Grenade     | Enemy/Player  | Explodes on contact
Enemy       | Player        | Melee damage
Enemy       | Obstacle      | Pathfinding avoidance
HealthPack  | Player        | Restore health
AmmoBox     | Player        | Restore ammo
```

## Timer Management

```
TIMERS USED
├── FireRateTimer (per weapon)
├── ReloadTimer
├── AttackCooldownTimer (per enemy)
├── PatrolTimer (per enemy)
├── MuzzleFlashTimer
├── DamageNumberTimer (per damage number)
├── ScreenShakeTimer
├── WaveStartBannerTimer
���── PathUpdateTimer
├── HealthRegenTimer
└── GrenadeTimer (per enemy)
```

## Object Hierarchy

```
SCENE OBJECTS
├── Player (main character)
│   ├── PlayerSprite
│   ├── PlayerHitbox
│   ├── WeaponSocket (attach point)
│   └── CameraController
├── Enemies[] (array of enemies)
│   ├── EnemySprite
│   ├── EnemyHitbox
│   ├── HealthBar (above head)
│   └── DetectionArea (radius)
├── Bullets[] (projectiles)
│   ├── PlayerBullets[]
│   └── EnemyBullets[]
├── Obstacles[] (environment)
│   ├── Walls[]
│   ├── Crates[]
│   └── ExplosiveBarrels[]
├── Pickups[] (items)
│   ├── HealthPacks[]
│   ├── AmmoBoxes[]
│   └── ScoreMultipliers[]
├── Effects[] (visual feedback)
│   ├── MuzzleFlashes[]
│   ├── ExplosionParticles[]
│   ├── DamageNumbers[]
│   └── BloodSplats[]
└── UI[] (interface)
    ├── HealthBar
    ├── AmmoCounter
    ├── ScoreDisplay
    ├── WaveCounter
    ├── PauseMenu
    └── DebuggingLayer
```

## Performance Optimization Strategies

### Object Pooling
```
Instead of Create/Delete:
├── Pre-spawn object pools at scene start
├── Disable/Enable objects as needed
├── Reuse pooled objects for bullets, particles
└── Reduces lag spikes from object creation
```

### Culling
```
Off-screen culling:
├── Check if object outside camera bounds
├── Disable physics and rendering if off-screen
├── Re-enable when entering bounds
└── Significant performance gain
```

### LOD (Level of Detail)
```
Based on distance from player:
├── Far enemies: Simple AI, reduced particle effects
├── Medium enemies: Full AI, standard effects
└── Close enemies: Full detail, all features
```

## State Diagrams

### Player State Machine
```
[Idle] ←→ [Moving] ←→ [Sprinting]
  ↓        ↓          ↓
[Crouch] [Aiming] [Reloading]
  ↓        ↓          ↓
[Dead]←────┴──────────┴
```

### Enemy State Machine
```
[Idle] → [Chase] → [Attack]
  ↑        ↓         ↓
  └─ [Seeking Cover] ←┘
         ↓
       [Dead]
```

### Game State Machine
```
[Menu] → [Playing] → [Paused]
           ↓          ↓
        [GameOver]←──┘
```

---

**This architecture supports**:
- ✅ Easy system addition and modification
- ✅ Efficient performance management
- ✅ Clear separation of concerns
- ✅ Scalable to hundreds of entities
- ✅ Extensible for future features
