# Precision Forestry Remote Sensing Monitoring (geoskill-precision-forestry-monitoring)

> CHM tree height and crown width allometric volume, NDVI/NDRE health grading and SAR biomass fusion for precision forestry

---

## 1. Overview

Fuses multi-source remote sensing for stand-scale precision forestry monitoring, covering four levels — single tree, stand, health, and biomass — to support forest resource inventory, carbon-sink estimation, and management decisions.

Tree height / single tree: CHM = DSM − DTM, and local peak detection (non-maximum suppression) extracts single-tree positions and heights; crown width is used to invert diameter at breast height (DBH = k·CW), and the allometric equation V = a·DBH^b·H^c estimates single-tree volume, accumulated into stand volume. Canopy closure is the fraction of canopy pixels with CHM above a threshold (∈[0,1]). Health is graded by combining NDVI and NDRE (healthy / moderate / stressed). Biomass is estimated from SAR backscatter σ⁰ (dB) → linear → power-function empirical relationship, and management recommendations are generated from canopy closure / volume / health.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-precision-forestry-monitoring.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `chm.tif` | GeoTIFF | Canopy height model (tree height, m) |
| `health_grade.tif` | GeoTIFF | Health grade (3 healthy / 2 moderate / 1 stressed / 0 bare land) |
| `biomass_t_ha.tif` | GeoTIFF | SAR-derived aboveground biomass (t/ha) |
| `canopy_mask.tif` | GeoTIFF | Canopy cover mask |
| `trees.geojson` | GeoJSON | Single-tree positions (height/crown width/DBH/volume) |
| `forestry_report.json` | JSON | Stand statistics + management recommendations |
| `output-manifest.json` | JSON | Run manifest |


## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
