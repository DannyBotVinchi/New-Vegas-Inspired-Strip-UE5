# New Vegas-Inspired Strip — UE5 Environment & Systems Study

<!-- HERO IMAGE: your best dusk shot of the Strip (the tower lit against the orange sky) -->
![The Strip at dusk](media/hero_dusk.png)

A non-commercial fan project rebuilding a post-apocalyptic desert casino district in Unreal Engine 5, built entirely in Blueprints.
This project started as a learning sandbox alongside my main game. It's where I worked out
environment building, time-of-day lighting, and AI behavior at district scale before using those
skills in my main project.

▶️ **Video breakdown:** [YouTube link]
🎮 **My main project:** [UCUmc73Z1On0DePpVBa0pyqA / game page]

> **Status:** In progress — on hold while I focus on my main game. Some areas are intentionally
> unfinished; the focus of this project is systems and the Strip's layout, not a full world.

---

## What I Built

### 🏙️ The Strip
<!-- 1–2 screenshots: overview shot + a street-level shot (e.g. the Freeside entrance) -->
![Strip overview](media/strip_overview.png)

- Laid out the Strip and its surrounding district from scratch: [roads, blocks, landmarks, district transitions]
- Designed the contrast between the lit, maintained Strip and the decayed outer district: [how you did this, e.g. lighting, materials, prop density]
- [Any level-design techniques you used: modular kits, landscape sculpting, foliage/prop scattering, etc.]

### 🌅 Day/Night Cycle
<!-- GIF of a sped-up full cycle -->
![Day/night timelapse](media/daynight_cycle.gif)

- [How time progresses, e.g. a time-of-day variable driving sun rotation]
- [What reacts to it, e.g. neon/building lights switching on at dusk, sky and fog changes]
- [Anything else it controls: ambient audio, NPC schedules, etc.]
- 🔗 Blueprint: [blueprintue.com link]

### 🤖 Enemy & Friendly AI
<!-- Short clip or screenshot with the Gameplay Debugger visible + Behavior Tree screenshot -->
![AI debug view](media/ai_debug.png)
![Behavior Tree](media/behavior_tree.png)

- [Enemy behavior: e.g. patrol → detect → chase → attack]
- [Friendly/neutral behavior: e.g. wandering, following, reacting to combat]
- [Perception setup: sight/hearing, how targets are chosen]
- 🔗 Blueprint: [blueprintue.com link]

---

## Challenges & How I Solved Them

### 🔦 Searchlights

Sweeping searchlight beams around the central tower that fade in at dusk and out at dawn,
driven by the day/night cycle.

![Searchlights at dusk](media/searchlights_dusk.gif)

- Beams are a cone mesh with a custom additive, unlit material (not real lights), so they're
  visible at any distance and cost almost nothing to render
- Material fades the beam along its length, softens the edges (inverted Fresnel), and blends
  softly into geometry (Depth Fade)
- Brightness is driven by a `BeamFade` value in a Material Parameter Collection, which the
  day/night cycle updates every frame
- Each searchlight sweeps side to side using a sine wave, with instance-editable speed, angle,
  and phase offset so neighboring beams criss-cross instead of moving in sync
- 🔗 Blueprint: [blueprintue.com link]
- 🔗 Day/night fade logic: [blueprintue.com link]

---

## Challenges & How I Solved Them

**Spotlights weren't visible as beams**
My first approach used real spotlights, but they only lit the surfaces they hit: blinding
on the tower and invisible in the air. Volumetric fog could show the beam, but only within a
limited range of the camera, and turning it up enough washed out the whole scene. I switched
to the approach games commonly use: a cone mesh with an additive, unlit material that fakes
the volumetric look. It's visible from any distance and doesn't affect the rest of the lighting.

**Lights snapping on/off instead of fading**
My day/night cycle already toggled lights with a binary on/off value, which works for neon
but looked wrong on searchlights. I added a second parameter to the same Material Parameter
Collection, using two Map Range Clamped ramps around sunset and sunrise combined with a Max
node, so the value fades smoothly across dusk and dawn and handles the wrap past midnight
without special cases. It updates on the cycle's timeline Update so the fade is smooth rather
than stepped.

**The beam wouldn't fade along its length**
I first faded the beam using the mesh's UV coordinates, but no combination of channels worked
because the cone's UVs don't run along its length. I switched to the mesh's local-space
position instead (world position transformed into local space, using the height axis), which
works regardless of how the UVs are laid out or how the actor is rotated.

**The beam swung from the wrong end**
The sweep rotated around the wrong point because the cone's pivot is at its wide end. I added
a separate pivot component to rotate and offset the cone so its tip sits on that pivot. The
offset has to account for scale: the mesh is 100 units tall and scaled 300x, so the tip was
actually 30,000 units from the pivot. That was the fix that made the beams originate from the
tower.

**[Problem 2, e.g. "AI got stuck on the Strip's props and walls"]**
[What went wrong → what you tried → what finally fixed it]

**[Problem 3]**
[What went wrong → what you tried → what finally fixed it]

---

## What This Project Taught Me

- [Skill or lesson 1, and how it carried over into my main game]
- [Skill or lesson 2]
- [Skill or lesson 3]

---

## Tech

- **Engine:** Unreal Engine [version 5.7]
- **Scripting:** Blueprints only
- **Key systems:** Behavior Trees, AI Perception, [Sequencer / Movie Render Queue / other tools used]
- **Plugins/Assets:** [anything notable you used]<img width="2547" height="1154" alt="HighresScreenshot00003" src="https://github.com/user-attachments/assets/0c5aecf6-1cb1-4e3d-bf74-927b5595ce9f" />


---

## Disclaimer

This is a non-commercial fan project made for learning and portfolio purposes. It is not affiliated
with, endorsed by, or connected to Bethesda Softworks, ZeniMax Media, Microsoft, or Obsidian
Entertainment. *Fallout* and all related names and assets are the property of their respective owners.
Project files and source are not distributed.
