---
layout: page
title: QR Demodulation in MIMO Systems
description: Efficient QR Decomposition for MIMO Signal Demodulation
img: assets/img/mimo_main.jpg
importance: 1
category: Selected Projects
related_publications: false
---

## Overview
The project focuses on implementing a **QR Decomposition (QRD)** module, a critical component in modern wireless communication systems, for a 4x4 MIMO receiver. By transforming complex channel matrices into simpler forms, QRD enables efficient Maximum Likelihood (ML) demodulation, enhancing system reliability and throughput in the presence of AWGN (Additive White Gaussian Noise).

The implementation is based on the **Modified Gram-Schmidt algorithm**, which decomposes the channel matrix \(H\) into:  
- **Q**: An orthogonal matrix  
- **R**: An upper triangular matrix  

This approach reduces computational complexity and optimizes signal recovery.

---

## Algorithm: Modified Gram-Schmidt
The **QR Decomposition** process involves:
1. **Vector Normalization**: Calculating Euclidean distances to normalize channel vectors.  
2. **Iterative Updates**: Using the Gram-Schmidt process to orthogonalize and update signal vectors.  
3. **Matrix Generation**: Producing the orthogonal matrix \(Q\) and upper triangular matrix \(R\) for efficient decoding.
<div class="row justify-content-center">
    <div class="col-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/Gram_Schdmit_algo.jpg" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/iter0.jpg" title="Iter 0" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/iter1.jpg" title="Iter 1" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/iter2.jpg" title="Iter 2" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
    <div class="col-sm-6 col-md-4 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/iter3.jpg" title="Iter 3" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>


<div class="caption">
    Iteration Process of Modified Gram-Schmidt Algorithm
</div>

---

## Methodology

1. Fixed-Point Configuration
- **Register Design**: Optimized register bit-widths to balance precision and hardware efficiency.  
- **LUTs for Arithmetic Operations**: Leveraged Look-Up Tables (LUTs) for fast **square** and **square root** calculations, reducing computational delay.
2. Pipelined Architecture
- **Stage Division**: The computation process was divided into multiple stages, enabling parallel operations and minimizing latency.  
- **Resource Sharing**: Efficiently shared resources across pipeline stages to improve utilization.  
- **Throughput Optimization**: Achieved high processing speeds with minimal resource overhead.
<div class="row justify-content-center">
    <div class="col-24 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/hardware_schedule.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>

<div class="caption">
    We precisely arrange each processing unit (i.e. Multiplier, Adder and other) usage to ensure maximum usage in every cycle.
</div>
---
## Block Diagram
<div class="row justify-content-center">
    <div class="col-8 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/QR_Engine_block_diagram.png" title="Full-Width Image" class="img-fluid rounded z-depth-1" style="max-width: 70%;" %}
    </div>
</div>
---
## Results 
**Post-Layout Performance Ranked 3rd** in All Undergraduate Teams  
1. **Area**: 827604.71 um^2  
2. **Latency**: 100161.5 ns  
3. **Power**: 46.8 mW  
