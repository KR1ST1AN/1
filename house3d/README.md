# Huone 3D: house3d-card for Home Assistant

Your Sweet Home 3D room as a rotatable 3D card: tap devices to control them, a Devices list with
Shortcuts and If / Then / Else automations, Phone view, weather with a rain map, and live lights.

## What's in here

| File | What it is |
| --- | --- |
| `house3d-card.js` | The card itself |
| `three.module.min.js` | three.js r160 (3D engine, MIT licence), loaded by the card, so no internet is needed for it |
| `huone/` | The room model from Sweet Home 3D (`Huone.obj`, `Huone.mtl` and its textures) |
| `robot_vacuum.glb` | The robot vacuum model (low-poly model by Moryak on Sketchfab, CC BY 4.0) |
| `card.yaml` | Your card's configuration: entities, positions, Phone view order, weather |
| `preview.html` | The demo page with a pretend Home Assistant, to try it without Home Assistant |

## Install

1. Copy this whole `house3d` folder to `/config/www/`, so you have `/config/www/house3d/house3d-card.js`.
   (With the File editor or Samba add-on, or `scp`.) If `www` didn't exist yet, restart Home Assistant once.
2. Add the card as a resource: **Settings → Dashboards → ⋮ → Resources → Add resource**
   - URL: `/local/house3d/house3d-card.js?v=1`
   - Type: **JavaScript module**
3. Add the card: open your dashboard → **Edit** → **Add card** → **Manual**, and paste the contents of `card.yaml`.
4. Reload the page.

After updating `house3d-card.js`, bump the `?v=1` in the resource URL (to `?v=2` and so on) so browsers and the
companion app load the new version.

## Worth knowing

- **Entities:** `card.yaml` uses the entity IDs from the preview (`light.hogvind`, `fan.dyson`, `cover.blind`,
  `binary_sensor.unifi_internet`...). If any of yours are named differently, change them there.
- **Shortcuts & automations** you make in the card are saved with your Home Assistant user. Automations with a
  trigger are saved as real Home Assistant automations (only admin users can create them).
- **Rain map:** uses Home Assistant's home location, radar from RainViewer and map tiles from CARTO /
  OpenStreetMap (needs internet). Adjust or turn off with `weather: { entity: ..., rain_map: { zoom: 7 } }` or
  `rain_map: false`.
- **Kiosk mode** (☰ → Kiosk mode): the card covers the whole screen over Home Assistant's header and sidebar, goes
  fullscreen and keeps the screen awake, for a wall tablet. It's remembered per screen; after a reload it covers the
  screen straight away and goes fullscreen again on the first tap. Leave it from ☰, or with Esc / the back gesture.
- **Phone view** order: `phone_first:` and `phone_last:`; everything else goes left to right from the start view.
- **Free camera:** `snap: false` (or ☰ → Snap to angles). `return_home: 4` glides back to the start view after
  4 s without interaction; set it to `0` to turn that off.
- **Preview:** serve the folder (for example `python3 -m http.server` inside it) and open `preview.html`.
  Opening it straight from disk won't work, since browsers block loading the model from `file://`.
