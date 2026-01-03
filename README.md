✈️ Edge AI: Self-Healing Engine Controller
<div align="center"> <a href="https://introegeai-9vdt2me7swysudy44ctw5i.streamlit.app/" target="_blank"> <img src="https://img.shields.io/badge/🚀Live-Demo-success?style=for-the-badge" alt="Live Demo"> </a> </div>

This project demonstrates an Edge AI Digital Twin that simulates a jet engine's sensor data and uses a TensorFlow Lite model to predict the Remaining Useful Life (RUL). If the AI predicts a failure, the system automatically intervenes to throttle the engine or perform an emergency shutdown.

🌟 Features
Real-time Simulation: Generates synthetic sensor data with drift and noise to mimic engine degradation.

Edge AI Inference: Uses a lightweight tflite model to predict RUL locally.

Self-Healing Logic:

Normal Operation: Engine runs at 100% RPM.

Warning State: If RUL < 40 cycles, the system throttles the engine to 75% RPM.

Critical State: If RUL < 10 cycles, the system triggers an emergency stop (0% RPM).

Interactive Dashboard: Built with Streamlit to visualize telemetry, RUL predictions, and system status.

🧠 Model Architecture & Training
The "Brain" of this application (engine_model.tflite) was trained using a custom synthetic data pipeline to ensure reproducibility and robustness.

1. Synthetic Data Generation
Instead of relying on static datasets, we implemented a physics-based degradation logic to simulate 100 unique engines.

The data generation follows this mathematical logic for 14 different sensors:

Python

# Logic: Base Value + (Drift * Degradation Factor) + Random Noise
base_val = 1000 + (sensor_index * 100)
drift = (sensor_index % 2 * 2 - 1) * 50 * degradation 
noise = np.random.normal(0, 2)

sensor_reading = base_val + drift + noise
Drift: Even-numbered sensors drift upwards, odd-numbered sensors drift downwards.

Degradation: A linear factor from 0 (Healthy) to 1 (Failure).

2. Neural Network Topology
We trained a Regression Neural Network using TensorFlow/Keras to map the 14 sensor inputs to a single output: Remaining Useful Life (RUL).

Shutterstock

Input Layer: 14 Neurons (Normalized Sensor Data)

Hidden Layer 1: 32 Neurons (ReLU Activation)

Hidden Layer 2: 16 Neurons (ReLU Activation)

Output Layer: 1 Neuron (Linear Prediction of RUL)

Loss Function: Mean Squared Error (MSE)

3. Edge Conversion (Quantization)
To simulate deployment on a microcontroller (like an ESP32 or Cortex M4), the trained Keras model was converted to TensorFlow Lite:

Python

converter = tf.lite.TFLiteConverter.from_keras_model(model)
tflite_model = converter.convert()
# Result: A binary file < 5KB suitable for embedded devices
🛠️ Installation
Clone the repository:

Bash

git clone <your-repo-url>
cd edgeAi
Install dependencies:

Bash

pip install -r requirements.txt
Ensure the model is present: Make sure engine_model.tflite is in the root directory of the project.

🚀 Usage
Run the Streamlit application:

Bash

streamlit run app.py
The dashboard will open in your default web browser.

Use the Control Panel on the left to adjust the simulation speed.

Click Start Engine Simulation to begin the monitoring process.

Watch the Live Telemetry and Chart to see how the AI responds to sensor degradation.

📂 Project Structure
app.py: Main application script containing the simulation, UI, and control logic.

engine_model.tflite: The pre-trained TensorFlow Lite model used for inference.

requirements.txt: List of Python dependencies.

README.md: Project documentation.
