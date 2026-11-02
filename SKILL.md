---
name: geoskill-precision-forestry-monitoring
description: 'CHM tree height and crown width allometric volume, NDVI/NDRE health grading and SAR biomass fusion for precision forestry'
---

# 精准林业遥感监测 | Precision Forestry Monitoring

Integrates multi-source remote sensing for stand-scale precision forestry monitoring, covering four levels — individual tree, stand, health and biomass — to support forest resource inventory, carbon sink estimation and management decisions.

Tree height / individual trees: CHM = DSM − DTM, with local peak detection (non-maximum suppression) extracting individual tree locations and heights. Crown width is used to invert diameter at breast height (DBH = k·CW), and the allometric equation V = a·DBH^b·H^c estimates individual-tree volume, which is summed into stand volume. Canopy closure is the proportion of canopy pixels whose CHM exceeds a threshold (∈[0,1]). Health is graded by a combination of NDVI and NDRE (healthy / moderate / stressed). Biomass is estimated from SAR backscatter σ⁰ (dB) → linear → power-function empirical relationships, and management recommendations are generated from canopy closure / stand volume / health.

## Dependencies / 依赖

```bash
pip install 'numpy' 'rasterio' 'scipy' 'scikit-learn'
```

## Usage / 使用方法

### Basic usage

```bash
python geoskill-precision-forestry-monitoring.py --bbox 116.0 39.0 117.0 40.0 [other options]
```

### Example 1 (synthetic stand, offline)

```bash
python geoskill-precision-forestry-monitoring.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

### Example 2 (real 6-band imagery (DSM/DTM/Red/NIR/RedEdge/SAR))

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --output-dir ./out
```

### Example 3 (health grading using NDVI only)

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --health-method ndvi --output-dir ./out
```

### Example 4 (increase the minimum detection height and window)

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --min-height 5 --footprint 7 --output-dir ./out
```

### Example 5 (adjust the canopy closure threshold)

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --closure-threshold 3 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `chm.tif` | GeoTIFF | Canopy height model (tree height, m) |
| `health_grade.tif` | GeoTIFF | Health grade (3 healthy / 2 moderate / 1 stressed / 0 bare ground) |
| `biomass_t_ha.tif` | GeoTIFF | SAR-derived above-ground biomass (t/ha) |
| `canopy_mask.tif` | GeoTIFF | Canopy cover mask |
| `trees.geojson` | GeoJSON | Individual tree locations (height / crown width / DBH / volume) |
| `forestry_report.json` | JSON | Stand statistics + management recommendations |
| `output-manifest.json` | JSON | Run manifest |

## Data Source / 数据源 / Source

Multi-band GeoTIFF with band order DSM / DTM / Red / NIR / RedEdge / SAR. Alternatively, use `--synthetic` to generate physically consistent simulated data (fully offline).

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; user data is never uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
