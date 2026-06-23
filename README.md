# 🕹️ MASM Surfers: 3D Software Rendering Engine

![Assembly](https://img.shields.io/badge/Language-x86_MASM-blue)
![Architecture](https://img.shields.io/badge/Architecture-32--bit-red)
![Platform](https://img.shields.io/badge/Platform-Win32_API-lightgrey)
![Status](https://img.shields.io/badge/Status-Complete-success)

A fully functional, textured 3D game engine built entirely from scratch in **pure x86 Assembly language**. This project implements a classic Wolfenstein 3D-style raycaster, handling all 3D projection, sprite scaling, collision mathematics, and memory management directly on the CPU via the x87 FPU.

## 🧠 Core Technical Competencies Demonstrated

This project was built to demonstrate deep systems-level programming and an uncompromising understanding of computer architecture. It does not rely on modern graphics libraries (No OpenGL, No Vulkan). Every pixel is calculated mathematically and written to a raw memory buffer.

* **Low-Level Memory Management:** Direct manipulation of a 172,800-pixel raw memory buffer (`frameBuffer`) using highly optimized string instructions (`rep movsd`, `stosd`).
* **FPU Mathematics & Linear Algebra:** Implementation of vector cross-products, trigonometric ray-stepping, and orthogonal distance scaling (PerpDist) to cure the "fisheye" distortion effect.
* **Bitwise Operations & Masking:** Custom software-level alpha-compositing. Engineered a "Wide-Net Color Filter" using bitwise shifts (`shr`, `and`) to dynamically strip MS Paint artifacts and isolate/delete specific RGB thresholds for perfect sprite transparency.
* **Win32 API Integration:** Interfacing natively with Windows subsystem libraries (`user32.lib`, `gdi32.lib`, `winmm.lib`) for message pumping, window management, and asynchronous audio threading.
* **Algorithmic Optimization:** Transitioning between floating-point precision and truncated integer bounds (`fistp` with control word overrides) to achieve lightning-fast grid collision detection without frame-drops.

## 🚀 Engine Features

* **Real-Time Raycasting:** 360-degree textured walls projected from a 2D memory array.
* **Procedural Texture Generation:** Algorithmic generation of brick-and-mortar textures dynamically written into RAM at startup.
* **Sprite Billboarding:** 2D sprites (Treasures, Enemies, Pitfalls) that correctly scale, anchor to the 3D floor plane with gravity math, and always face the camera.
* **Manhattan Distance AI:** Enemy (Wumpus) pathfinding that tracks player coordinates across the integer grid.
* **Post-Processing VFX:** * Real-time trigonometric camera shake (Heartbeat tied to AI proximity).
  * Chromatic Aberration tearing effects via offset memory-pointer blending.
  * Environmental vignetting based on adjacent tile sensors (Stench/Breeze).
* **Asynchronous Audio Engine:** Hardware-interrupt safe audio locks preventing looping glitches during high-load FPU collision blocks.

## 📂 Architecture Overview

The engine strictly adheres to a modular design pattern, separating rendering, logic, and state.

* **`main.asm`**: The Core Game Loop. Handles Win32 message pumping, the master State Machine (Menu, Game, Dead, Win), user input debouncing, and memory initialization.
* **`render.asm`**: The Graphics Pipeline. Executes the ray-stepping loop, sorts depth via the Painter's Algorithm, applies Z-depth shading (Fog), and handles bitwise color-keying for sprites.
* **`shared.inc` / `macros.inc`**: Global scope definitions, exposed prototypes, and inline macro optimizations.

## 🛠️ Build & Installation

This project requires a Windows environment and the MASM32 SDK (or Visual Studio configured for MASM).

1. Clone the repository.
2. Ensure all `.bmp` and `.wav` assets are placed in the root build directory alongside the `.exe`. *(Note: Audio files must strictly be 16-bit PCM WAV format for legacy Win32 API compatibility).*
3. Compile using MASM:
   ```cmd
   ml /c /Zd /coff main.asm render.asm
   link /SUBSYSTEM:WINDOWS main.obj render.obj
