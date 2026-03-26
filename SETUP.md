# Advanced Shooter - Development Setup Guide

Complete step-by-step guide to set up your development environment and start building the Advanced Shooter game in GDevelop.

## Prerequisites

### System Requirements
- **OS**: Windows, macOS, or Linux
- **RAM**: 4GB minimum (8GB recommended)
- **Storage**: 2GB available space
- **Browser**: Modern browser (Chrome, Firefox, Edge)
- **Internet**: For GDevelop Cloud features (optional)

## Step 1: Install GDevelop

### Option A: Online Editor (Recommended for Beginners)
1. Go to https://gdevelop.io
2. Click "Start Creating"
3. Sign up with email or GitHub account
4. Create new project

### Option B: Desktop Application
1. Download from https://gdevelop.io/download
2. Choose your OS (Windows, macOS, Linux)
3. Run installer and follow prompts
4. Launch GDevelop desktop app

### Option C: Self-Hosted (Advanced)
1. Clone from https://github.com/4ian/GDevelop
2. Follow local installation guide in repository

## Step 2: Create New Project

1. **Launch GDevelop**
2. **Click "Create a New Project"**
3. **Choose Template**:
   - Select "Blank 2D Game" for top-down shooter
   - Or "Blank 3D" for first-person shooter
4. **Configure Settings**:
   - Game Resolution: 1280 × 720
   - Physics Engine: 2D Rapier (or 3D if needed)
   - Color Mode: RGB

## Step 3: Set Up Project Structure

### Create Scenes
In the Project Manager, create these scenes:

```
Scenes/
├── MainMenu
├── GameplayScene (main shooter gameplay)
├── GameOver
└── PauseMenu (optional, can be overlay)
```

### Create External Layouts (for modular design)

```
External Layouts/
├── UILayout (all HUD elements)
├── EnemyLayout (enemy prefabs)
├── PickupLayout (item drops)
└── EffectsLayout (visual effects)
```

### Folder Organization

```
Resources/
├── Sprites/
│   ├── Player/
│   │   ├── idle.png
│   │   ├── walking.png
│   │   ├── running.png
│   │   ├── crouching.png
│   │   └── dead.png
│   ├── Enemies/
│   │   ├── regular_idle.png
│   │   ├── regular_walking.png
│   │   ├── regular_attack.png
│   │   └── elite_*.png
│   ├── Weapons/
│   │   ├── pistol.png
│   │   ├── machinegun.png
│   │   ├── shotgun.png
│   │   └── sniper.png
│   ├── Bullets/
│   │   ├── bullet_pistol.png
│   │   ├── bullet_shotgun.png
│   │   ├── grenade.png
│   │   └── muzzle_flash.png
│   ├── Environment/
│   │   ├── crate.png
│   │   ├── barrel_explosive.png
│   │   ├── wall_segment.png
│   │   └── floor.png
│   ├── Items/
│   │   ├── health_pack.png
│   │   ├── ammo_box.png
│   │   └── score_bonus.png
│   ├── UI/
│   │   ├── healthbar_bg.png
│   │   ├── healthbar_fill.png
│   │   ├── ammo_icon.png
│   │   └── pause_menu_bg.png
│   └── Effects/
│       ├── explosion_spritesheet.png
│       ├── blood_splat.png
│       └── particle_flame.png
├── Audio/
│   ├── SFX/
│   │   ├── weapon_fire/
│   │   │   ├── pistol_shot.wav
│   │   │   ├── mg_shot.wav
│   │   │   ├── shotgun_shot.wav
│   │   │   └── sniper_shot.wav
│   │   ├── weapon_reload/
│   │   │   ├── reload_pistol.wav
│   │   │   └── reload_shotgun.wav
│   │   ├── enemy_sounds/
│   │   │   ├── enemy_death.wav
│   │   │   ├── enemy_attack.wav
│   │   │   └── grenade_throw.wav
│   │   ├── impact_sounds/
│   │   │   ├── bullet_impact.wav
│   │   │   ├── explosion.wav
│   │   │   └── crate_break.wav
│   │   └── ui_sounds/
│   │       ├── pickup.wav
│   │       ├── wave_start.wav
│   │       └── ui_click.wav
│   └── Music/
│       └── background_music.ogg
└── Data/
    └── game_balance.json (optional)
```

## Step 4: Set Up Global Variables

Create these in **Scene Variables** or **Project Variables**:

### Scene Variables (Gameplay Scene)

```
Player:
- Health (Number): 100
- MaxHealth (Number): 100
- Stamina (Number): 100
- Score (Number): 0
- CurrentWeapon (Text): "Pistol"
- IsReloading (Boolean): false
- IsSprinting (Boolean): false
- IsCrouching (Boolean): false
- IsAiming (Boolean): false

Game:
- CurrentWave (Number): 1
- EnemiesRemaining (Number): 0
- GameState (Text): "Playing"
- ScreenShakeIntensity (Number): 0
- DifficultyMultiplier (Number): 1.0

Ammo (structure):
- Pistol (Number): 120
- MachineGun (Number): 200
- Shotgun (Number): 30
- Sniper (Number): 12
```

## Step 5: Install Recommended Extensions

In GDevelop, go to **Extensions → Search and Install**:

1. **Pathfinding Behavior** (for enemy AI)
2. **Physics Engine 2D** (for grenades and knockback)
3. **Health Bar** (for enemy health display)
4. **Top-Down Movement** (optional, alternative to manual movement)

### How to Install:
1. Click Extensions in toolbar
2. Search for extension name
3. Click "Install"
4. Grant permissions if prompted
5. Restart GDevelop

## Step 6: Create Player Object

1. **Create New Object** → "Sprite"
2. Name it: "Player"
3. **Add sprite animation frames**:
   - Idle (repeat if only 1 frame)
   - Walking (8 frames)
   - Running (8 frames)
   - Crouching (4 frames)
4. **Set default animation**: Idle
5. **Add Behaviors**:
   - Physics (if using 2D physics)
   - Custom movement (manual or Top-Down)

## Step 7: Create Enemy Object

1. **Create New Object** → "Sprite"
2. Name it: "Enemy"
3. **Add sprite animation frames**:
   - Idle
   - Walking
   - Attack
   - Death
4. **Add Behaviors**:
   - Pathfinding (for navigation)
   - Health (for damage tracking)
5. **Create Variables**:
   - Type (Regular/Elite)
   - Health
   - MaxHealth
   - Speed
   - AttackCooldown

## Step 8: Create Bullet Objects

### Player Bullet
1. **Create New Object** → "Sprite"
2. Name: "PlayerBullet"
3. Use small circle or bullet sprite
4. **Add Properties**:
   - Damage (Number)
   - Owner (Text): "Player"

### Enemy Bullet
1. Duplicate PlayerBullet
2. Name: "EnemyBullet"
3. Set Owner to "Enemy"

### Grenade
1. **Create New Object** → "Sprite"
2. Name: "Grenade"
3. Use round sprite
4. **Add Behaviors**:
   - Physics (bounces and falls)
5. **Add Properties**:
   - Damage
   - ExplosionRadius
   - ExplodeTimer

## Step 9: Create Environmental Objects

### Crate
1. **Create New Object** → "Sprite"
2. Name: "Crate"
3. Add sprite frames: normal, damaged, destroyed
4. **Add Variables**:
   - Health (Number): 30
   - MaxHealth (Number): 30

### Explosive Barrel
1. **Create New Object** → "Sprite"
2. Name: "ExplosiveBarrel"
3. Use distinctive sprite (red barrel)
4. **Add Variables**:
   - Health (Number): 50
   - Damage (Number): 60
   - ExplosionRadius (Number): 200

### Wall/Obstacle
1. **Create New Object** → "Tiled Sprite" or "Sprite"
2. Name: "Wall"
3. Use wall texture
4. Use for collision blocking only

## Step 10: Create UI Objects

### Health Bar
1. **Create Panel object** for background
2. **Create shape/rectangle** for fill bar
3. Add text for health number

### Ammo Counter
1. **Create Text object**
2. Name: "AmmoText"
3. Set default text: "120 / 120"

### Score Display
1. **Create Text object**
2. Name: "ScoreText"
3. Set default text: "Score: 0"

### Wave Counter
1. **Create Text object**
2. Name: "WaveText"
3. Set default text: "Wave 1"

## Step 11: Set Up Event Sheets

### Main Event Sheet Structure

```
EVENT SHEET: GameplayScene Events

[GROUP] Initialization
├─ On scene load
│  ├─ Create player at center
│  ├─ Initialize variables
│  └─ Set up UI

[GROUP] Player Controller
├─ Movement input
├─ Sprint logic
├─ Crouch logic
└─ Camera follow

[GROUP] Weapon System
├─ Weapon switching
├─ Firing logic
├─ Reload mechanics
└─ Grenade throwing

[GROUP] Enemy AI
├─ Spawn enemies
├─ Pathfinding
├─ Combat logic
└─ Death handling

[GROUP] Wave System
├─ Wave progression
├─ Difficulty scaling
└─ Enemy spawning

[GROUP] Collision & Damage
├─ Bullet collisions
├─ Damage application
└─ Object destruction

[GROUP] UI Updates
├─ Health bar
├─ Ammo counter
├─ Score display
└─ Wave display

[GROUP] Effects & Audio
├─ Sound playback
├─ Particle effects
├─ Screen shake
└─ Visual feedback
```

## Step 12: Testing Setup

### Enable Debug Mode
1. Go to **File → Project Properties**
2. Check "Enable debug drawing"
3. Check "Show FPS"

### Test Levels
Create these for incremental testing:

```
TestScenes/
├── TestPlayer (just player movement)
├── TestWeapons (player + weapons only)
├── TestEnemy (one enemy, no waves)
└── TestFull (complete game)
```

## Step 13: Performance Monitoring

### Enable Profiler
1. **File → Preferences**
2. **Debug → Enable profiler**
3. Shows performance metrics while playing

### Monitor These:
- FPS (target: 60)
- Object count (optimize if > 500)
- Memory usage

## Step 14: Asset Management Tips

### Use Asset Packs (for storage)
1. Go to **Asset store → My packs**
2. Create custom asset pack
3. Upload your own sprites and sounds

### Free Resources
- Sprites: itch.io, Kenney.nl
- Sounds: Freesound.org, FreeAudio
- Music: YouTube Audio Library

## Quick Development Workflow

```
1. Create basic scene layout
2. Implement player movement
3. Test and iterate
4. Add one system at a time
5. Playtest after each addition
6. Adjust balance parameters
7. Polish and optimize
8. Export and publish
```

## Exporting Your Game

### Export to Web
1. **File → Export**
2. Choose **Electronjs (Desktop)** or **Web Export**
3. Configure output folder
4. Click Export
5. Publish to itch.io or GitHub

### Export to Mobile
1. **File → Export**
2. Choose **Android** or **iOS**
3. Follow platform-specific setup
4. Upload to app stores

## Troubleshooting

### Issue: FPS Drop
- **Solution**: Reduce enemy count, enable culling, use object pooling

### Issue: Lag on Firing
- **Solution**: Pre-create bullet pool, reduce particle count

### Issue: Path Not Found
- **Solution**: Check pathfinding grid size, add obstacles to tilemap

### Issue: Game Crashes
- **Solution**: Check debug console, reduce simultaneous objects, clear unused events

## Next Steps

1. ✅ Follow **EVENT_LOGIC.md** for implementation
2. ✅ Reference **BALANCE.md** for game settings
3. ✅ Consult **ARCHITECTURE.md** for system design
4. ✅ Playtest frequently and iterate

---

**You're now ready to build your advanced shooter!** 🎮
