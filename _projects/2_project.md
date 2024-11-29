---
layout: page
title: FPGA-Accelerated Audio Beat Detection Using Spectral Flux Analysis
description: Developed a music recording and automatic beat detection system on FPGA.
img: assets/img/Onset_Detection.png
importance: 2
category: Selected Projects
giscus_comments: true
---

## Overview
This project implements an Onset Detection Algorithm on an FPGA to enhance audio beat detection in a rhythm game. The algorithm is critical for analyzing audio signals and identifying rhythmic points by detecting sudden increases in signal energy, commonly corresponding to beats or note onsets in music.

The core of the detection process involves dividing audio into small segments, transforming them into the frequency domain using the Fast Fourier Transform (FFT), and calculating the Spectral Flux, which measures the energy differences across consecutive segments. Positive differences in spectral energy indicate potential beats. These are further filtered using a moving average to confirm significant rhythmic events.

---

# Onset Detection Algorithm

The Onset Detection Algorithm is the core component of the FPGA-accelerated audio beat detection system. It identifies rhythmic points in audio by detecting sudden increases in signal energy, which correspond to beats or note onsets in music.

<div class="row justify-content-center">
    <div class="col-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/Onset_Detection.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>
## Steps of the Algorithm

- **Audio Segmentation**  
   The audio signal is divided into small overlapping segments. Each segment represents a window of audio data for analysis.

<div class="row justify-content-center">
    <div class="col-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/Audio_Segmentation.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>

-  **Frequency Domain Transformation**  
   - A **Fast Fourier Transform (FFT)** is applied to each segment, converting the signal from the time domain to the frequency domain.  
   - The frequency components allow us to analyze the energy distribution of the signal.

<div class="row justify-content-center">
    <div class="col-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/FFT.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>

-  **Spectral Flux Calculation**  
   - For each frequency, the energy difference between the current window and the previous window is calculated.  
   - **Positive Differences**: Indicate an increase in energy, potentially corresponding to an onset (beat).  
   - **Negative Differences**: Ignored, as they represent a decrease in energy (decay).

-  **Moving Average Filtering**  
   - A moving average is computed using the energy values from a set of preceding and following windows.  
   - If the energy in the current window exceeds the moving average by a defined threshold, it is marked as a rhythmic point (onset).

- **Storing Onset Data**  
   - Identified onsets are encoded into the audio data by modifying the least significant bit (LSB) of the original 16-bit signal.  
   - This method synchronizes drum hits with the audio while minimizing additional memory usage.

---

## FPGA Implementation
- **System Block Diagram**:  
   The hardware system for FPGA-accelerated audio beat detection is designed as a modular and efficient architecture. It consists of a Top Module that orchestrates system states, including audio recording, processing, and playback, supported by an Audio Module with I²C-driven devices for capturing and outputting audio. Core to the system is the Onset Detection Module, which employs FFT for spectral analysis, a flux calculator for detecting energy changes, and a moving average filter to identify beats. Detected beats are encoded into SRAM for synchronization with gameplay visuals and audio. The design incorporates pipelined processing, fixed-point optimization, and resource sharing to maximize performance and minimize latency, providing a responsive and accurate rhythm detection system.

<div class="row justify-content-center">
    <div class="col-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/Onset_Detection_Top_Module.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>

- **FFT Module**:  
   Processes audio segments to extract frequency-domain data. Uses an 8-point FFT with 256 overlapping windows for efficient signal processing under FPGA resource constraints.
<div class="row justify-content-center">
    <div class="col-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/FFT_butterfly.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>

- **Flux Calculator**:  
   Accumulates and compares spectral flux values across windows to calculate the energy difference.

- **Moving Average Module**:  
   Computes the average energy across multiple windows to filter significant onsets.

- **Memory Optimization**:  
   Uses SRAM to store intermediate FFT results and onset data, ensuring efficient resource usage.
---
## Result

- **Resource Usage**:
<div class="row justify-content-center">
    <div class="col-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/Resource_Usage.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>

- **DEMO Video** \\
[Watch on YouTube](https://www.youtube.com/watch?v=9JwqjSBcTBg&ab_channel=DCLabNTUEE)
<div style="text-align: center;">
    <video width="560" height="315" controls>
        <source src="../assets/video/Beat_the_drum_demo.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
</div>
---