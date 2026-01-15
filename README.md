# CNN Accelerator IP based on Eyeriss-Style Systolic Array

## 1. Overview

This project implements a **CNN accelerator IP core** inspired by the **Eyeriss architecture**, using a **systolic array of Processing Elements (PEs)** to efficiently execute convolution operations at RTL level.

The design targets:
- FPGA / ASIC implementation
- Energy-efficient CNN inference
- Research and educational purposes

Core ideas adopted from Eyeriss:
- Spatial dataflow
- Local data reuse
- Systolic propagation of partial sums
- Reduced memory access cost

---

## 2. Eyeriss Architecture Background

Eyeriss is a CNN accelerator architecture proposed to address the high energy cost of data movement in deep neural networks.

Key principles:
- Computation is distributed spatially across PEs
- Weights, activations, and partial sums are reused locally
- Memory accesses are minimized by dataflow-aware mapping

This project follows these principles using a **systolic array architecture**.

---

## 3. Top-Level Architecture

### 3.1 Block Diagram

lua
Sao chép mã
           +----------------------+
Activations ->| |
Weights ->| top.v |--> Output Feature Map
| Systolic PE Array |
| Control Logic |
+----------------------+

yaml
Sao chép mã

The system consists of:
- A 2D systolic array of Processing Elements
- Control logic for data scheduling
- Streaming input interfaces
- Output accumulation logic

---

## 4. Processing Element (PE)

### 4.1 PE Functionality

Each **Processing Element (PE)** performs a **Multiply–Accumulate (MAC)** operation:

psum_out = psum_in + activation × weight

yaml
Sao chép mã

PE responsibilities:
- Multiply input activation with weight
- Accumulate partial sums
- Forward data to neighboring PEs

---

### 4.2 PE RTL Module (`pe_3x3.v`)

Main components:
- Multiplier
- Adder
- Registers for:
  - Activation
  - Weight
  - Partial Sum

Design characteristics:
- Fully synchronous RTL
- Pipeline-friendly
- Synthesizable for FPGA / ASIC
- Supports local data reuse

---

## 5. Systolic Array Dataflow

### 5.1 Data Movement

| Data Type     | Propagation Direction |
|---------------|----------------------|
| Activations   | Horizontal           |
| Weights       | Vertical             |
| Partial Sums  | Local / Forwarded    |

This dataflow enables:
- High parallelism
- Efficient accumulation
- Reduced off-chip memory access

---

## 6. CNN Convolution Mapping

### 6.1 Standard Convolution

For a 2D convolution operation:

O(x,y) = Σ I(x+i, y+j) × W(i,j)

yaml
Sao chép mã

### 6.2 Mapping onto the Systolic Array

- Each PE computes one MAC per cycle
- Partial sums are accumulated across PEs
- Final output feature map values are produced at the array boundary

This mapping follows **Eyeriss-style spatial computation**.

---

## 7. RTL File Structure

.
├── top.v # Top-level CNN accelerator
├── pe_3x3.v # Processing Element (MAC unit)
├── README.md

yaml
Sao chép mã

### 7.1 `top.v`
- Instantiates the PE systolic array
- Controls data flow and timing
- Manages convolution execution phases

### 7.2 `pe_3x3.v`
- Core MAC computation unit
- Handles partial sum accumulation
- Building block of the systolic array

---

## 8. Design Features

- Systolic array based CNN computation
- Local partial sum accumulation
- Modular and scalable PE design
- Suitable for low-power inference
- RTL-level implementation

---

## 9. Applications

- Embedded vision systems
- Edge AI accelerators
- FPGA-based CNN inference
- Research on CNN hardware architectures

---

## 10. Limitations and Future Work

Possible extensions:
- Multi-channel convolution support
- Stride and padding
- Pooling and activation functions (ReLU)
- AXI / DMA memory interface
- Quantized data types (INT8 / INT16)
- Performance and power evaluation

---

## 11. References

1. Y.-H. Chen et al.,  
   **"Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks"**,  
   ISSCC 2016

2. H. T. Kung and C. E. Leiserson,  
   **Systolic Arrays for VLSI**, 1979

---

## 12. License

This project is intended for **academic and research use**.
