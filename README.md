# LSTM-Based Sketch Generation for Robotic Drawing

This project implements a **class-conditional LSTM-based sketch generator** trained on the **Google QuickDraw dataset**.  
The trained model generates 2D sketch trajectories that can be sent to **MATLAB** for **serial manipulator drawing**.

---

## 📌 Overview

The pipeline consists of:

1. **Data Processing (QuickDraw)**
   - Uses only *recognized* sketches
   - Converts raw strokes into `(Δx, Δy)` sequences
   - Pads / truncates to fixed sequence length

2. **LSTM Sketch Generator (PyTorch)**
   - Class-conditioned generation using one-hot labels
   - Predicts next `(Δx, Δy)` given previous strokes
   - Trained using **Huber loss (SmoothL1)**

3. **Inference**
   - Autoregressive generation (no ground truth at test time)
   - Produces `(Δx, Δy)` → converted to `(x, y)` coordinates

4. **MATLAB Integration**
   - Python exports full `(x, y)` trajectory to CSV
   - MATLAB performs inverse kinematics
   - Serial manipulator draws the sketch

---

## 🧠 Model Architecture

- **Input**
  - Class one-hot vector (size = `NUM_CLASSES`)
  - Previous stroke `(Δx, Δy)`

- **Architecture**
  - Linear layer for class embedding
  - Multi-layer LSTM
  - Fully connected output layer

- **Output**
  - Next stroke `(Δx, Δy)`

