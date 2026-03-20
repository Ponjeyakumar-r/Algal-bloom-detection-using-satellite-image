# 🌊 Algal Bloom Detection Using Satellite Imagery

Detect harmful algal blooms in water bodies using **Sentinel-2 satellite imagery**, **Google Earth Engine (GEE)**, and unsupervised machine learning — all in a single Jupyter Notebook.

---

## 📌 Overview

Algal blooms pose serious threats to aquatic ecosystems and drinking water supplies. This project leverages multispectral satellite data to automatically identify potential bloom regions using:

- **Floating Algae Index (FAI)** — spectral index sensitive to floating vegetation
- **Normalized Difference Vegetation Index (NDVI)** — highlights photosynthetically active material
- **Normalized Difference Water Index (NDWI)** — isolates water bodies from land
- **K-Means Clustering** — unsupervised classification of water pixel spectral signatures

---

## 🛰️ Data Source

| Source | Details |
|--------|---------|
| Satellite | Sentinel-2 (ESA Copernicus) |
| Collection | `COPERNICUS/S2_SR_HARMONIZED` |
| Bands Used | B2 (Blue), B3 (Green), B4 (Red), B8 (NIR), B11 (SWIR) |
| Resolution | 10–20 m native; 100 m used for downloads |
| Access | Google Earth Engine API |

---

## 🔬 Methodology

```
Sentinel-2 Image (GEE)
        │
        ▼
  Band Download (B2, B3, B4, B8, B11)
        │
        ▼
  Water Mask via NDWI (threshold > 0.2)
        │
        ▼
  Spectral Index Calculation (FAI, NDVI)
        │
        ▼
  K-Means Clustering (k=4) on Water Pixels
        │
        ▼
  Bloom Cluster Identification (NIR + FAI + Green/Blue)
        │
        ▼
  Visualization (True Color, FAI, NDVI, NIR, Bloom Map)
```

---

## 📊 Output Visualizations

The notebook generates a 6-panel figure:

| Panel | Description |
|-------|-------------|
| True Color Image | RGB composite of the study area |
| Water Bodies | Binary water mask (NDWI-based) |
| Floating Algae Index | FAI values over water pixels |
| NDVI | Vegetation index over water |
| NIR Band | Near-infrared reflectance |
| **Algal Bloom Detection** | Final map — Red=Bloom, Blue=Clean Water, Brown=Land |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- A [Google Earth Engine](https://earthengine.google.com/) account
- A Google Cloud project with the Earth Engine API enabled

### Installation

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/algal-bloom-detection.git
cd algal-bloom-detection

# Install dependencies
pip install -r requirements.txt
```

### Running the Notebook

**Option 1 — Google Colab (Recommended)**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

1. Upload `algal_bloom_detection_clean.ipynb` to Colab
2. Run the setup cell to install `earthengine-api` and `geemap`
3. Authenticate with GEE when prompted

**Option 2 — Local Jupyter**

```bash
jupyter notebook algal_bloom_detection_clean.ipynb
```

### Step-by-Step Usage

1. **Authenticate** with Google Earth Engine (first run only)
2. **Set date range** — modify `start_date` and `end_date` in Cell 6
3. **Draw your AOI** — use the map drawing tools to select a region of interest
4. **Download bands** — run the download cell (saves GeoTIFF files to `sentinel2_bands/`)
5. **Run detection** — execute the bloom detection cell to generate the output map

---

## 📁 Project Structure

```
algal-bloom-detection/
├── algal_bloom_detection_clean.ipynb   # Main notebook
├── requirements.txt                    # Python dependencies
├── .gitignore                          # Files to exclude from Git
└── README.md                           # This file
```

> **Note:** The `sentinel2_bands/` folder (downloaded GeoTIFF files) is excluded from Git via `.gitignore` as files can be large.

---

## 🧪 Key Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `start_date` | `2022-01-01` | Start of imagery window |
| `end_date` | `2022-01-31` | End of imagery window |
| `cloud_filter` | `< 20%` | Maximum cloud cover allowed |
| `scale` | `100 m` | Download resolution (lower = larger files) |
| `n_clusters` | `4` | K-Means cluster count |
| `ndwi_threshold` | `0.2` | Water body detection threshold |

---

## 📦 Dependencies

| Library | Purpose |
|---------|---------|
| `earthengine-api` | Google Earth Engine access |
| `geemap` | Interactive GEE maps |
| `rasterio` | GeoTIFF reading |
| `numpy` | Array operations |
| `scikit-learn` | K-Means clustering |
| `matplotlib` | Visualization |
| `scipy` | Morphological operations on masks |

---

## ⚠️ Notes

- Replace `'your-project-id'` in the notebook with your actual GEE project ID
- Large regions are automatically split into tiles during download
- For best results, choose dates with low cloud cover and calm weather

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [Google Earth Engine](https://earthengine.google.com/) for satellite data access
- [ESA Copernicus Programme](https://www.copernicus.eu/) for Sentinel-2 imagery
- [geemap](https://geemap.org/) by Qiusheng Wu for the interactive mapping interface