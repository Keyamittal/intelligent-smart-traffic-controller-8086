# INTELLIGENT SMART TRAFFIC CONTROLLER USING 8086 ASSEMBLY LANGUAGE

A comprehensive 16-bit x86 Assembly project developed for the **Computer Organization and Architecture (COA)** course. This program implements a smart, adaptive traffic signal controller designed to run on the **emu8086** microprocessor emulator, interfacing directly with the virtual Traffic Lights peripheral via I/O Port 4.

---

## 📌 Project Overview

This project models an intelligent 4-way intersection controller (North-South and East-West) with real-time feedback, adaptive timing based on vehicle density, and fail-safe safety mechanisms.

### Key Highlights:
- **Architecture:** 16-bit Real Mode 8086 Assembly (`.COM` format, `ORG 100h`).
- **Target Platform:** emu8086 Microprocessor Emulator.
- **Hardware Interfacing:** Direct I/O port control on **Port 4** driving 12 virtual LED signal lamps (4 directions × Red, Yellow, Green).
- **Real-Time Timing:** System RTC polling using DOS Interrupt `INT 21h, AH=2Ch` with non-blocking user input detection via `INT 21h, AH=06h`.

---

## 🚦 Features

1. **Automatic Adaptive Traffic Control:**
   - Dynamically alternates North-South and East-West phases with a 2-second All-RED clearance buffer between phase changes.
   - Adjusts Green/Yellow light durations according to traffic density.

2. **Traffic Density Categories:**
   - **Low (0–40 vehicles):** 30s Green, 5s Yellow
   - **Medium (41–80 vehicles):** 40s Green, 7s Yellow
   - **High (81+ vehicles):** 45s Green, 10s Yellow

3. **Emergency Vehicle Priority:**
   - Grants an immediate 60-second Green priority wave to a selected direction (North, South, East, or West) while holding all other directions at Red.

4. **Pedestrian Crossing Phase:**
   - Dedicated 45-second All-RED crossing phase.
   - Supports on-the-fly pedestrian request queueing by pressing `P`/`p` during vehicular countdowns.

5. **Night & Low-Traffic Mode:**
   - Toggles power-saving / low-delay timing (forces Low category timings).

6. **Peak & Off-Peak Scheduling:**
   - Selectable schedule modes: Morning Peak, Evening Peak, Normal, and Off-Peak.

7. **Signal Fault Detection & Simulation:**
   - Simulates hardware lamp faults with immediate emergency All-RED lockouts. Blocks normal operation until the fault is explicitly cleared.

8. **Road Closure Management:**
   - Configurable road closures (Close NS, Close EW, or Reopen All) with automatic rerouting of phase cycles.

9. **Manual Signal Control:**
   - Allows an operator to manually trigger NS Green, EW Green, or All-RED states.

10. **Traffic Statistics & Reporting:**
    - Real-time logging of completed cycles, NS/EW phases, pedestrian crossings, emergency operations, and estimated cumulative vehicle counts.

---

## 🔌 Hardware Port Mapping (Port 4)

The virtual Traffic Lights peripheral on **Port 4** utilizes a 12-bit word (`OUT 4, AX`), where each direction is allocated 3 bits (`001b` = Red, `010b` = Yellow, `100b` = Green):

| Signal State | Control Word (`AX`) | Description |
| :--- | :---: | :--- |
| **All-RED** | `0249h` | All 4 directions set to RED (Safety hold) |
| **NS Green** | `0861h` | North & South Green; East & West Red |
| **NS Yellow** | `0451h` | North & South Yellow; East & West Red |
| **EW Green** | `030Ch` | East & West Green; North & South Red |
| **EW Yellow** | `028Ah` | East & West Yellow; North & South Red |
| **Emergency North** | `0261h` | North Green only; all others Red |
| **Emergency South** | `0849h` | South Green only; all others Red |
| **Emergency East** | `024Ch` | East Green only; all others Red |
| **Emergency West** | `0309h` | West Green only; all others Red |

---

## 🛠️ How to Run in emu8086

1. **Download and Install emu8086:**
   - Install the [emu8086 emulator](https://github.com/AhmadNaserTurnkeySolutions/emu8086) on your machine or inside a virtual machine / DOSBox.

2. **Open the Project:**
   - Launch **emu8086**.
   - Open `COA_PRO.asm`.

3. **Launch the Traffic Lights Peripheral:**
   - Click **Compile and Emulate** (or press `F5`).
   - In the emulator window, go to the top menu: **Virtual Devices** $\rightarrow$ **Traffic Lights**.
   - A window displaying a 4-way intersection with 12 traffic lamps will appear.

4. **Run the Program:**
   - Press **Run** (or `F9`).
   - Use the terminal menu to choose operating modes, set density, trigger emergency vehicles, or view live traffic statistics.

---

## 📜 Academic Attribution

- **Project:** Computer Organization and Architecture (COA) Course Project
- **Author:** [Keya Mittal](https://github.com/Keyamittal)
- **Language:** Intel 8086 Assembly Language (NASM / MASM / emu8086 syntax)
