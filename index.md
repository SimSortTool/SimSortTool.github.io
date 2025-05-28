---
layout: default
---
<img src="./logo.png" width="300" alt="logo">

[![GitHub](https://img.shields.io/badge/GitHub-Source_Code-blue?logo=github)](https://github.com/SimSortTool/SimSort-Tetrode)

## Overview

**SimSort** is a deep learning–based framework for spike sorting, pre-trained with large-scale biologically realistic simulations of extracellular recordings.

Currently, SimSort supports tetrode (4-channel) recordings. SimSort 2.0for Neuropixels is under development!

---

## How To Install SimSort

### 🔧 Installation (Linux/macOS/Windows)

We recommend installing SimSort in a **conda** environment with **PyTorch + CUDA** for optimal performance. CPU-only inference is supported but significantly slower.

#### Step 1. Clone the repository

```bash
git clone https://github.com/SimSortTool/SimSort-Tetrode.git
cd SimSort-Tetrode
```

#### Step 2. Set up environment

```bash
# Create and activate conda environment
conda create -n SimSort python=3.10 -y
conda activate SimSort
# Install Python dependencies
pip install -r model/requirements.txt
```
> ‼️ Additionally, you need to install PyTorch following the [official setup guide](https://pytorch.org/) depending on your system configuration.

#### Step 3. Download pre-trained models (Manual)

Please manually download the following two zip files:

* [extractor\_bbp\_L1-L5-8192.zip](https://github.com/SimSortTool/SimSort-Tetrode/releases/download/v1.0.0/extractor_bbp_L1-L5-8192.zip)
* [detector\_bbp\_L1-L5-8192.zip](https://github.com/SimSortTool/SimSort-Tetrode/releases/download/v1.0.0/detector_bbp_L1-L5-8192.zip)

Extract both zip files into the folder:

```
SimSort-Tetrode/model/simsort_pretrained/
```

After extraction, the directory structure should look like this:

```
model/simsort_pretrained/
├── detector_bbp_L1-L5-8192/
│   ├── detection_config.yaml
│   ├── detection_aug_config.yaml
│   └── saved_models/
│       ├── checkpoint.pth
│       └── args.yaml
└── extractor_bbp_L1-L5-8192/
    ├── config.yaml
    ├── aug_config.yaml
    └── saved_models/
        ├── checkpoint.pth
        └── args.yaml
```

---

## 🎉 Running SimSort on Your Own Neural Data

Here is the example in `model/SimSort_Tool_Demo.ipynb`.
You can run the Jupyter notebook directly from that location.

### Setp 1. Load Your Recording

SimSort use [SpikeInterface](https://spikeinterface.readthedocs.io/en/stable/) to load neural recordings, supporting common formats such as `.plx`, `.mda`, `.npy`, and binary `.bin` files.

Here’s an example of a `.plx` recording:

```python
import spikeinterface.extractors as se
from task.custom_sorting import SortingTask

recording_file = r'custom_data/4chTetrodeDemoPLX.plx'
recording = se.PlexonRecordingExtractor(recording_file, stream_name='TETWB')
```

> 🔎 For more formats, see: [Supported File Formats](https://spikeinterface.readthedocs.io/en/latest/modules/extractors.html)

---

### Setp 2. Spike Sorting

```python
sorting = SortingTask(recording=recording, verbose=False)
sorting.run()
```

SimSort's sorting pipeline includes:

* **Data Preprocessing**: filtering, normalizing, whitening.
* **Spike Detection**: using a pre-trained detection model.
* **Spike Identification**: feature extraction and clustering with a pre-trained identification model.

---

### Step 3. Save Results

```python
sorting.save_results(output_dir='./simsort_results')
```

This saves the spike waveforms, timestamps, and unit IDs to disk.

---

## 📚 Advanced Usage

For more advanced usage such as model training and benchmarking (available on Linux/macOS or Windows via WSL):

* Training:

  ```bash
  # Train spike detection model
  bash scripts/train_detector.sh

  # Train spike identification model
  bash scripts/train_extractor.sh
  ```

* Benchmarking with Pre-trained Models:

   ```bash
   # Run spike sorting with pre-trained models
   bash scripts/run_sorting.sh
   ```

---

## 📢 Questions?

Feel free to open issues on [GitHub](https://github.com/SimSortTool/SimSort-Tetrode/issues) if you run into any problems!
