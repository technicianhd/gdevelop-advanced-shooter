# Advanced Shooter - Game Balance Parameters

All numerical values for game balancing and tuning. Adjust these to fine-tune difficulty and gameplay feel.

## Player Statistics

```
Health System:
├─ Max Health: 100 HP
├─ Health Regen: +1 HP every 5 seconds (optional)
├─ Critical Health Threshold: 25 HP (20% of max)
└─ Damage Flash Duration: 0.2 seconds

Movement:
├─ Walk Speed: 200 pixels/sec
├─ Sprint Speed: 350 pixels/sec (1.75x multiplier)
├─ Crouch Speed: 120 pixels/sec
├─ Acceleration: 0 (instant response)
└─ Deceleration (Friction): 0.85 multiplier per frame

Stamina:
├─ Max Stamina: 100
├─ Sprint Drain: 45 per second
├─ Regen Rate: 30 per second
├─ Min Stamina to Sprint: 0 (can always sprint when not depleted)
└─ Regen Delay After Sprint: 0 seconds (immediate)

Aiming:
├─ Aim Zoom Level: 0.7x (30% zoom in)
├─ Aim Zoom Smoothness: Lerp factor 0.1
├─ Scope Offset: Player.AimOffset × 0.3
└─ Movement While Aiming: Normal speed (no slow)
```

## Weapon Balance

### Pistol
```
Damage: 20 per bullet
Fire Rate: 0.1 seconds (10 bullets/sec)
Reload Time: 1.5 seconds
Magazine Size: 120 bullets
Spread (Inaccuracy): 0 degrees
Recoil: 2 pixels
Bullet Speed: 600 pixels/sec
Ammo Pickup: 30 rounds
Cost Multiplier: 1.0x
```

### Machine Gun
```
Damage: 15 per bullet
Fire Rate: 0.05 seconds (20 bullets/sec)
Reload Time: 2.0 seconds
Magazine Size: 200 bullets
Spread (Inaccuracy): 5 degrees
Recoil: 4 pixels
Bullet Speed: 550 pixels/sec
Ammo Pickup: 50 rounds
Cost Multiplier: 1.0x
```

### Shotgun
```
Damage: 50 per pellet
Fire Rate: 0.8 seconds (1.25 shots/sec)
Reload Time: 2.5 seconds
Magazine Size: 30 shells (30 shots)
Pellets Per Shot: 5
Spread Per Pellet: 15 degrees
Recoil: 8 pixels
Bullet Speed: 600 pixels/sec
Ammo Pickup: 6 shells
Cost Multiplier: 1.0x
Effective Range: 150 pixels
```

### Sniper Rifle
```
Damage: 80 per bullet
Fire Rate: 1.5 seconds (0.67 shots/sec)
Reload Time: 3.0 seconds
Magazine Size: 12 bullets
Spread (Inaccuracy): 1 degree
Recoil: 3 pixels
Bullet Speed: 800 pixels/sec (fastest)
Ammo Pickup: 3 bullets
Cost Multiplier: 1.0x
Effective Range: Unlimited (no damage falloff)
```

### Grenade
```
Damage: 40 per grenade
Explosion Radius: 150 pixels
Fuse Time: 3.0 seconds (before auto-detonation)
Detonation on Impact: Yes
Max Grenades Held: 10
Carry Speed: 350 pixels/sec
Throw Speed: 300 pixels/sec
Physics Gravity: 300 pixels/sec² (falling)
Physics Bounce: 0.6 bounce factor
Restock Rate: 1 grenade every 30 seconds (optional)
```

## Enemy Statistics

### Regular Enemy
```
Health: 25 HP
Max Health: 25 HP
Speed: 120 pixels/sec
Damage: 10 HP (per attack)
Attack Cooldown: 2.0 seconds
Detection Range: 300 pixels
Attack Range: 150 pixels
Accuracy: Random spread 2-8 degrees
Points on Kill: 100 points
Loot Drop Chance: 30%
  ├─ 40% Ammo Box
  ├─ 50% Health Pack
  └─ 10% Score Bonus (1.5x for 10 sec)
```

### Elite Enemy
```
Health: 50 HP
Max Health: 50 HP
Speed: 150 pixels/sec
Damage: 15 HP (per attack)
Attack Cooldown: 1.5 seconds
Detection Range: 350 pixels
Attack Range: 150 pixels
Accuracy: Random spread 2-6 degrees
Grenade Throw: 5% chance per frame, cooldown 8 seconds
Grenade Damage: 40 HP
Cover Seeking: At 40% health, seek nearest cover
Points on Kill: 500 points
Loot Drop Chance: 50% (better drops)
  ├─ 30% Ammo Box (larger amount)
  ├─ 50% Health Pack (more health)
  └─ 20% Score Bonus (2.0x for 15 sec)
```

## Wave Progression

### Base Wave Formula
```
Base Enemy Count: 5 + (CurrentWave × 1.5)
  Example:
  Wave 1: 5 enemies
  Wave 5: 12 enemies
  Wave 10: 20 enemies
  Wave 20: 35 enemies

Health Scaling: 25 + (CurrentWave × 2)
Speed Scaling: 120 + (CurrentWave × 5)
Damage Scaling: 10 + (CurrentWave × 1)
```

### Difficulty Tiers
```
Wave 1-4: Introduction
├─ Regular enemies only
├─ Simple AI (no grenades)
└─ Difficulty: Easy

Wave 5-9: Intermediate
├─ 30% Elite enemies introduced
├─ Grenades enabled
└─ Difficulty: Medium

Wave 10-14: Advanced
├─ 50% Elite enemies
├─ Increased grenade frequency
└─ Difficulty: Hard

Wave 15+: Extreme
├─ 70% Elite enemies
├─ Very frequent grenades
└─ Difficulty: Insane
```

### Wave Bonuses
```
On Wave Complete:
├─ Base Bonus: 1000 points
├─ Time Bonus: 100 × (300 - TimeToComplete)
├─ Health Bonus: 50 × CurrentHealth
└─ Difficulty Bonus: 500 × DifficultyMultiplier
Total: Range 1000-5000 points per wave
```

## Destructible Balance

### Crate
```
Health: 30 HP
Bullet Damage Taken: 100%
Explosion Damage Taken: 100%
Loot Drop Chance: 50%
  ├─ Ammo Box (30 rounds)
  └─ Health Pack (25 HP)
Visual States:
├─ Intact (0-100% health)
├─ Damaged (50-100% health)
└─ Destroyed (0% health)
Break Time: Instant
Sound Effect: "crate_break.wav"
```

### Explosive Barrel
```
Health: 50 HP
Bullet Damage Taken: 100%
Explosion Damage: 60 HP
Explosion Radius: 200 pixels
Explosion Delay: Instant (on hit)
Loot Drop Chance: 0% (too dangerous)
Visual States:
├─ Normal (0-100% health)
├─ Leaking (< 50% health)
└─ Exploded (destroyed)
Knockback Force: 300 pixels/sec
Break Time: 0.2 seconds
Sound Effect: "explosion_large.wav"
Screen Shake: Intensity 10
Cooldown Between Explosions: N/A (one-time)
```

## Pickup/Loot Balance

### Health Pack
```
Health Restored: 25 HP
Pickup Range: 30 pixels
Duration on Ground: 30 seconds (auto-disappear)
Spawn Rate: 30% from regular enemies, 50% from elite
Weight: Light (no effect on pickups)
Sound Effect: "pickup.wav"
Visual Effect: Rotating animation, glow
```

### Ammo Box
```
Ammo Restored: 30 rounds (weapon-specific)
Pickup Range: 30 pixels
Duration on Ground: 30 seconds (auto-disappear)
Spawn Rate: 40% from regular enemies, 30% from elite
Distribution: Random weapon (60% current, 40% other)
Weight: Light
Sound Effect: "pickup.wav"
Visual Effect: Rotating animation, glow
```

### Score Multiplier
```
Multiplier: 1.5x or 2.0x (elite only)
Duration: 10 seconds (regular), 15 seconds (elite)
Pickup Range: 40 pixels
Visual Effect: Bright glow, pulsing
Sound Effect: "bonus_pickup.wav"
Stack Limit: Up to 2x multipliers (max 3x or 4x)
```

## Difficulty Multiplier

```
Scales with waves:
DifficultyMultiplier = 1.0 + (CurrentWave × 0.1)

Application:
├─ Enemy Health × DifficultyMultiplier
├─ Enemy Speed × DifficultyMultiplier
├─ Enemy Damage × DifficultyMultiplier
├─ Score Rewards × DifficultyMultiplier
└─ Wave Completion Bonus × DifficultyMultiplier

Examples:
Wave 1: 1.1x
Wave 5: 1.5x
Wave 10: 2.0x
Wave 20: 3.0x
```

## Scoring System

```
Base Points:
├─ Regular Enemy Kill: 100 points
├─ Elite Enemy Kill: 500 points
├─ Crate Destroyed: 25 points
├─ Wave Completion: 1000 points
└─ Headshot Bonus: 1.5x multiplier (if implemented)

Wave Completion Bonus:
├─ Speed Bonus: 100 × (300 - time in seconds)
├─ Health Bonus: 50 × remaining health
└─ Difficulty Bonus: 500 × DifficultyMultiplier

Total Points Example (Wave 5):
├─ 12 enemies × 100 = 1200 points
├─ 2 elites × 500 = 1000 points
├─ 5 crates × 25 = 125 points
├─ Wave completion = 1000 points
├─ Speed bonus = 2000 points
├─ Health bonus = 2500 points
├─ Difficulty bonus = 2500 points
└─ Total: ~10,325 points
```

## Visual Effects Balance

```
Muzzle Flash:
├─ Duration: 0.08 seconds
├─ Scale: 0.8-1.2x (variation)
└─ Rotation: Random 0-360 degrees

Screen Shake:
├─ Pistol: Intensity 1
├─ Machine Gun: Intensity 2
├─ Shotgun: Intensity 4
├─ Sniper: Intensity 1.5
├─ Explosion: Intensity 10
└─ Decay: -100 × TimeDelta()

Damage Number:
├─ Duration: 1.5 seconds
├─ Rise Speed: 50 pixels/sec
├─ Fade: Linear (1 to 0 opacity)
├─ Font Size: 24pt
├─ Colors:
│   ├─ Player damage: Red
│   ├─ Enemy damage: Yellow
│   └─ Healing: Green
└─ Stack Offset: +10 pixels between numbers
```

## Performance Tuning

```
Object Limits:
├─ Max Enemies: 100 (soft limit)
├─ Max Bullets: 500 (soft limit)
├─ Max Particles: 1000 (soft limit)
└─ Max UI Elements: 50

Spawn Rate Limits:
├─ Bullets/sec: 60 max (pistol auto would exceed)
├─ Enemies/sec: 10 max
└─ Particles/sec: 100 max

Culling Settings:
├─ Cull off-screen objects: Yes
├─ Cull radius: 100 pixels beyond screen
├─ Physics sleep: Yes (inactive objects)
└─ Rendering LOD: Yes (distance-based)
```

## Testing Checklist

```
Balance Testing:
☐ Pistol feels balanced compared to others
☐ Shotgun is powerful but limited ammo
☐ Sniper requires skill but high reward
☐ Machine Gun is versatile
☐ Regular enemies are manageable
☐ Elite enemies are challenging
☐ Difficulty scaling feels right
☐ Wave progression is smooth
☐ Rewards feel satisfying
☐ Game doesn't feel impossible past wave 20
☐ Health packs drop at good rate
☐ Ammo pickups prevent ammo starvation
☐ Player feels powerful but not overpowered
☐ Enemies feel threatening but beatable
```

---

**Adjust these values based on playtest feedback to fine-tune your game!**
