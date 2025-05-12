### Overview
In this section, i will describe the type of Dataset we need with their respective metadata and parameters, in addition, i explain the intended use of each dataset. 

### 1. Dataset: Standard Weld Surface Dataset

**Description**:  
This dataset contains raw images of weld surfaces captured under standard conditions using the Renault camera system.

**Constitution Process**:
- Data collected from production line cameras at regular intervals.
- Images labeled by experts as `OK`, `KO`, or `Unknown`.
- Preprocessing includes:
  - Resizing to 224x224 pixels.
  - Normalization of pixel intensities.
  - Basic denoising and contrast enhancement.
- Metadata attached to each sample:
  - Camera ID
  - Timestamp
  - Operator ID
  - Manual quality label

**Parameters**:
- Image resolution: 224x224
- Capture frequency: 1 frame per part
- Label types: binary or ternary (`OK`, `KO`, `Unknown`)

#### Intended Use

**Modeling Phase**:
- Used for training and validation of the base classifier (e.g., YOLO, ResNet).
- Ground truth for performance evaluation (F1-score, recall, etc.)

**Post-Processing Phase**:
- Used to validate the calibration of uncertainty estimation modules.
- Supports training of explainability tools by correlating model attention with labeled defect regions.


### 2. Dataset: Flash-Affected Weld Images

**Description**:  
This dataset simulates welding surface images impacted by glare or flash from the camera — for robustness testing and OOD detection.

**Constitution Process**:
- Base images taken from the standard dataset.
- Synthetic glare/flash overlays applied using image processing (e.g., Gaussian flare, lens distortion).
- Preprocessing includes:
  - Region-of-interest labeling for glare.
  - Retaining original quality labels.

**Parameters**:
- Flash intensity (0.5 to 1.0 scale)
- Flash location (random or localized near weld seams)
- Number of synthetic variants per original: 3

**Metadata**:
- Original image ID
- Flash coordinates
- Flash intensity level
- Label consistency flag

#### Intended Use

**Robustness Testing**:
- Evaluates model stability under extreme lighting.
- Used to train and test robustness modules.

**OOD Detection**:
- Flash-affected samples should ideally be flagged as out-of-distribution.

### 3. Dataset: Tagged KO Samples

**Description**:  
Subset of `KO` images with detailed defect-type annotations (e.g., porosity, cracking, incomplete fusion).

**Constitution Process**:
- Manual tagging of defect types by experts.
- Optionally supported with image segmentation masks or bounding boxes.
- Defect heatmaps may be generated for visual interpretability.

**Parameters**:
- Defect type (categorical)
- Severity level (1–5 scale)
- Location on surface (bounding box or polygon mask)

**Metadata**:
- Defect tag
- Severity score
- Bounding box coordinates

#### Intended Use

**Explainability & Trust**:
- Helps evaluate if the model can correctly localize and justify KO predictions.
- Serves as ground truth for saliency map comparison.

**Training Sub-Models**:
- Enables future models to predict not just binary quality but defect type/severity.


### 4. Dataset: Rotated Weld Surfaces

**Description**:  
This dataset includes original welding images augmented by rotation (±15°, ±30°, 90°, etc.) to test model rotational invariance.

**Constitution Process**:
- Original labeled images used as base.
- Rotated using affine transformations.
- Labels retained.
- May include minor cropping artifacts or black padding to simulate real-world camera misalignment.

**Parameters**:
- Rotation angles: [-30°, -15°, 0°, +15°, +30°, +90°]
- Retain original resolution or crop to center

**Metadata**:
- Original image ID
- Applied rotation (degrees)
- Crop vs. pad flag

#### Intended Use

**Robustness Evaluation**:
- Checks if the model prediction remains stable under image rotations.
- Trains the model with augmented orientations if needed.
- Useful for deploying systems with variable camera angles.

### 5. Dataset: Drifted Input Image Dataset

**Description**:  
This dataset simulates real-world visual drift in image characteristics due to changes in camera parameters, lighting conditions, environment dust, or wear over time. These are **input-side drifts**, not shifts in the label distribution.

**Constitution Process**:
- Generate variants of clean images with synthetic or captured image distortions such as:
  - Color temperature changes (warmer/cooler tones)
  - White balance shifts
  - Blur (from focus drift or camera vibration)
  - Brightness variation due to ambient light
  - Image compression artifacts or reduced resolution

**Parameters**:
- Brightness factor: ±20%
- Blur radius: Gaussian kernel sizes (3–11)
- Color drift: shift RGB balance (e.g., +10 red)
- Resolution downsampling: original to 128x128 then upsampled
- Compression: JPEG at 60–90 quality factor

**Metadata**:
- Original image ID
- Type of drift applied
- Drift intensity
- Is synthetic or real

### Intended Use

**Robustness and Monitoring**:
- Evaluate if the model remains accurate when facing unseen image-level drift.
- Test model sensitivity and uncertainty when input distribution slowly shifts over time.
- Use in UQ or drift detection pipelines to identify "off-nominal" inputs without needing new labels.
- Critical for systems expected to run over months without hardware recalibration.


Other Datasets like could be use like: Partially Occluded Welds or Sensor Noise Simulation

**Purpose**: Add Gaussian, salt-and-pepper, or motion blur noise to simulate sensor degradation or movement.

**Method**:
- Overlay masks or random blocks
- Retain ground truth
- Apply synthetic noise to standard dataset
- Keep labels

**Use**:
- Explainability and anomaly response evaluation
- OOD detection, robustness modules

