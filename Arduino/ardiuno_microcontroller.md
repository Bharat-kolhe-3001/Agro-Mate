
# 🎮 Role of Arduino in This Project

In your IoT architecture, the **Arduino** acts as the **Sensor Interface & Data Pre-Processor**. Think of it as the device responsible for **reading**, **formatting**, and **sending** data. Here's a detailed breakdown:

## 🧠 1. **Sensor Data Acquisition (Main Role)**

The Arduino:

- Reads real-time data from multiple sensors:
  - **DHT11** for temperature & humidity
  - **Soil Moisture Sensor** for soil wetness level
- These sensors are directly connected to the Arduino pins:
  - DHT11 → Digital Pin `2`
  - Soil Moisture AO → Analog Pin `A0`

### Why Arduino?

- Arduino is great at **handling analog inputs** (e.g., moisture sensor on A0)
- Lightweight, low power, perfect for **real-time sensing**

## 🧮 2. **Data Conversion & Preprocessing**

The Arduino converts raw sensor values into **meaningful information**:

| Sensor        | Raw Value | Converted Output       |
|---------------|-----------|------------------------|
| DHT11         | Digital   | Celsius & % Humidity   |
| Soil Sensor   | Analog 0–1023 | Moisture % (0–100%) |

It then:

- **Normalizes moisture value** using the formula:  

  ```cpp
  float moisturePercent = 100 - ((moisture / 1023.0) * 100);
  ```

- Packages the values into a **JSON-formatted string** like:

  ```json
  {"temperature":25.4,"humidity":47.2,"moisture":62.7}
  ```

## 🔁 3. **Serial Communication to ESP32 (UART)**

Once the data is formatted, Arduino sends it to the ESP32 over **UART** (Serial Communication):

- Arduino `Serial.println()` → sends data out of USB (TX)
- ESP32’s `Serial2.readStringUntil('\n')` → receives it via RX pin (`GPIO16`)

### Why not send directly via Arduino?

- Arduino UNO/Nano doesn't support WiFi natively
- Offloading network tasks to ESP32 makes the system modular and efficient

## 🔌 4. **Acts as a Modular Sensor Node**

The Arduino is essentially:

- A **plug-and-play sensor module**
- That continuously **reads data every 5 seconds**
- And passes it to a WiFi-capable device (ESP32)

## 🧭 Real-World Analogy

Imagine this like a weather station:

- **Arduino** = Weather instruments (reading temperature, soil wetness)
- **ESP32** = The weather broadcaster (sending the info to a server/cloud)

## ✅ Summary: Arduino’s Responsibilities

| Function | Description |
|----------|-------------|
| Sensor Interfacing | Connects to DHT11 and Soil Moisture sensor |
| Data Collection | Reads temperature, humidity, and moisture |
| Conversion | Converts raw data to human-readable % and °C |
| Formatting | Creates JSON strings from sensor values |
| Communication | Sends formatted data to ESP32 via Serial UART |
