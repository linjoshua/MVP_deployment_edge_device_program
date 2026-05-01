# CNC Acoustic Monitoring System - Edge Device

Architect an end-to-end IoT pipeline for manufacturing companies' machines with remote-monitor system and ML-based health evaluation. Programs coded in python and deployed on Linux / Windows Operating System.

This repository contains the core Edge Device codebase for the CNC acoustic monitoring system. It handles high-frequency audio capture, real-time feature engineering (RMS/dB calculation), Neural Network (ANN) state inference, and remote data distribution/web visualization via MQTT and WebSockets.

## 🏗️ System Architecture Overview
* **Audio Capture Layer**: A low-latency audio pipeline built on GStreamer.
* **Data Processing & Inference**: Real-time dB SPL calculation and machine state analysis using TensorFlow/Keras.
* **Communication & Storage**: Publishes high-value data via MQTT, pushes real-time spectrograms via WebSockets, and automatically syncs audio files to the cloud using Rclone.

---

## 🛠️ Prerequisites

On a brand new Windows Edge Device, please install the following foundational software:

1. **Miniconda (Python Environment Management)**
   * Go to the official website and download the Windows 64-bit installer. Install using default settings.
2. **Eclipse Mosquitto (MQTT Broker)**
   * Download the Windows 64-bit installer (`.exe`).
   * Take note of the installation path (usually defaults to `C:\Program Files\mosquitto`).
3. **GStreamer (Low-level Audio Engine)**
   * Download the Windows MSVC 64-bit installer.
   * **⚠️ Critical**: You must download and install BOTH the "Runtime" and "Development" installers. Choose "Complete" installation when prompted.
4. **Rclone (Cloud Sync Tool)**
   * Download the Windows - Intel/AMD - 64 Bit version.
   * Extract the file and copy `rclone.exe` to this project's root folder (e.g., inside `ultimate_version`).

---

## 📦 Environment Setup

Open the **Anaconda Prompt** and execute the following commands sequentially to build the core environment:

1. Create and activate the virtual environment**
```bash
conda create -n gst311 python=3.11 -y
conda activate gst311

2. Install GStreamer bridge packages

```bash
conda install -c conda-forge pygobject gstreamer gst-plugins-base gst-plugins-good -y
3. Install core computing and AI packages

```bash
pip install paho-mqtt numpy websockets tensorflow tf_keras
⚙️ Configuration
1. Rclone Cloud Authorization (Connecting to Purdue Box)
In the command prompt, navigate to the project folder and run:

```DOS
rclone config
Interactive Setup Process:

Enter n (New remote) and name it my_box.

For storage, select box.

Press Enter to leave the next few options blank, and for box_sub_type, enter user.

For Use auto config? select y. A browser window will pop up.

Log in to your Purdue Box account and grant access. Return to the terminal and enter q to quit.

2. MQTT Broker Setup
Create a configuration file (e.g., my_broker.conf on your desktop) and add the following two lines to set the port and allow anonymous connections:

Plaintext
listener 1884
allow_anonymous true
🚀 Official Launch SOP (Daily Operations)
For daily monitoring, open the following three channels (terminals) in order:

Channel 1: Start the MQTT Broker
Open CMD and enter:

```DOS
cd "C:\Program Files\mosquitto"
mosquitto -c "%USERPROFILE%\Desktop\my_broker.conf" -v
Channel 2: Start the MAIN Program (Sensing & AI Inference)
Open Anaconda Prompt and enter:

```bash
conda activate gst311
cd <path_to_your_project_folder>
python -m ultimate_version
Once started, the Web UI (spectrogram and status dashboard) can be accessed via a browser on the same network, e.g., http://10.0.0.5:8000/web.html or via your Tailscale IP.

Channel 3: Start the Cloud Sweeper (Background Upload)
Open another CMD or Anaconda Prompt and enter:

```bash
cd <path_to_your_project_folder>
python cloud_uploader.py
