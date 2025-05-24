---
layout: default
---
<img src="./logo.png" width="300" alt="logo">

[![GitHub](https://img.shields.io/badge/GitHub-Source_Code-blue?logo=github)](https://github.com/SimSortTool/SimSort-Tetrode)

## Overview

SimSort is a deep learning–based framework for spike sorting, pre-trained with large-scale simulated electrophysiology simulations.

Currently, SimSort supports spike sorting for Tetrode (4-channel) recordings. SimSort 2.0 for Neuropixels is on the way!

---

## Getting Started

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/SimSortTool/SimSort-Tetrode.git
   cd SimSort-Tetrode
   ```

2. Set up the environment:
   ```bash
   # Create and activate conda environment
   conda create -n SimSort python=3.10 -y
   conda activate SimSort

   # Install dependencies
   pip install -r model/requirements.txt
   ```

3. Download and set up pre-trained models:
   ```bash
   # Create directory for pre-trained models
   mkdir -p model/simsort_pretrained

   # Download and extract pre-trained models
   cd model/simsort_pretrained
   wget https://github.com/SimSortTool/SimSort-Tetrode/releases/download/v1.0.0/extractor_bbp_L1-L5-8192.zip
   wget https://github.com/SimSortTool/SimSort-Tetrode/releases/download/v1.0.0/detector_bbp_L1-L5-8192.zip
   unzip extractor_bbp_L1-L5-8192.zip
   unzip detector_bbp_L1-L5-8192.zip
   cd ../..
   ```

   > **Important**: After extraction, ensure that the models are in the correct directory structure:
   > ```
   > model/simsort_pretrained/
   > ├── detector_bbp_L1-L5-8192/
   > │   ├── detection_config.yaml
   > │   ├── detection_aug_config.yaml
   > │   └── saved_models/
   > │       ├── checkpoint.pth
   > │       └── args.yaml
   > └── extractor_bbp_L1-L5-8192/
   >     ├── config.yaml
   >     ├── aug_config.yaml
   >     └── saved_models/
   >         ├── checkpoint.pth
   >         └── args.yaml
   > ```

### Basic Usage

Here's a simple example to get you started:

```python
import spikeinterface.extractors as se
from task.custom_sorting import SortingTask

# Input: recording must be a SpikeInterface RecordingExtractor object
# Example using binary data:
recording = se.read_binary(
    file_path='your_data.bin',    # Your raw data file
    num_channels=4,               # For tetrode recordings
    sampling_frequency=30000
)

# Initialize and run spike sorting
sorting = SortingTask(recording=recording,    # RecordingExtractor object
                     verbose=False)
sorting.run()

# Save results
sorting.save_results(output_dir='./simsort_results')
```
See example in `model/SimSort_Tool_Demo.ipynb`.