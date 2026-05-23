# 🎯 DroppingCalc Clone — Free Fortnite Drop Calculator

A free, open-source clone of [droppingcalc.com](https://www.droppingcalc.com/).
**Same map, same physics, same 318 known drop spots, $0.**

## ✨ What it does

Calculates the **optimal pixel drop** to land first in Fortnite, using the same algorithm and data as the paid services:

1. 🚌 You input the **battle bus trajectory** (2 clicks)
2. 🎯 You pick your **landing spot** (1 click, OR pick from 318 known spots)
3. ⚡ The calculator instantly returns:
   - 🔴 **Red point** — when to jump from the bus
   - 🟢 **Green point** — the exact pixel to aim at during free-fall
   - 🔵 **Blue point** — where you'll land
   - ⏱ **Total drop time** + breakdown (bus / fall / glide)

## 🧠 How is this different from my own attempt?

Three big improvements over a homemade calculator:

### 1. **Real Fortnite physics constants** (from [Violevo's open-source calculator](https://github.com/Violevo/Fortnite-Drop-Calculator))
```js
v_bus     = 73.3 m/s   // battle bus speed
v_fall_h  = 14.5 m/s   // horizontal speed during free-fall
v_fall_v  = 32.0 m/s   // vertical free-fall speed
v_glide_h = 17.0 m/s   // glider horizontal speed
v_glide_v = 7.0 m/s    // glider descent rate
H_bus     = 832 m      // bus altitude
H_deploy  = 30 m       // glider auto-deploy altitude
```
These are the actual values reverse-engineered from the game.

### 2. **Proper two-case algorithm**
For each candidate jump point along the bus:
- **Case 1 (short/medium distance):** Use the full free-fall, then deploy the glider at the minimum altitude. We have spare horizontal range so we can't waste it.
- **Case 2 (long distance):** We need the glider for *more* than the minimum descent. Solve the system of equations (`distance = v_fall_h*t_f + v_glide_h*t_g`, `altitude = v_fall_v*t_f + v_glide_v*t_g = H_bus`) for the exact `t_f` that lands on target.

The winner is the jump point that minimizes `t_bus + t_fall + t_glide`.

### 3. **Real droppingcalc.com data** (318 known spots across 107 POIs)
Their public API at `/api/sync/map?map=br` returns the entire list of pre-mapped drop spots. We embed this in the page, so you can:
- See the same numbered POI markers (Battlewood Boulevard, Sandy Strip, Wonkeeland, etc.)
- Click any number to see all spots within that POI (Floor Spawn, Roof Chest, Top Truck…)
- Search any POI or specific spot in the sidebar
- Click a spot → it becomes your target → calculator runs instantly

## 🗺️ Live map tiles

The map uses droppingcalc's public CDN: `https://cdn.droppingcalc.com/40.41/{z}/{x}/{y}.webp`.
- ✅ Always up-to-date with the current Fortnite patch
- ✅ 5 zoom levels (up to 8192×8192 pixel detail)
- ✅ Auto-falls-back to previous patches if a tile is missing

## 🚀 Run it

### Locally
Open `index.html` (landing page) or `map.html` (calculator) in your browser. Works offline once tiles are cached.

### Host it for free
- **Netlify Drop** (no account): https://app.netlify.com/drop → drag the folder
- **GitHub Pages**: push to a public repo, Settings → Pages
- **Vercel**: import the folder

## 📂 Files

- `index.html` — Landing page (hero, features, marquee of pros, stats)
- `map.html` — The calculator (Leaflet + droppingcalc tiles + 318 spots data)
- `README.md` — This file

## ⌨️ Shortcuts

| Key | Action |
|-----|--------|
| `B` | Set bus mode |
| `T` | Pick spot mode |
| `F` | Save current drop as favorite |
| `R` | Reset |
| `0` / `Home` | Recenter map |
| `Esc` | Cancel current mode |
| Mouse wheel | Zoom |

## 🔬 Differences vs the paid version

| Feature | This | droppingcalc.com |
|---------|------|------------------|
| Live tiles | ✅ | ✅ |
| 318 known spots | ✅ | ✅ |
| Physics-correct calc | ✅ | ✅ |
| Auto-patch updates | ✅ (via CDN) | ✅ |
| Save personal drops | ✅ (local) | ✅ (cloud) |
| Squad sync | ❌ | ✅ |
| Account/login | ❌ | ✅ |
| **Price** | **$0** | $5-15/mo |

## ⚖️ Legal note

This is a free educational clone made for the community.
Not affiliated with Epic Games, Fortnite, or droppingcalc.com.
Tile images and POI metadata are loaded from droppingcalc's public CDN/API.
Physics model is GPL v3 from Violevo/Fortnite-Drop-Calculator.

If droppingcalc requests it, we'd happily switch to self-hosted tiles
(download the tile pyramid once — see notes in `map.html`).
