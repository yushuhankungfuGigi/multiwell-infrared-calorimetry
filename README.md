# Multiwell Infrared Calorimetry (UI + Analysis)

This repository contains the **NiceGUI-based control UI** and **analysis scripts** for a multiwell infrared calorimetry workflow used to rapidly high-throughput porosity screening via gas-dosing–induced thermal signals.

# Overview
This project implements a **multiwell infrared calorimetry workflow** to rapidly screen porous materials for CO₂ adsorption behavior.  
The system integrates:

- A **NiceGUI-based control dashboard** for hardware coordination (camera, dosing, heating).  
- A **central Python controller** for experiment sequencing, data capture, and file I/O.  
- **Post-analysis scripts** for temperature–time signal analysis, peak detection, and heat integration.  

The full pipeline — *from degas to data visualization* — runs with minimal manual intervention.

## What’s here

```
multiwell-infrared-calorimetry/
│
├── classes/ # Hardware interface modules
│ ├── camera.py # FLIR infrared camera control (PySpin SDK)
│ ├── arduino.py # Gas dosing and pressure monitoring via Arduino
│ ├── pheonix_ii.py # Temperature controller (Phoenix II)
│ └── threads.py # Thread utilities for concurrent control
│
├── data_controller.py # Core orchestrator: manages camera, dosing, heating, CSV writing
│
├── scripts/ # User interface and analysis scripts
│ ├── ui.py # NiceGUI web dashboard for real-time experiment control
│ ├── calculate_porus.py # Analysis: peak detection and integral quantification
│ ├── distance.py # Adjust camera distance parameter
│ ├── emissivity.py # Adjust camera emissivity parameter
│
├── data/ # Placeholder for experimental CSV data
└── out/ # Placeholder for processed plots and reports
```

## Requirements

Python 3.10–3.12 recommended.
`pip install -r requirements.txt`

> **Note:** This code depends on a private/local package named **`calorimetry`** (e.g., `calorimetry.data_controller.DataController`, `calorimetry.classes.camera.FlirCamera`). You will need to **install or vendor** that package for the UI and helpers to run. If it lives in another repo, add it to `requirements.txt` as a Git URL, or copy the package folder into this project.

## Quickstart

1. (Optional) Create and activate a virtual environment.
`python -m venv venv`

Windows: `venv\Scripts\activate`
 
MacOS / Linux: `source venv/bin/activate`

2. Install dependencies:
`pip install -r requirements.txt`

3. Launch the control dashboard
`python scripts/ui.py`

4. The interface runs at http://127.0.0.1:8080. Use the UI to:

- Connect and stream from the FLIR infrared camera
- Define well-plate corners and layout (X×Y wells)
- Control degassing via Phoenix heater
- Start gas dosing cycles via Arduino
- Capture and save temperature CSV files in/data/

5. Run post-analysis:
`python calculate_porus.py`  # edit the path inside, or adapt to accept CLI args
The script normalizes to a chosen blank (default: `8H`), finds peaks, annotates integrals, and shows an interactive figure.


## Tips & Gotchas

- `distance.py` Set object distance in the FLIR camera
- `emissivity.py` Set emissivity value (0–1) for calibration 
- `calculate_porus.py` Plot and quantify adsorption heat signals

## Data workflow

- Degas step — controlled heating viaPheonix II
- Dosing step — Arduino triggers gas injection
- Thermal capture — FLIR camera records temperature field
- Per-well processing — automatic grid mapping and averaging
- CSV logging — temperature vs. time for all wells
- Analysis — normalization, peak detection, heat integration

## Dependencies
Core libraries:
`nicegui`
`numpy`, `pandas`, `scipy`, `plotly`
`opencv-python`
`pyserial`
`PySpin`(FLIR camera SDK)

Install via:
`pip install -r requirements.txt`

## License

MIT (see `LICENSE`). Update the copyright holder.
