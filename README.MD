# HH-ball-plate

**HH-ball-plate** is a ball-and-plate stabilization system designed to balance a ping-pong ball on a moving platform. This project was developed for our **Physics TFE** (Graduation Project).

The system tracks the ball's position in real-time and adjusts the tilt of the plate using two servomotors to keep it centered.

## 👥 Project Team & Roles

* **Gilles Meunier** — *Lead Software Developer & System Designer*
  * Responsible for the whole codebase (Rust controller & Arduino firmware), architecture design, computer vision, and control loop engineering.

* **Victoria Douchamps** — *Project Coordinator, Structural Builder & Team Chef*
  * Managed project timelines, co-designed and assembled the physical platform, and kept the team energized as the official cook.

* **Eléonore Mernier** — *Structural Designer, Diagram Artist & QA Tester*
  * Focused on the design and assembly of the physical platform, created all technical schematics, and co-led the hardware testing phases.

* **Cyril Asselborn** — *Hardware Manager, Structural Designer & QA Tester*
  * Handled component buying and hardware inventory, co-designed the mechanical structure, and co-led the system testing phases.

---

## 🛠️ Project Architecture

The project is divided into two separate parts working together:

* **`controller-rust/` (The Brain):** This part contains the main application written in Rust. It does the heavy work: receiving the ball's coordinates, calculating the regulation algorithm (PID), and making real-time decisions to counteract gravity.
* **`driver-arduino/` (The Executor):** This part contains the firmware managed with PlatformIO for the Arduino board. Its role is strictly hardware-based: it listens for commands sent by the Rust application via serial communication and instantly applies the angles to the **two servomotors** (X and Y axes).

---

## 📐 Schematics & System Diagrams

This section details the physical construction and the data flow of the system.

### 1. Platform & Mechanical Structure
This diagram shows the physical assembly, the pivot system supporting the plate, and how the two servomotors are linked to control the X and Y axes.

![Platform Mechanical Structure](assets/structure_schema.jpg)

### 2. System Functional Loop
This flowchart maps out the real-time loop, showing how data travels from the physical world to the software and back to the motors.

![Flowchart Ball & Plate System](assets/flowchart.jpg)

---

## 🚀 Features

* **Active Center Stabilization:** Keeps a ping-pong ball perfectly balanced at the center of an inclined plane using real-time physics calculations.
* **Dual-Axis Control:** Independent and synchronized management of the plate's X and Y axes.
* **Advanced PID Regulation:** Custom PID loop optimized to reduce overshoot, handle fast movements, and counteract gravity efficiently.
* **Computer Vision Tracking:** High-speed recognition of the ping-pong ball using robust HSV color filtering to maintain reliable tracking under different lighting conditions.
* **Live Visualization Window:** A dedicated interface displaying the live camera feed with tracking overlays, along with real-time plots showing the target vs. actual angles of the servomotors.
* **High-Frequency Control Loop:** Low-latency pipeline ensuring that frame processing and motor adjustments happen fast enough to prevent the ball from falling.
* **Dynamic Mouse Interaction:** Real-time target control directly from the window (Left-click to add custom waypoints, Right-click and hold to make the target follow the mouse, and Middle-click to reset the target to the center). *(EXPERIMENTAL in branch 'develop')*
* **Automated Unit Testing:** Simple test suites implemented in Rust to validate the core logic, data parsing, and PID behavior. *(EXPERIMENTAL in branch 'feat/tests')*
* **PID Parameter Optimizer:** An integrated tool designed to fine-tune PID coefficients using a simplified physics simulation engine. *Note: The simulation engine is currently very basic and should not be fully trusted for the real setup yet.* *(EXPERIMENTAL in branch 'feat/tests')*
* **Optimized Architecture:** Clear separation between high-performance data processing (Rust) and low-level hardware control (Arduino via PlatformIO).

---

## 🔧 Installation and Setup

### Prerequisites
* A working **Rust** environment (`rustup`).
* **PlatformIO** (Core or VS Code extension) to build and upload the Arduino code.
* **OpenCV** library installed on your system (required for computer vision and ball tracking).
* Basic build tools (like `pkg-config`, `cmake`, or C++ compilers depending on your OS to compile OpenCV with Rust).
* The physical components (plate, 2 servomotors, camera, Arduino board).

### Configuration & WSL Notes
1. **Environment Variables:** Before running the application, make sure to configure the environment variables in a `.env` file inside the `controller-rust/` directory.
2. **WSL Users (Important):** If you run the Rust controller inside WSL (Windows Subsystem for Linux), USB devices are not connected by default. You **must use `usbipd`** to connect both hardware components to WSL:
   * Connect the **camera** so the Rust controller can access the video stream.
   * Once the code is uploaded to the **Arduino**, connect it via `usbipd` so the Rust controller can communicate with it over the serial port.

### Running the Project
1. Upload the code from `driver-arduino` to your Arduino board using PlatformIO.
2. Connect both the Arduino and the camera to your computer (and connect them via `usbipd` if using WSL).
3. Launch the controller application:
   ```bash
   cd controller-rust
   cargo run --release
   ```

---

## 📝 Conclusion

This project was a great practical experience in robotics and engineering. It allowed us to build a complete system from scratch by combining mechanics, coding, and hardware control.

The system works well thanks to a simple division of labor: Rust handles the fast camera tracking with OpenCV, while the Arduino controls the motors. Building it taught us how to solve real-world problems, such as adjusting the camera settings, tuning the PID algorithm, and dealing with unexpected hardware issues—like when our very first servomotor broke down during testing.

Beyond the technical side, this was an amazing team experience. Whether writing code, designing diagrams, building the platform, or testing the system, everyone played an important part (and the great food kept the team motivated!). This project proves that we can work together to bring a complex idea to life, giving us a strong foundation for our future goals.

---
© 2026 Cyril Asselborn, Eléonore Mernier, Victoria Douchamps, Gilles Meunier. All rights reserved.