# cnn-accelerator-on-fpga
# 🧠 CNN Accelerator on FPGA (Zynq SoC)

## 📌 Project Overview

This project implements a Convolutional Neural Network (CNN) inference accelerator on a Xilinx Zynq SoC using hardware–software co-design.

The computationally intensive CNN layers are offloaded to the FPGA Programmable Logic (PL), while the ARM Processing System (PS) handles control, memory management, and preprocessing.

The goal is to demonstrate hardware acceleration of CNN inference and evaluate performance improvement over pure software execution.

---

## 🏗 System Architecture

### Architecture Components

- Processing System (ARM Cortex-A9)
- AXI DMA (Memory ↔ Stream data transfer)
- CNN Accelerator IP (Vitis HLS generated)
- AXI Interconnect
- DDR Memory

### Data Flow

1. Input image is loaded into DDR memory.
2. PS configures AXI DMA.
3. Input data is streamed to CNN accelerator via AXI4-Stream.
4. CNN processes data inside FPGA (PL).
5. Output is streamed back to DDR memory.
6. PS reads classification result.

---

## ⚙ Hardware Design

### 🔹 HLS Design

- Language: C/C++
- Tool: Vitis HLS
- Interfaces:
  - AXI4-Stream (input/output data)
  - AXI4-Lite (control register)
- Optimizations used:
  - Loop pipelining
  - Loop unrolling
  - Array partitioning
  - Fixed-point arithmetic (if used)

Top module:
---

### 🔹 Vivado Block Design

Components used:

- Zynq Processing System
- AXI DMA
- CNN HLS IP
- AXI Interconnect
- Processor System Reset

Clock Frequency: ___ MHz  
Target Board: ___ (e.g., PYNQ-Z2 / ZedBoard)  
Vivado Version: 2022.1  

---

## 💻 Software Design (PYNQ)

The Python control program performs:

- Bitstream loading
- DMA configuration
- Accelerator start
- Output readback
- Performance measurement

Example control sequence:

```python
dma.recvchannel.transfer(out_buffer)
dma.sendchannel.transfer(in_buffer)
cnn.write(0x00, 0x01)
dma.sendchannel.wait()
dma.recvchannel.wait()
