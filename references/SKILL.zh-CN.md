# 精准林业遥感监测 | Precision Forestry Monitoring

融合多源遥感实现林分尺度精准林业监测，覆盖“单木—林分—健康—生物量”四个层次，支撑森林资源调查、碳汇估算与经营决策。

树高/单木：CHM = DSM − DTM，局部峰值检测（非极大值抑制）提取单木位置与树高；由冠幅反演胸径 (DBH = k·CW)，再用异速生长方程 V = a·DBH^b·H^c 估单木蓄积量并累加为林分蓄积量。郁闭度取 CHM 高于阈值的冠层像元占比（∈[0,1]）。健康由 NDVI 与 NDRE 组合分级（健康/中等/胁迫）。生物量由 SAR 后向散射 σ⁰(dB)→线性→幂函数经验关系估算，并据郁闭度/蓄积量/健康生成经营建议。

## 依赖

```bash
pip install 'numpy' 'rasterio' 'scipy' 'scikit-learn'
```

## 使用方法

### 基本用法

```bash
python geoskill-precision-forestry-monitoring.py --bbox 116.0 39.0 117.0 40.0 [其他参数]
```

### 示例 1（合成林分，离线）

```bash
python geoskill-precision-forestry-monitoring.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

### 示例 2（真实 6 波段影像（DSM/DTM/Red/NIR/RedEdge/SAR））

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --output-dir ./out
```

### 示例 3（仅用 NDVI 分级健康）

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --health-method ndvi --output-dir ./out
```

### 示例 4（调高最小检测树高与窗口）

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --min-height 5 --footprint 7 --output-dir ./out
```

### 示例 5（调整郁闭度阈值）

```bash
python geoskill-precision-forestry-monitoring.py --input forest.tif --closure-threshold 3 --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `chm.tif` | GeoTIFF | 冠层高度模型（树高，m） |
| `health_grade.tif` | GeoTIFF | 健康分级（3 健康 / 2 中等 / 1 胁迫 / 0 裸地） |
| `biomass_t_ha.tif` | GeoTIFF | SAR 反演地上生物量（t/ha） |
| `canopy_mask.tif` | GeoTIFF | 冠层覆盖掩膜 |
| `trees.geojson` | GeoJSON | 单木位置（树高/冠幅/胸径/蓄积量） |
| `forestry_report.json` | JSON | 林分统计 + 经营建议 |
| `output-manifest.json` | JSON | 运行清单 |

## 数据源 / Source

多波段 GeoTIFF，波段顺序 DSM / DTM / Red / NIR / RedEdge / SAR。 或使用 `--synthetic` 生成物理一致的模拟数据（完全离线）。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
