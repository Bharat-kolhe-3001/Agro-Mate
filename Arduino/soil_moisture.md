
# 🌱 Soil Moisture Sensor – In-Depth Explanation (with Percentage Conversion)

## 🔍 Overview

The **Soil Moisture Sensor** is a **capacitive or resistive analog sensor** used to detect water content in soil. It's widely used in **smart agriculture**, **automated irrigation**, and **plant health monitoring**.

Your project uses this sensor to:

- Measure how dry or wet the soil is
- Convert this data into percentage
- Send the result to an ESP32 module, which forwards it to a server for analysis or dashboard display

## 🧩 Types of Soil Moisture Sensors

There are two common types:

| Type         | Description |
|--------------|-------------|
| **Resistive** | Two exposed metal probes; measures conductivity. Cheap but corrodes quickly. |
| **Capacitive** | Uses capacitive sensing. Longer lifespan, better accuracy. No corrosion. |

> You’re likely using a **resistive sensor**, as it connects to analog pin A0 and is common in basic setups.

## ⚙️ Pin Configuration in Your Project

### Arduino Side

```cpp
#define MOISTUREPIN A0
```

| Sensor Pin | Arduino Pin | Function |
|------------|-------------|----------|
| VCC        | 5V          | Power supply |
| GND        | GND         | Ground |
| AO         | A0          | Analog Output – returns soil moisture level as voltage |

## 📈 How It Works

- The sensor measures **voltage resistance** (resistive type) or **capacitance** (capacitive type) between probes.
- **More water in soil → more conductivity → lower analog value**
- **Dry soil → less conductivity → higher analog value**

## 🔁 Raw Value to Percentage Conversion

### Raw Value Range (from `analogRead()`)

- **0** = fully wet
- **1023** = fully dry

### 💡 Moisture % Formula

```cpp
float moisturePercent = 100 - ((moisture / 1023.0) * 100);
```

This inversely scales the sensor reading into a percentage range where:

| Raw Value | Moisture % | Interpretation     |
|-----------|------------|--------------------|
| 0         | 100%       | Water-saturated soil |
| 300       | ~70%       | Moist soil |
| 600       | ~41%       | Semi-dry |
| 850       | ~17%       | Quite dry |
| 1023      | 0%         | Bone dry |

## 🛠️ Updated Arduino Code Snippet

```cpp
int moisture = analogRead(MOISTUREPIN);
float moisturePercent = 100 - ((moisture / 1023.0) * 100);

if (!isnan(temperature) && !isnan(humidity)) {
  
  Serial.print(",{\"moisture\":");
  Serial.print(moisturePercent, 2);
  Serial.println("}");
}
```

This outputs a **structured JSON string** like:

```json
{"temperature":25.6,"humidity":42.4,"moisture":63.71}
```

## 📤 UART Communication with ESP32

- The **Arduino sends this JSON string over UART**.
- The **ESP32 receives it on Serial2** (GPIO16 as RX) and:
  - Validates connection to WiFi
  - Sends it as an HTTP POST request to your backend server

### ESP32 UART Code Snippet (from your sketch)

```cpp
if (Serial2.available()) {
  String jsonData = Serial2.readStringUntil('\n');
  ...
  http.POST(jsonData);
}
```

## 📚 Real-World Use Cases

| Use Case                   | How Sensor Helps                         |
|----------------------------|------------------------------------------|
| **Smart Irrigation**       | Automatically water plants when dry      |
| **Precision Farming**      | Monitor field zones for crop health      |
| **Indoor Gardening**       | Keep optimal moisture for houseplants    |
| **Agritech Dashboards**    | Visualize moisture trends on a server UI |
| **Alert Systems**          | Trigger alerts when soil gets too dry    |

## 🧪 Pro Tip: Moisture Status Labeling

To make your data more human-readable, you can **map % ranges to moisture levels**:

```cpp
String status;
if (moisturePercent > 75) status = "Very Wet";
else if (moisturePercent > 50) status = "Moist";
else if (moisturePercent > 25) status = "Dry";
else status = "Very Dry";
```

Then include `status` in your JSON if needed:

```cpp
Serial.print(",\"status\":\"");
Serial.print(status);
Serial.println("\"}");
```

## 🛡️ Sensor Tips & Maintenance

- Avoid submerging the entire sensor—just the probe tip
- Calibrate the sensor in your actual soil (dry/wet)
- Clean the sensor regularly for accurate results
- Use capacitive sensors for long-term use (no corrosion)

## ✅ Summary

- The Soil Moisture Sensor provides **critical environmental input**
- You convert raw analog data into a **percentage scale**
- Data is formatted into **JSON**, sent via **UART** to ESP32, and pushed to your backend for analysis or automation
