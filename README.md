# Wojaks_Nightmare_MobileSetuo
Production Architeofor Wojaks Nightnares for Mobile.
Wojaks_Nightmares/
│
├── project.godot
├── export_presets.cfg
├── README.md
├── CHANGELOG.md
├── LICENSE
├── .gitignore
├── .gitattributes
│
├── config/
│   ├── game_config.cfg
│   ├── difficulty.cfg
│   ├── weapons.json
│   ├── enemies.json
│   └── bosses.json
│
├── assets/
│
│── sprites/
│   ├── characters/
│   │   ├── player/
│   │   ├── enemies/
│   │   └── bosses/
│   │
│   ├── environment/
│   │   ├── gothic/
│   │   ├── acid_void/
│   │   └── cathedral/
│   │
│   ├── ui/
│   └── icons/
│
├── audio/
│   ├── music/
│   ├── ambience/
│   └── sfx/
│
├── shaders/
│   ├── acid_effect.gdshader
│   ├── gothic_light.gdshader
│   ├── damage_flash.gdshader
│   └── post_process.gdshader
│
├── scenes/
│
│── core/
│   ├── Game.tscn
│   ├── World.tscn
│   └── LoadingScreen.tscn
│
│── player/
│   ├── Player.tscn
│   ├── Projectile.tscn
│   └── Spell.tscn
│
│── enemies/
│   ├── base/
│   │   └── EnemyBase.tscn
│   │
│   ├── MadChadler/
│   │   ├── MadChadler.tscn
│   │   ├── Gunner.tscn
│   │   ├── Heavy.tscn
│   │   └── Elite.tscn
│
│── bosses/
│   ├── REKT/
│   │   ├── REKT.tscn
│   │   ├── attacks/
│   │   └── phases/
│   │
│   ├── RUGPULL/
│   └── Legion/
│
│── levels/
│   ├── Level1.tscn
│   ├── GothicCathedral.tscn
│   ├── AcidVoid.tscn
│   └── FinalCathedral.tscn
│
│── ui/
│   ├── MainMenu.tscn
│   ├── HUD.tscn
│   ├── FaithMeter.tscn
│   ├── BossBar.tscn
│   ├── Settings.tscn
│   └── PauseMenu.tscn
│
├── scripts/
│
│── core/
│   ├── GameManager.gd
│   ├── SceneManager.gd
│   ├── EventBus.gd
│   └── CrashReporter.gd
│
│── player/
│   ├── Player.gd
│   ├── Movement.gd
│   ├── Combat.gd
│   └── SpellSystem.gd
│
│── enemies/
│   ├── EnemyBase.gd
│   ├── AIController.gd
│   ├── Pathfinding.gd
│   └── LootDrop.gd
│
│── bosses/
│   ├── BossBase.gd
│   ├── PhaseManager.gd
│   └── AttackController.gd
│
│── systems/
│
│   ├── SaveSystem.gd
│   ├── AudioManager.gd
│   ├── InputManager.gd
│   ├── ObjectPool.gd
│   ├── DifficultyManager.gd
│   ├── PerformanceManager.gd
│   ├── Analytics.gd
│   └── Localization.gd
│
│── mobile/
│   ├── TouchController.gd
│   ├── VirtualJoystick.gd
│   └── MobileOptimizer.gd
│
│── weapons/
│   ├── WeaponBase.gd
│   ├── ProjectileWeapon.gd
│   └── SpellWeapon.gd
│
├── autoloads/
│   ├── GameManager.gd
│   ├── SaveSystem.gd
│   ├── AudioManager.gd
│   ├── ObjectPool.gd
│   └── SettingsManager.gd
│
├── tests/
│   ├── player_tests.gd
│   ├── combat_tests.gd
│   └── save_tests.gd
│
├── tools/
│   ├── asset_optimizer/
│   └── build_scripts/
│
├── builds/
│   ├── android/
│   ├── ios/
│   ├── windows/
│   └── linux/
│
└── .github/
    └── workflows/
        └── build.yml
        extends Node

signal game_started
signal game_paused


var current_level
var difficulty = 1


func start_game():
    get_tree().paused = false
    game_started.emit()


func pause_game():
    get_tree().paused = true
    game_paused.emit()
    extends Node


var pools = {}


func create_pool(name, scene, amount):

    pools[name] = []

    for i in amount:
        var obj = scene.instantiate()
        obj.hide()
        add_child(obj)
        pools[name].append(obj)



func get_object(name):

    for obj in pools[name]:

        if !obj.visible:
            obj.show()
            return obj

    return null
    extends Node


func apply_settings():

    if OS.has_feature("mobile"):

        ProjectSettings.set_setting(
        "rendering/renderer/rendering_method",
        "gl_compatibility"
        )

        Engine.max_fps = 60


    else:

        Engine.max_fps = 144
        extends Node


const SAVE_VERSION = 1

var data = {}


func save():

    data.version = SAVE_VERSION

    var file = FileAccess.open(
    "user://save.json",
    FileAccess.WRITE
    )

    file.store_string(
    JSON.stringify(data)
    )


func load():

    if FileAccess.file_exists(
    "user://save.json"
    ):

        var file = FileAccess.open(
        "user://save.json",
        FileAccess.READ
        )

        data = JSON.parse_string(
        file.get_as_text()
        )
        {
 "MadChadler":
 {
   "health":100,
   "damage":15,
   "speed":90,
   "drops":"faith"
 },

 "Elite":
 {
   "health":500,
   "damage":50,
   "speed":60
 }
}
[rendering]

renderer/rendering_method="gl_compatibility"
renderer/rendering_method.mobile="gl_compatibility"

textures/default_filters/use_nearest_mipmap_filter=true

[application]

run/main_scene="res://scenes/core/Game.tscn"


[display]

window/size/viewport_width=1920
window/size/viewport_height=1080

window/stretch/mode="canvas_items"


[performance]

max_fps=60
✓ Android build
✓ iOS build
✓ Windows build
✓ Controller support
✓ Touch controls
✓ Save migration
✓ Crash logging
✓ FPS limiter
✓ Texture compression
✓ Shader fallback
✓ Offline mode
✓ Accessibility settings
✓ Localization ready
✓ Unit tests
✓ Boss AI testing
✓ Memory leak testing
✓ Load testing
core/
systems/
autoloads/
player/
enemies/
bosses/
weapons/
levels/
assets/
shaders/
audio/
ui/
tests/
tools/
builds/
github/
Full prosuction for IOS.
Wojaks_Nightmares_iOS/
│
├── project.godot
├── export_presets.cfg
│
├── ios/
│   ├── WojaksNightmares.xcodeproj
│   ├── Info.plist
│   ├── Assets.xcassets
│   └── Config/
│       ├── Release.xcconfig
│       └── Debug.xcconfig
│
├── scenes/
│   ├── core/
│   │   ├── Game.tscn
│   │   └── LoadingScreen.tscn
│   │
│   ├── player/
│   │   └── Player.tscn
│   │
│   ├── enemies/
│   │   ├── MadChadler.tscn
│   │   ├── Gunner.tscn
│   │   ├── Heavy.tscn
│   │   └── Elite.tscn
│   │
│   ├── bosses/
│   │   ├── REKT.tscn
│   │   ├── RUGPULL.tscn
│   │   └── Legion.tscn
│   │
│   └── ui/
│       ├── HUD.tscn
│       ├── TouchControls.tscn
│       └── Settings.tscn
│
├── scripts/
│
│   ├── core/
│   │   ├── GameManager.gd
│   │   ├── SceneLoader.gd
│   │   └── CrashLogger.gd
│
│   ├── mobile/
│   │   ├── TouchInput.gd
│   │   ├── HapticFeedback.gd
│   │   └── BatteryOptimizer.gd
│
│   ├── systems/
│   │   ├── SaveSystem.gd
│   │   ├── AudioManager.gd
│   │   ├── Analytics.gd
│   │   └── PerformanceManager.gd
│
├── assets/
│
├── shaders/
│
├── localization/
│   ├── en.csv
│   └── es.csv
│
├── tests/
│
└── builds/
    └── ios/
    [rendering]

renderer/rendering_method="mobile"
renderer/rendering_method.mobile="mobile"

textures/default_filters/use_nearest_mipmap_filter=true

environment/defaults/default_clear_color=Color(0,0,0,1)
extends Node


var movement := Vector2.ZERO
var attack := false


func _process(delta):

    movement = Vector2(
        Input.get_action_strength("move_right")
        -
        Input.get_action_strength("move_left"),

        Input.get_action_strength("move_down")
        -
        Input.get_action_strength("move_up")
    )


func attack_pressed():

    attack = true
    extends Control


func _ready():

    var safe = DisplayServer.get_display_safe_area()

    offset_left = safe.position.x
    offset_top = safe.position.y
    extends Node


func hit_feedback():

    Input.vibrate_handheld(80)
    extends Node


func optimize():

    if OS.get_name()=="iOS":

        Engine.max_fps = 60

        ProjectSettings.set_setting(
        "rendering/limits/rendering_method",
        "mobile"
        )
        Before test flight
        [x] Bundle Identifier
[x] App Icons
[x] Launch Screen
[x] Privacy manifest
[x] Signing certificates
[x] Provisioning profile
[x] Archive build
[x] TestFlight upload
[x] App Store metadata
[x] Review compliance


        
    
    

    
        
    
        
