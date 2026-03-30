# Intelligent Traffic Perception System (ITPS) Road Detection (Counting Station with Classification)

---

# 1 Idea

The goal is the automated capture of traffic flows along a road section using a fixed installed camera. The camera shall be mountable on existing street lighting, and the power supply shall be provided directly via recharging from the street lighting.

The system serves as a flexible alternative to classical counting stations and enables:

- Vehicle counting
- Classification according to Swiss-10
- Direction determination
- Lane-based evaluation

---

# 2 Objective

The system shall:

- reliably detect and count vehicles
- classify them according to Swiss-10
- determine the direction of travel
- deliver robust results under various environmental conditions

---

# 3 Basic Principle

The detection station operates autonomously:

1. Camera continuously captures the scene
2. Vehicles are detected locally
3. Tracking determines the direction of movement
4. Classification is performed per vehicle
5. Results are aggregated and stored

---

# 4 System Architecture

### Edge Components

- Camera (fixed installation)
- AI accelerator (e.g. Hailo)
- local computer (e.g. Raspberry Pi CM5)

### Central Platform

- Data storage
- Model training
- Model management

---

# 5 Functionality

## 5.1 Detection & Classification

- Vehicles are detected using a YOLO model
- Classification is performed according to Swiss-10
- only relevant classes are considered and can be selectively chosen

---

## 5.2 Tracking & Direction Determination

- Vehicles are tracked over multiple frames
- the direction of travel is determined from the movement
- optional: assignment to defined lanes according to configuration

---

## 5.3 Counting

- Vehicles are counted as soon as they pass defined zones or lines
- Multiple counts are prevented by tracking

---

# 6 Data Strategy

Data is specifically collected for training:

- uncertain classifications
- random examples to ensure diversity

---

# 7 Learning Strategy

The individual detection stations capture the camera feed and evaluate the camera images directly through an ML model. Through an ML algorithm such as YOLO, individual vehicles can be detected and tracked.
To continuously improve the capture and evaluation of images, a constant improvement process of the used models shall be sought.

This could look as follows:

1. Selected images are stored (edge cases and average values)
2. Manual classification is performed centrally, manually
3. Data sets are expanded
4. Model is fine-tuned (from a few thousand new data sets)
5. New model is validated with new data and recordings
6. Improved model is deployed back

---

# 8 Model Concept

**Global Model**

A unified, global model is further developed based on all recorded images. The base model of e.g. YOLO is thereby further improved with the different camera angles and optimized for the application.

This model is deployed to every new detection station.

**Local Model**

Locally, the existing models can be further developed. The detection station stores images below a certain confidence level for manual classification. To prevent edge bias, additional average images are stored randomly.

Once these images are manually classified, a model can be optimized specifically for this detection station. This can bring a significant advantage for special viewing angles or road conditions.
The new model is validated and then loaded onto the detection station.

---

# 9 Technical Implementation (Conceptual)

- Camera delivers continuous stream, approx. 16 MPx
- YOLO runs on AI accelerator
- Tracking is performed locally
- Results are aggregated
- Selective data transmission to central infrastructure

Once a detection station has reached the desired accuracy, the data connection could optionally be provided via NB-IOT, i.e. LoRaWAN.

---

# 10 Extension Possibilities

- Finer lane evaluation such as bicycles on sidewalk
- Detection of special vehicles such as tractors, construction machinery, emergency vehicles, etc.
- Additional sensor integration
- Combination of multiple cameras
- Real-time dashboards
- Integration of traffic jam detection
- Extended analysis of vehicle behavior

---
