# 🚀 DevRocket

> A 3D physics-based rocket navigation game built with Unity — thrust your rocket through hazardous obstacle courses across multiple themed worlds, from ancient Egyptian desert ruins to futuristic sci-fi underground labs!

[![Unity](https://img.shields.io/badge/Unity-2022.x%20LTS-black?logo=unity&logoColor=white)](https://unity.com/)
[![Language](https://img.shields.io/badge/Language-C%23-239120?logo=csharp&logoColor=white)](https://docs.microsoft.com/en-us/dotnet/csharp/)
[![Input System](https://img.shields.io/badge/Input%20System-New-blue?logo=unity)](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/index.html)
[![Platform](https://img.shields.io/badge/Platform-PC%20%7C%20Windows%20%7C%20Mac-lightgrey?logo=windows)](https://unity.com/features/multiplatform)
[![Cinemachine](https://img.shields.io/badge/Cinemachine-Enabled-purple?logo=unity)](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.9/manual/index.html)
[![License](https://img.shields.io/badge/License-MIT-brightgreen)](LICENSE)
[![GitHub](https://img.shields.io/badge/Repo-DevRocket-181717?logo=github)](https://github.com/Chandan-Baskey/DevRocket)

---

## 📸 Game Screenshots

### 🗺 Full Level Overview
![Game Level Overview](https://github.com/Chandan-Baskey/DevRocket/blob/c8781c68afe501015fad1b2ff7b2c0a1a9772753/GameLevel.png?raw=true)

### 🎮 In-Game Views

<p align="center">
  <img src="Screenshots/gameplay-desert.png" width="48%" alt="Egyptian Desert Level — Gameplay"/>
  &nbsp;
  <img src="Screenshots/editor-egyptian-cave.png" width="48%" alt="Egyptian Underground — Editor View"/>
</p>

<p align="center">
  <img src="Screenshots/editor-scifi-lab.png" width="48%" alt="Sci-Fi Lab — Editor View"/>
  &nbsp;
  <img src="Screenshots/gameplay-pixel-forest.png" width="48%" alt="Pixel Forest Level"/>
</p>

> 📌 **To add screenshots:** Create a `Screenshots/` folder in your repo root, place your PNG images there, commit and push. GitHub will render them automatically in this README.

---

## 📖 Table of Contents

- [About the Game](#-about-the-game)
- [Features](#-features)
- [Gameplay](#-gameplay)
- [Game Worlds & Levels](#-game-worlds--levels)
- [Project Structure](#-project-structure)
- [Scripts Reference](#-scripts-reference)
  - [CollisionHandler.cs](#collisionhandlercs)
  - [Movement.cs](#movementcs-newmonobehaviourscript)
  - [Oscillator.cs](#oscillatorcs)
- [Physics & Movement System](#-physics--movement-system)
- [Audio & VFX System](#-audio--vfx-system)
- [Debug & Dev Tools](#-debug--dev-tools)
- [How to Play](#-how-to-play)
- [Installation & Setup](#-installation--setup)
- [Build Instructions](#-build-instructions)
- [Scene Build Index](#-scene-build-index)
- [Dependencies](#-dependencies)
- [Contributing](#-contributing)
- [Author](#-author)
- [License](#-license)

---

## 🛸 About the Game

**DevRocket** is a 3D physics-based rocket piloting game developed in Unity. The player controls a small rocket/drone and must navigate through carefully hand-crafted obstacle courses without crashing into walls, terrain, or moving hazards. Each level is themed around a completely different visual world — from ancient Egyptian desert ruins glowing with hieroglyphic light, to sterile futuristic underground sci-fi laboratories — all rendered in a clean **low-poly art style** with dramatic directional lighting and atmospheric post-processing.

The game is built on Unity's **new Input System** for rebindable controls, a `Rigidbody`-based physics engine for realistic rocket behavior, **Cinemachine** for smooth dynamic camera follow, and Unity **Particle Systems + AudioSource** for immersive thruster and explosion feedback.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🚀 Physics-based rocket | Rigidbody thrust via `AddRelativeForce` — realistic weight and momentum |
| 🌍 Multiple themed worlds | Egyptian desert, Egyptian underground city, sci-fi cave lab, pixel forest |
| 💥 Crash & finish VFX | Particle systems on both collision outcomes |
| 🔊 Full spatial audio | Thruster loop, crash SFX, level-complete chime — all via AudioSource |
| 🏛 Oscillating obstacles | `Oscillator.cs` drives moving platforms/hazards using PingPong + Lerp |
| 🎥 Cinemachine camera | Smooth follow camera that tracks the rocket dynamically |
| 🔄 Auto scene progression | Finish triggers next scene; last level loops back to level 1 |
| 🛠 Dev/debug hotkeys | `L` skip level, `C` no-clip, `ESC` quit — built in for rapid testing |
| ⌨️ New Input System | Clean `InputAction`-based controls for thrust and rotation |
| 🎨 Low-poly art style | Stylized environments with warm desert and cool sci-fi color palettes |
| 🔁 Auto restart on crash | Level automatically reloads after a configurable `delayTime` |

---

## 🎮 Gameplay

### Objective
Pilot your rocket from its **starting pad** to the **glowing Finish zone** in each level — avoid all terrain, walls, and oscillating obstacles along the way. Each level is a unique spatial puzzle requiring precise thrust timing and rotation control.

### Collision Tag System

| Tag | Response |
|-----|----------|
| `"Finish"` | ✅ Triggers **WIN sequence** — VFX, audio, loads next scene |
| `"Friendly"` | ✅ Safe to touch — no penalty, decorative objects only |
| *(anything else)* | 💥 Triggers **CRASH sequence** — VFX, audio, reloads current scene |

### Win Condition
When the rocket reaches the `"Finish"` tagged zone:
1. Finish VFX particle system fires
2. Background audio stops; finish chime plays as one-shot
3. Player movement script (`NewMonoBehaviourScript`) is **disabled**
4. `Invoke("LoadNextScene", delayTime)` — scene transitions after `delayTime` seconds (default: **2s**)

### Lose / Crash Condition
When the rocket hits an obstacle or terrain:
1. Crash VFX particle system fires
2. Crash audio plays as one-shot
3. Player movement script is **disabled**
4. `Invoke("ReloadScene", delayTime)` — level reloads after `delayTime` seconds

### Level Looping
After the final level, `LoadNextScene()` wraps back to **scene index 2** (Level 1) — creating an infinite loop of increasing challenge.

---

## 🌍 Game Worlds & Levels

### 🌲 Level 1 — Pixel Forest *(Intro)*
A **2D pixel-art styled** introductory environment easing the player into the controls. Features a blue-suited character/rocket navigating stair-step grass terrain platforms with tall pine trees, fluffy clouds, and a layered parallax mountain backdrop. A warm-up before the 3D challenge begins.

**Visual Style:** Classic 16-bit pixel art, side-scrolling  
**Difficulty:** Beginner — wide corridors, no moving hazards

---

### 🏛 Level 2 — Egyptian Desert Ruins *(Ancient World)*
A stunning **low-poly Egyptian-themed** 3D level set inside a warm desert canyon at sunset. The rocket starts on a stone pedestal and must navigate to a **glowing golden gateway portal** (the Finish zone).

**Key Environment Elements:**
- Giant crossed swords mounted on ancient stone temple pillars
- Cat-paw hieroglyph totems (gold-and-teal cat statues)
- Stepped stone paths and sandy canyon floor
- Central glowing gate — the Finish zone
- Warm orange-to-gold sunset gradient sky

**Visual Style:** Low-poly, warm desert palette  
**Difficulty:** Intermediate — tight gaps around pillars

---

### 🏛 Level 3 — Egyptian Underground City *(Deep Desert)*
An expanded underground Egyptian world — the most visually ambitious level. The player navigates beneath a massive overhanging rock ceiling through an **entire ancient buried city**.

**Key Environment Elements:**
- Enormous natural rock cave with jagged ceiling
- Full ancient city layout: obelisks, temples, market ruins, scattered crates
- A **grand central temple** with a glowing artifact egg at its heart (the Finish zone)
- Giant golden Egyptian Guardian statues flanking both sides of the level
- Symmetrical level design — the finish zone is centered and well-visible
- Sandy warm ambient light with deep brown rock tones

**Visual Style:** Low-poly, underground desert atmosphere  
**Difficulty:** Advanced — dense city obstacles, limited vertical clearance

---

### ⚙️ Level 4 — Sci-Fi Underground Lab *(Future World)*
A **futuristic low-poly cave level** — a stark visual contrast to the Egyptian warmth. The player navigates through a decommissioned underground industrial laboratory.

**Key Environment Elements:**
- Grey rocky cave ceiling and stone walls
- Industrial floor with bold yellow warning stripe markings
- White dome reactor structures and metallic pillar columns
- Red support pillars with glowing yellow strip lights
- A large **yellow spacecraft / research vehicle** as the level's centerpiece
- Blue accent lights on scattered tech structures
- Cool neutral light temperature — sterile and clinical feel

**Visual Style:** Low-poly, sci-fi industrial cave  
**Difficulty:** Expert — narrow corridors, moving machinery obstacles

---

## 📁 Project Structure

```
DevRocket/
├── Assets/
│   ├── Scenes/
│   │   ├── 0-MainMenu.unity           # Scene 0 — Main Menu
│   │   ├── 1-PixelForest.unity        # Scene 1 — Pixel Forest (intro)
│   │   ├── 2-EgyptDesert.unity        # Scene 2 — Egyptian Desert Ruins
│   │   ├── 3-EgyptUnderground.unity   # Scene 3 — Egyptian Underground City
│   │   └── 4-SciFiLab.unity           # Scene 4 — Sci-Fi Underground Lab
│   │
│   ├── Scripts/
│   │   ├── CollisionHandler.cs        # Collision logic, SFX/VFX, scene transitions
│   │   ├── Movement.cs                # Rocket thrust, rotation, particle SFX
│   │   └── Oscillator.cs              # Moving obstacle / platform system
│   │
│   ├── Audio/
│   │   ├── sfx_thrust.wav             # Thruster engine loop
│   │   ├── sfx_crash.wav              # Crash/explosion sound
│   │   └── sfx_finish.wav             # Level complete chime
│   │
│   ├── Particles/
│   │   ├── VFX_Crash.prefab           # Explosion burst on crash
│   │   ├── VFX_Finish.prefab          # Sparkle/celebration on finish
│   │   ├── VFX_ThrusterMain.prefab    # Main bottom thruster flame
│   │   ├── VFX_ThrusterLeft.prefab    # Left rotation thruster
│   │   └── VFX_ThrusterRight.prefab   # Right rotation thruster
│   │
│   ├── Prefabs/
│   │   ├── Rocket.prefab              # Full player rocket with all scripts attached
│   │   └── MovingObstacle.prefab      # Oscillator-driven hazard prefab
│   │
│   └── Materials/
│       ├── Desert/                    # Egyptian level material set
│       ├── SciFi/                     # Sci-fi level material set
│       └── Rocket/                    # Rocket body and thruster materials
│
├── Screenshots/                       # Game screenshots for README
│   ├── gameplay-desert.png
│   ├── editor-egyptian-cave.png
│   ├── editor-scifi-lab.png
│   └── gameplay-pixel-forest.png
│
├── GameLevel.png                      # Full panoramic level overview
└── README.md
```

---

## 📜 Scripts Reference

### `CollisionHandler.cs`

**Attached to:** Rocket / Player GameObject  
**Namespace:** None (global)

The central event-driven collision system. Detects what the rocket hits, routes to the correct response sequence (crash or finish), coordinates audio + VFX, disables player controls, and triggers scene transitions.

```csharp
public class CollisionHandler : MonoBehaviour
```

#### Serialized Inspector Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `sfxCrash` | `AudioClip` | — | Audio played when crashing |
| `sfxFinish` | `AudioClip` | — | Audio played on level completion |
| `vfxCrash` | `ParticleSystem` | — | Explosion particle burst on crash |
| `vfxFinish` | `ParticleSystem` | — | Celebration particle burst on finish |
| `audioSource` | `AudioSource` | — | The rocket's main AudioSource |
| `delayTime` | `float` | `2f` | Seconds before scene transition |

#### Private State

| Variable | Type | Purpose |
|----------|------|---------|
| `isControllable` | `bool` | Guards against double-firing collision sequences during the delay window |

#### Collision Routing Logic

```csharp
private void OnCollisionEnter(Collision collision)
{
    if (isControllable) return; // Block if sequence already started

    switch (collision.gameObject.tag)
    {
        case "Friendly": break;             // Safe — ignore
        case "Finish":   StartFinishSquence(); break;
        default:         StartCrashSquence();  break;
    }
}
```

#### Method Reference

```csharp
void StartFinishSquence()
```
> Sets `isControllable = false` → plays `vfxFinish` → stops engine audio → plays `sfxFinish` as one-shot → disables `NewMonoBehaviourScript` → `Invoke("LoadNextScene", delayTime)`

```csharp
void StartCrashSquence()
```
> Sets `isControllable = true` → plays `vfxCrash` → plays `sfxCrash` as one-shot → disables `NewMonoBehaviourScript` → `Invoke("ReloadScene", delayTime)`

```csharp
void LoadNextScene()
```
> Gets `buildIndex` of current scene → loads `currentIndex + 1` → if at last scene: wraps to index `2` (first gameplay level)

```csharp
void ReloadScene()
```
> Reloads current scene by build index — instant restart

```csharp
void CheckDebugKeys()   // L = next level, C = toggle no-clip
void CheckForQuit()     // ESC = Application.Quit()
```

---

### `Movement.cs` (`NewMonoBehaviourScript`)

**Attached to:** Rocket / Player GameObject  
**Namespace:** None (global)

Controls all rocket movement using Unity's **New Input System**. Applies upward thrust via `Rigidbody.AddRelativeForce` in `FixedUpdate` (physics-safe) and handles rotation in `Update` (input-responsive). Manages 5 particle systems and 1 AudioSource in sync with input state.

```csharp
public class NewMonoBehaviourScript : MonoBehaviour
```

#### Serialized Inspector Fields

| Header | Field | Type | Description |
|--------|-------|------|-------------|
| **InputActions** | `thrust` | `InputAction` | Bound to Space/W/↑ — reads as button |
| **InputActions** | `rotation` | `InputAction` | Bound to A/D or ←/→ — reads as float (-1 or +1) |
| **Component References** | `rb` | `Rigidbody` | Physics body — auto-fetched in `Start()` |
| **Component References** | `audioSource` | `AudioSource` | Engine audio — auto-fetched in `Start()` |
| **Component References** | `sfxthrust` | `AudioClip` | Thruster engine audio clip |
| **Particle Systems** | `sfxmain` | `ParticleSystem` | Main bottom thruster flame |
| **Particle Systems** | `sfxleft` | `ParticleSystem` | Left rotation thruster flame |
| **Particle Systems** | `sfxright` | `ParticleSystem` | Right rotation thruster flame |
| **Power Values** | `thrustPower` | `float` | Force applied each FixedUpdate frame |
| **Power Values** | `rotationPower` | `float` | Degrees per second of rotation |

#### Physics Implementation Detail

```csharp
// THRUST — FixedUpdate for consistent physics
private void ProcessThrust()
{
    if (thrust.IsPressed())
    {
        rb.AddRelativeForce(Vector3.up * thrustPower * Time.deltaTime);
        // AddRelativeForce uses LOCAL space — force always goes out the rocket's nose
        // regardless of current tilt angle. Authentic rocket feel.
    }
}

// ROTATION — Update for responsive input
private void ProcessRotation()
{
    float rotationValue = rotation.ReadValue<float>(); // -1 (left) or +1 (right)
    if (rotationValue != 0)
    {
        rb.freezeRotation = true;   // Temporarily override physics so we control rotation manually
        transform.Rotate(rotationValue * -Vector3.forward * rotationPower * Time.deltaTime);
        rb.freezeRotation = false;  // Return control to physics (gravity, collisions)
    }
}
```

#### Particle System Trigger Logic

| Input State | Particles Active |
|-------------|-----------------|
| Thrusting (`Space` held) | `sfxmain` ✅ |
| Not thrusting | `sfxmain` ❌, audio stopped |
| Rotating left (`A` / `←`) | `sfxleft` ✅ |
| Rotating right (`D` / `→`) | `sfxright` ✅ |
| No rotation | `sfxleft` ❌, `sfxright` ❌ |

---

### `Oscillator.cs`

**Attached to:** Moving obstacle and platform GameObjects

Smoothly moves any GameObject between two world positions using `Mathf.PingPong` for continuous oscillation and `Vector3.Lerp` for smooth interpolation. A purely data-driven component — just set the direction/distance and speed in the Inspector.

```csharp
public class Oscillator : MonoBehaviour
```

#### Serialized Inspector Fields

| Field | Type | Description |
|-------|------|-------------|
| `movementVector` | `Vector3` | Direction and total distance of movement (world space offset from start) |
| `speed` | `float` | Oscillation speed — higher = faster back-and-forth |

#### How It Works

```csharp
private void Start()
{
    startPosition = transform.position;
    endPosition = startPosition + movementVector; // Compute end point once
}

private void Update()
{
    // PingPong returns a value that ramps: 0 → 1 → 0 → 1 → ...
    movementFactor = Mathf.PingPong(Time.time * speed, 1f);

    // Lerp smoothly interpolates between the two endpoints
    transform.position = Vector3.Lerp(startPosition, endPosition, movementFactor);
}
```

#### Practical Configuration Examples

| `movementVector` | `speed` | Result |
|-----------------|---------|--------|
| `(4, 0, 0)` | `1.0` | Slow horizontal patrol, 4 units wide |
| `(0, 3, 0)` | `2.5` | Fast vertical bounce, 3 units tall |
| `(0, 0, 6)` | `0.5` | Very slow depth patrol, 6 units deep |
| `(2, 2, 0)` | `1.5` | Diagonal movement — tricky hazard! |

---

## ⚙️ Physics & Movement System

The rocket's feel is entirely controlled by `Rigidbody` settings and the `thrustPower`/`rotationPower` values in `Movement.cs`.

### Recommended Rigidbody Settings

```
Rigidbody Configuration:
├── Mass:                  1
├── Drag:                  0        (no air resistance — floaty feel)
├── Angular Drag:          0.05     (slight rotational damping)
├── Use Gravity:           true     (gravity works against thrust)
├── Interpolation:         Interpolate  (smoother visuals between physics steps)
├── Collision Detection:   Continuous   (prevents tunneling at high thrust)
└── Freeze Rotation:       X, Y axes    (only rotate on Z for 2.5D play)
```

### Tuning the Game Feel

| Parameter | Too Low | Too High | Sweet Spot |
|-----------|---------|----------|------------|
| `thrustPower` | Rocket barely lifts | Instant overshoot | ~500–1500 |
| `rotationPower` | Impossible to steer | Spins uncontrollably | ~100–200 |
| `delayTime` | Scene switches too abruptly | Waiting feels too long | `1.5f`–`2.5f` |
| Oscillator `speed` | Trivially easy to dodge | Nearly impossible | `0.5`–`2.0` |
| `Rigidbody.drag` | Very floaty | Sluggish | `0`–`0.5` |

---

## 🔊 Audio & VFX System

### Audio Architecture

The rocket uses a single `AudioSource` component for **engine audio** (looping), with `PlayOneShot()` calls for event sounds (crash, finish) that layer on top without interrupting.

| Clip | Trigger | Method | Notes |
|------|---------|--------|-------|
| `sfxthrust` | While thrust held | `PlayOneShot` (re-called) | Checked with `isPlaying` to avoid stacking |
| `sfxCrash` | Obstacle collision | `PlayOneShot` | Plays over engine audio |
| `sfxFinish` | Finish zone reached | `PlayOneShot` (after `Stop()`) | Engine audio stopped first |

### Particle System Architecture

| System | Trigger | Position | Effect |
|--------|---------|----------|--------|
| `sfxmain` | Thrust key held | Bottom nozzle | Main engine flame |
| `sfxleft` | Rotate left input | Left side port | Left thruster puff |
| `sfxright` | Rotate right input | Right side port | Right thruster puff |
| `vfxCrash` | Any obstacle hit | Rocket body center | Explosion burst |
| `vfxFinish` | Finish zone hit | Rocket body center | Sparkle celebration |

---

## 🛠 Debug & Dev Tools

Two hotkeys are active in all builds via `CollisionHandler.Update()`:

| Key | Action | Purpose |
|-----|--------|---------|
| `L` | Load next scene | Skip levels quickly during testing |
| `C` | Toggle `isControllable` | Enable no-clip — fly through all walls |
| `ESC` | `Application.Quit()` | Exit game from any scene |

> ⚠️ **Production Warning:** These debug keys are always active in the current build. For a shipped release, wrap them in `#if UNITY_EDITOR`:

```csharp
private void CheckDebugKeys()
{
#if UNITY_EDITOR
    if (Keyboard.current.lKey.wasPressedThisFrame) LoadNextScene();
    if (Keyboard.current.cKey.wasPressedThisFrame) isControllable = !isControllable;
#endif
}
```

---

## 🕹 How to Play

### Controls

| Action | Key(s) | Description |
|--------|--------|-------------|
| **Thrust** | `Space` / `W` / `↑` | Fire main engine — rocket moves upward relative to its nose direction |
| **Rotate Left** | `A` / `←` | Rotate rocket counter-clockwise |
| **Rotate Right** | `D` / `→` | Rotate rocket clockwise |
| **Quit Game** | `Escape` | Exit the application |
| *(Debug)* Skip Level | `L` | Immediately load next scene |
| *(Debug)* No-Clip | `C` | Toggle collision handling |

### Pro Tips 🎯

- 🎯 Use **short thrust bursts** instead of holding — gives you much finer altitude control
- 🔄 **Rotate first, then thrust** — changing direction mid-thrust is harder to control
- ⏳ After a crash, just **wait** — the level auto-reloads in 2 seconds
- 🚪 The **glowing gateway** or bright zone is always the Finish — aim for it
- 🌀 Watch oscillating obstacles — **time your gaps**, don't rush through
- 💡 If you're stuck, use `L` in debug mode to preview the next level

---

## 🛠 Installation & Setup

### Requirements

| Tool | Version | Notes |
|------|---------|-------|
| Unity Editor | 2022.x LTS+ | Older versions may work but are untested |
| Universal Render Pipeline (URP) | 14.x | Required for post-processing and lighting |
| Cinemachine | 2.9.x | Required for camera system |
| Input System (New) | 1.6.x | **Required** — scripts use new InputAction API |
| TextMeshPro | 3.x | For any UI text elements |

### Clone & Open

```bash
# 1. Clone the repository
git clone https://github.com/Chandan-Baskey/DevRocket.git

# 2. Open Unity Hub → Add project from disk
#    Navigate to the cloned DevRocket/ folder and select it

# 3. Unity will import all assets automatically (allow 2–5 minutes on first open)

# 4. When prompted about the New Input System → click YES and allow Unity to restart
#    (Required: without this, Movement.cs will throw compile errors)

# 5. Open File > Build Settings
#    Verify all scenes are added in the correct index order (see table below)

# 6. Press ▶ Play in the Unity Editor to test!
```

> ⚠️ **Input System Note:** This project uses Unity's **New Input System**. On first open, Unity will show a dialog asking to enable it. Click **Yes** and let Unity restart. If you skip this step, `Movement.cs` will fail to compile because `InputAction` and `Keyboard.current` are from the new system package.

---

## 📦 Build Instructions

### 🖥 PC (Windows / macOS / Linux)

1. Open `File > Build Settings`
2. Select **PC, Mac & Linux Standalone**
3. Choose **Target Platform** (Windows x86_64 recommended)
4. Confirm all scenes are listed in correct order
5. Click **Build and Run** → choose your output folder

### 📱 Android *(Future)*

1. Install **Android Build Support** from Unity Hub
2. Switch platform to **Android** in Build Settings
3. In `Player Settings`: set Minimum API Level to **API 23 (Android 6.0)**
4. Enable **Auto Graphics API** (or set Vulkan / OpenGLES3 manually)
5. Connect device via USB → **Build and Run**

---

## 📋 Scene Build Index

> ⚠️ Critical: `CollisionHandler.LoadNextScene()` uses build index arithmetic. Scene order **must** match exactly.

```
Build Index | Scene File               | Description
------------|--------------------------|---------------------------------------
     0      | 0-MainMenu.unity         | Main Menu / Title Screen
     1      | 1-PixelForest.unity      | Level 1 — Pixel Forest (intro)
     2      | 2-EgyptDesert.unity      | Level 2 — Egyptian Desert Ruins
     3      | 3-EgyptUnderground.unity | Level 3 — Egyptian Underground City
     4      | 4-SciFiLab.unity         | Level 4 — Sci-Fi Underground Lab
```

**Loop-back logic in `LoadNextScene()`:**
```csharp
if (currentIndex == SceneManager.sceneCountInBuildSettings - 1)
{
    nextIndex = 2; // After Level 4, loop back to Level 2 (first gameplay level)
}
```
> Update the `nextIndex = 2` line if you shift your scene arrangement.

---

## 📦 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| Universal Render Pipeline (URP) | 14.x | Rendering, post-processing, bloom, color grading |
| Cinemachine | 2.9.x | Dynamic rocket-follow camera |
| Input System (New) | 1.6.x | `InputAction` for thrust and rotation |
| TextMeshPro | 3.x | High-quality UI text |
| Visual Effects Graph *(optional)* | 14.x | Advanced GPU particle VFX |

All packages are managed via **Unity Package Manager** (`Window > Package Manager`).

---

## 🤝 Contributing

Pull requests are welcome! To add a new level or feature:

```bash
# 1. Fork the repository
# 2. Create a feature branch
git checkout -b feature/level-5-underwater

# 3. Create your new scene in Assets/Scenes/
# 4. Register it in File > Build Settings at the next available index
# 5. If it's now the last level, update the loop-back in CollisionHandler.cs
# 6. Commit and push
git add .
git commit -m "Add Level 5: Underwater Research Station"
git push origin feature/level-5-underwater

# 7. Open a Pull Request on GitHub
```

### New Level Checklist

- [ ] New scene file created in `Assets/Scenes/`
- [ ] Scene added to **Build Settings** at correct index
- [ ] `Rocket.prefab` placed as player at the starting position
- [ ] Finish zone tagged `"Finish"` with a trigger collider
- [ ] All decorative/safe objects tagged `"Friendly"`
- [ ] `Oscillator.cs` applied to any moving obstacles
- [ ] Cinemachine Virtual Camera targeting the rocket placed in scene
- [ ] Ambient lighting and skybox configured for theme
- [ ] `CollisionHandler.cs` loop-back index updated if this is the new final level
- [ ] Screenshot added to `Screenshots/` folder and README updated

---

## 👤 Author

**Chandan Baskey**  
🌐 [GitHub — @Chandan-Baskey](https://github.com/Chandan-Baskey)  
📁 [DevRocket Repository](https://github.com/Chandan-Baskey/DevRocket)

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

  <h3>🚀 DevRocket</h3>
  <em>Thrust. Dodge. Finish. Repeat.</em>
  <br/><br/>
  Made with ❤️ in Unity

</div>
