# Rangi

A universal colour controller for cheap Bluetooth LED lights — RGB strips, RGBIC pixel strips, bulbs, car footwell kits. One page, no install, no account, no ads, nothing sent anywhere.

Built by [Jewel Films](https://www.jewelfilms.co.ke).

## What it controls

Rangi identifies a controller by the **GATT services it actually exposes**, not by its advertised name, because the names on these devices are unreliable. Four protocol families are supported:

| Family | Sold as | Service / write |
|---|---|---|
| ELK-BLEDOM | ELK-BLEDOM, ELK-BLEDOB, ELK-BLEDDM, LEDBLE, duoCo Strip | `fff0` / `fff3` |
| Triones | Triones-XXXX, HappyLighting, most RGBW strip boxes | `ffd5` / `ffd9` |
| MagicLight / Zengge | LEDBlue, LEDnet, Magic Hue bulbs | `ffe5` / `ffe9` |
| SP110E pixel | SP110E, SP107E, SP105E, BanlanX (WS2812B, SK6812, APA102) | `ffe0` / `ffe1` |

If none match, the **protocol prober** walks each one in turn, sends a green test, and asks you which worked.

## What it does

**Colour** — HSV wheel, precise sliders, hex entry, 2000–9000 K white bar (driving the real white channel where the strip has one), saved swatches, palette extraction from a photo.

**Effects** — 19 effects drawn frame by frame on the phone and streamed over BLE, so they work regardless of what the controller's own chip can do: breathe, candle, fire, lightning, heartbeat, rainbow, ocean, forest, sunrise, sunset, police, twinkle and more. Speed, depth and send rate are all adjustable. The controller's own built-in effects are also exposed, for when you want to walk away and leave them running.

**Effect builder** — your own sequence of colour stops with per-stop durations, fading or jumping, saved and replayable.

**Music** — microphone, a local track, or shared screen audio. Split into bass / mid / treble with adaptive beat detection. Five reaction modes and six palettes.

**Ambient** — match what the camera sees, or what's on a shared screen (ambilight for a film or a match).

**Groups** — drive several lights as one, in step, mirrored, or chasing along the row.

**Timers** — wake-up fade from amber to daylight, wind-down fade to off, countdowns, and a daily schedule.

**Calibration** — per light: channel order (RGB/GRB/BGR…), gamma, per-channel trim, and for ELK units the frame variant and trailer bytes. This is what to reach for when you pick green and the strip goes purple.

## Browser support

Rangi needs **Web Bluetooth**, which is not a Safari feature.

- **Android** — Chrome. Bluetooth and location permission both need to be on; Android requires location before any app may scan.
- **Desktop** — Chrome or Edge, any OS.
- **iPhone / iPad** — Safari will not work. Use [Bluefy](https://apps.apple.com/app/bluefy-web-ble-browser/id1492822055) or WebBLE, which are browsers built around Web Bluetooth.

Without Bluetooth, everything still runs against a **simulated strip** — useful for building scenes and effects away from the hardware.

## Running it

It is one self-contained file. Open `index.html` over HTTPS or on localhost (Web Bluetooth refuses plain HTTP) and that is the whole app.

Deployed on Vercel from this repository; pushes to `main` go live.

## Protocol notes

The byte formats are community reverse-engineering, not vendor documentation, so a given unit can disagree with its family. When colour commands are clearly landing but the colour is wrong, the fix is almost always in the light's calibration settings rather than in the code. Credit to the people who worked these out:

- [FergusInLondon/ELK-BLEDOM](https://github.com/FergusInLondon/ELK-BLEDOM/) and [dave-code-ruiz/elkbledom](https://github.com/dave-code-ruiz/elkbledom)
- [swkim01/ble](https://github.com/swkim01/ble) for the Triones and MagicLight command set
- [nguyenthuongvo/Vox](https://github.com/nguyenthuongvo/Vox) for the SP110E command table

## Privacy

No analytics, no network requests, no account. Saved colours, scenes, effects and calibration live in this browser's local storage and travel between devices only by the copy-and-paste setup string in Settings.
