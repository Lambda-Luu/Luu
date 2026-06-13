# 📘 Luu: Professional Virtual Instrumentation Lab

[![Platform: JRE 17+](https://img.shields.io/badge/Platform-JRE--17+-orange?style=flat-square)](#)
[![License: Freeware](https://img.shields.io/badge/License-Freeware-blue?style=flat-square)](#)
[![Status: Beta](https://img.shields.io/badge/Status-Beta-orange?style=flat-square)](#)

**Luu** is a high-performance Integrated Development Environment (IDE) and programming language specifically engineered for **virtual instrumentation, signal processing, and laboratory automation**.

It enables engineers and scientists to build complex industrial control panels and real-time data processing algorithms through a workflow that combines professional visual design with an immutable, safe programming model.

[Download Beta](#) • [Technical Documentation](#) • [Gallery](#)

---

## 🚀 Core Philosophy: "Safety is Performance"

In laboratory environments and high-speed data acquisition, stability is critical. Luu is built from the ground up to eliminate common programming errors that disrupt long-running experiments:

*   **🚫 Zero Null Policy**: The Luu language guarantees the absolute absence of null pointers. If a variable exists, it has a valid value.
*   **⚡ Immutability by Default**: Optimized for processing buffers and signal streams without unpredictable side effects.
*   **📉 Precision Engine**: A math-heavy execution core optimized for high-frequency signal visualization with minimal latency.

---

## 🛠️ Key Features

### 🎨 Industrial-Grade Visual Editor
Design professional interfaces by dragging and dropping components that behave like real hardware:
*   **Precision Plots**: Y-ts and XY graphs with "Blueprint" schematic styles, phosphorus-glow visualization, and technical grids.
*   **Analog Controls**: Knobs, Sliders, and Step-selectors with industrial aesthetics and high-precision feedback.
*   **Technical Displays**: 7-segment Edison displays, VUMeters, PhaseMeters, and LED matrices.

### 📡 Hardware Integration (Hardware-in-the-loop)
Control the physical world directly from your scripts:
*   Native support for **NI-DAQmx** (National Instruments) devices.
*   Direct control of digital and analog I/O lines with simplified syntax: `Sys.DAQmx().writeDigitalLine(...)`.

### 📐 Advanced Mathematical Engine
Built-in modules ready for production without external dependencies:
*   **Sys.DSP**: FFT, STFT, IIR/FIR filters, peak detection, and periodic wave generation.
*   **Sys.Numeric**: Integration, Differentiation, ODE Solvers (RK4), Splines, and Optimization.
*   **Sys.Geo**: 3D Geometry, vectors, 4x4 transformation matrices, and perspective proyections.
*   **Sys.Stats**: Comprehensive descriptive and inferential statistical analysis.

---

## 💻 Language in Action

Luu combines the power of modern functional programming with a clean syntax oriented towards engineering:

---

## 🏗️ Technical Modules

| Module | Core Functionality |
| :--- | :--- |
| **Sys.DSP** | Spectral analysis, Metrology (THD, SNR), and signal filtering. |
| **Sys.Math** | Complex trigonometry, Linear Algebra, and Decibel conversions. |
| **Sys.Numeric** | Calculus, Curve fitting, and Polynomial modeling. |
| **Sys.Geo** | Spatial analysis, Plane/Line intersections, and 3D to 2D projections. |
| **Sys.DAQmx** | Direct I/O control for professional data acquisition hardware. |

---

## 📦 Distribution & License

Luu is distributed as **Freeware**. The application is free to use for personal, educational, and professional purposes.

*   **Cost**: $0 (Free).
*   **Source Code**: The source code is proprietary and owned by **Xenione**. It is not public at this stage of the project.
*   **Runtime**: Distributed as a platform-independent package requiring a Java environment.

---

## 🛠️ System Requirements

*   **Java Runtime Environment (JRE) 17** or superior is required.
*   Optional hardware drivers (e.g., NI-DAQmx for hardware control).
*   Desktop OS: Windows, macOS, or Linux.

---

Developed by **Xenione**.  
*"Transforming signals into insights."*
