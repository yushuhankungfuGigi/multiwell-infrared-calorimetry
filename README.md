# Multiwell Infrared Calorimetry (UI + Analysis)

This repository contains the **NiceGUI-based control UI** and **analysis scripts** for a multiwell infrared calorimetry workflow used to rapidly high-throughput porosity screening via gas-dosing–induced thermal signals.

# Project
In this study, we developed a multiwell infrared calorimetry platform to measure CO2 adsorption–induced temperature changes in parallel across a wide range of porous materials. The system includes a custom–designed, sealed gas chamber that enables in situ degassing and simultaneous analysis of a large number of samples (up to 96 wells per run). Upon CO2 exposure, the exothermic heat released during adsorption produces localized temperature increases, which are captured in real time across the entire plate using an infrared thermal camera. Beyond qualitative classification, we also explore the semi–quantitative capability of the platform to evaluate adsorption behavior across a class of solid materials, providing a practical proxy for screening both porosity and adsorption performance. 

## What’s here

- `ui.py` — NiceGUI app to control camera, Arduino/pressure, degas/dosing, and thermal capture.
- `distance.py` and `emissivity.py` — tiny helpers to configure the camera via the `gasporosity` layer.
- `calculate_porus.py` — quick analysis/plotting of temperature time series (per well), peak finding, and integrals.
- `data/` and `out/` — placeholders for raw CSV exports and generated plots/reports.

## Requirements

Python 3.10–3.12 recommended.

```
pip install -r requirements.txt
```

> **Note:** This code depends on a private/local package named **`gasporosity`** (e.g., `gasporosity.data_controller.DataController`, `gasporosity.classes.camera.FlirCamera`). You will need to **install or vendor** that package for the UI and helpers to run. If it lives in another repo, add it to `requirements.txt` as a Git URL, or copy the package folder into this project.

## Quickstart

1. (Optional) Create and activate a virtual environment.
2. Install dependencies: `pip install -r requirements.txt`
3. Start the UI:

```
python ui.py
```

This launches NiceGUI (typically http://127.0.0.1:8080). Use the interface to:
- connect the thermal camera,
- select well-plate corners & well counts,
- run degas / dosing,
- and save **temperature CSV** files into `data/`.

4. Analyze a CSV:

```
python calculate_porus.py  # edit the path inside, or adapt to accept CLI args
```

The script normalizes to a chosen blank (default: `8H`), finds peaks, annotates integrals, and shows an interactive figure.

## Tips & Gotchas

- `distance.py` currently calls `FlirCamera.set_emissivity(distance)` — if your camera API has a separate `set_distance(...)`, consider updating that script.
- `calculate_porus.py` has a hard-coded example path in `__main__`. You may want to switch to command-line arguments like:
  ```python
  # calculate_porus.py
  if __name__ == "__main__":
      import sys
      csv = sys.argv[1]
      normalize = sys.argv[2] if len(sys.argv) > 2 else "8H"
      calculate_porus(csv, normalize=normalize)
  ```

## License

MIT (see `LICENSE`). Update the copyright holder.
