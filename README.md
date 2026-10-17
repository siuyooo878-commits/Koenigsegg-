# How Does a Hypercar Get Pushed Toward the Road? — Koenigsegg Jesko Attack (Three.js, 9:16, 76 s)

## Run
```
cd <this folder>
python3 -m http.server 8000
# open http://localhost:8000
```
HTTP is required: browsers block local-file loading of models/textures from file://.
Phone on the same Wi-Fi: open `http://<computer-LAN-IP>:8000`.
**Online dependency:** Three.js 0.180.0 from cdn.jsdelivr.net (import map in index.html). Everything else is local.

## Assets (all next to index.html)
| File | Role |
|---|---|
| `FINAL_MODEL_AT.fbx` (16 MB) | Hero Koenigsegg Jesko Attack (binary FBX 7.7, extracted from the nested zip `source/FINAL_MODEL_AT.zip`) |
| 30 × `T_*.png`, `common_carbon05_*.png` | The textures this FBX references (loaded by basename from this folder). Unreferenced textures from the zip were left out. |
| `bloem_hill_01_2k.hdr` | HDRI: lighting, reflections, background (RGBELoader + PMREM) |
| `ImageToStl_com_asphalt_pit_lane_1k_gltf.glb` | Asphalt: it is a material-preview *sphere*; only its base-colour, metal/rough and normal textures are used, tiled on a large ground plane |

Total ≈ 29 MB (above the 25 MB aim; the FBX, HDR and GLB are used as supplied).

## Controls
Play/Pause, Restart, scrubber, scene chips 1–10, layer toggles (airflow, pressure, vectors, labels, underbody cutaway, simple explainer), Inspect/orbit (drag to orbit, shows diagnostics + bounding box/axes/forward arrow; timeline keeps running), Reset camera, quality (540×960 / 720×1280 / 1080×1920), preview speed, sound + volume, optional browser speech (TTS), Record.
Every frame is a pure function of the master time T, so seeking/pausing/jumping reconstructs car, camera, particles, captions, arrows and audio parameters.

## Recording
Record plays the film from 0 at 1× and captures the composite canvas (3D scene + captions + labels + data cards, no DOM UI) with `captureStream(30)` + `MediaRecorder`. Audio (procedural, via Web Audio) is added only if Sound is on. Format is the first supported of MP4 (H.264/AAC) → WebM VP9/Opus → VP8/Opus → WebM; the status line reports the real format. The download is triggered when the recorder stops. Keep the tab visible while recording.

## Real geometry vs explanatory additions
Real (imported): body, rear wing (meshes named *Spoiler*), front/rear bumpers, wheels/tyres, glass, mirrors.
Procedural (explanatory, not factory geometry): streamlines/particles (shaped from a height field sampled from the real mesh vertices), pressure blobs, force arrows, cyan underbody section line, "active front flaps" schematic, the three 2-D inset schematics. Not CFD.
Limits: wheel rotation uses the FBX bone nodes named `Ani_Wheel_*`; steering, active wing/flap articulation and suspension are not animated. Mesh names are hints; positions come from vertex bounds.
If the car loads facing the wrong way, add `?flip=1` to the URL.

## Narration (for your own voiceover)
1. 0:00 At high speed, the Jesko Attack turns the air around it into grip.
2. 0:06 Air follows the car's shape, but the flow also creates pressure differences.
3. 0:13 Its double-profile rear wing redirects air; the reaction creates a downward force on the car.
4. 0:21 Up front, the long splitter and active underbody flaps manage airflow and help balance the nose.
5. 0:29 Under the car, shaped surfaces and diffusers guide air and manage how it expands behind the floor.
6. 0:37 Together, these parts balance front and rear downforce—not just produce as much as possible.
7. 0:44 And speed changes everything. Aerodynamic force grows roughly with the square of airspeed.
8. 0:51 Koenigsegg lists 700 kilograms of downforce at 250 kilometers per hour, and 1,000 at 290.
9. 1:00 Koenigsegg lists a maximum of 1,400 kilograms of downforce—a force expressed in kilogram-equivalent terms.
10. 1:08 That extra load can help the tires generate grip through corners—but only when the aero, tires, and track cooperate.

Figures (700 kg @ 250 km/h, 1,000 kg @ 290 km/h, 1,400 kg max) are Koenigsegg's published values, taken from the brief; I could not open the source page from here to re-check them. kg = kilogram-equivalent force, not added mass. The 100→200 km/h ×4 diagram is a simplified F ∝ v² illustration, not Jesko data.

## Verification report (honest)
VERIFIED here: the FBX is a valid binary FBX 7.7 with named parts (Spoiler, Bumper_F/B, Ani_Wheel_FL/BR nodes, Paint/Carbon/Glass/Tyre materials), Y-up, unit scale 1; the 30 referenced textures are present; the HDR is 2048×1024 RGBE; the GLB holds 3 JPEG PBR textures on a sphere; the inline JavaScript passes a Node syntax check.
NOT VERIFIED: **the page has never been run.** This environment has no browser and no network, so I did not test model import, orientation, ground contact, wheel pivots, camera framing/clipping, streamline fit, performance, scrubbing, audio, or recording. Expect to need tweaks (most likely: model orientation/scale, wheel axes, camera keys in the `K` array). Open Inspect mode and read the diagnostics block first.
