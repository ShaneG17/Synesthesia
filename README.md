# Synesthesia

Music becomes painting. Upload a song or use your microphone and watch it paint itself in real time.

---

## What it does

Synesthesia analyzes audio in real time and translates it into a generative painting on canvas. Every frequency range maps to a different color and painting style — bass hits paint red splatters, vocals leave green brushstrokes, cymbals scatter violet shimmer. No two songs produce the same painting.

---

## How it works

### Frequency analysis
The Web Audio API splits audio into 8 frequency bands, each corresponding to a real instrument range:

| Band | Range | Color | Style |
|---|---|---|---|
| Kick / Sub | 20–80hz | Crimson red | Splatters with flying drops |
| Bass | 80–250hz | Orange | Long brushstrokes |
| Low mid / Guitars | 250–500hz | Gold | Radial blooms |
| Mid / Vocals | 500–2khz | Green | Flowing bezier curves |
| High mid / Piano | 2–4khz | Cyan | Fine splats |
| Presence / Hi-hat | 4–6khz | Blue | Micro splatter |
| High / Cymbals | 6–12khz | Violet | Shimmer streaks |
| Air | 12–20khz | Lavender | Dust particles |

### Beat detection
Energy history is tracked across ~1 second of frames. When a beat is detected (energy exceeds the running average + variance threshold), splatter intensity doubles and a radial pulse fires from the center. BPM is estimated from inter-beat intervals.

### Song section analysis
Energy is averaged over ~5 second windows and compared to a long-term average. The ratio between recent and long-term energy determines the section — chorus is loud relative to average, bridge is quiet, verse sits in between.

### Spectral flux (onset detection)
Frame-to-frame changes in the frequency spectrum are measured. Sudden spikes trigger extra chaos painting — useful for detecting individual note onsets, drum hits, and sharp transients.

### Spectral centroid
The "center of mass" of the frequency spectrum is tracked. A high centroid means energy is concentrated in the treble. This drives painting direction in Flow and Ink modes.

### Frequency fingerprinting
When a file is uploaded, an OfflineAudioContext scans the first 30 seconds of audio, sampling the spectrum at 20 points. The spectral balance (bass vs mid vs high energy) generates a unique hue offset that shifts the entire color palette — so every song has its own color identity.

### Painting modes
Four modes auto-switch based on song section and energy:

- **Splatter** — chorus and high energy. Maximum chaos.
- **Flow** — bridge and quiet sections. Centroid-guided particle streams.
- **Crystal** — high-frequency content. Geometric shapes and lines.
- **Ink** — expressive brushstrokes guided by spectral centroid direction.

### Particle agents
120 agents drift through a noise flow field, each tuned to one of the 8 frequency bands. They leave permanent trails proportional to their band's energy.

---

## Controls

| Control | Action |
|---|---|
| Upload audio file | Start with any MP3, WAV, AAC, etc. |
| Use microphone | Live input — beatbox, sing, play an instrument |
| Pause / Resume | Freeze the painting |
| Clear | Wipe the canvas and start fresh |
| Save painting | Download the current painting as a PNG |

The painting accumulates permanently — nothing fades. Every session produces a unique piece.

