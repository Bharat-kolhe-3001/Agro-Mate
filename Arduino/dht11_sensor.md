
# 🌡️ DHT11 Sensor – Role in Your Project 

## 📍 Where It’s Used

In your **Arduino code**, the DHT11 sensor is responsible for measuring:

- **Temperature** (in °C)
- **Humidity** (in % RH)

These values are then sent as part of a **JSON payload** through **Serial communication (UART)** to the **ESP32**, which forwards the data to a backend server.

## ⚙️ Hardware Pin Configuration

```cpp
#define DHTPIN 2
#define DHTTYPE DHT11
```

- The **DHT11 sensor is connected to Digital Pin 2** on the Arduino.
- This pin is used for both **reading and writing** data (single-wire digital communication).

### DHT11 Pin Mapping (3-Pin Module)

| DHT11 Pin | Connected To      | Description                   |
|-----------|-------------------|-------------------------------|
| VCC       | Arduino 5V/3.3V   | Power supply for sensor       |
| DATA      | Arduino Digital 2 | Data signal pin (DHTPIN)      |
| GND       | Arduino GND       | Ground                        |

> 💡 Make sure to include a **4.7kΩ–10kΩ pull-up resistor** between the **DATA** and **VCC** pins (some breakout modules have this built-in).

## 🔁 Code Flow – DHT11 in Action

### In `setup()`

```cpp
dht.begin();
```

- Initializes the DHT11 sensor so it’s ready to provide data.

### In `loop()`

```cpp
float temperature = dht.readTemperature();
float humidity = dht.readHumidity();
```

- Reads the current temperature and humidity.
- The library takes care of timing and protocol handling.

### Output Format

```json
{"temperature":24.5,"humidity":45.2,"moisture":631}
```

- These values are printed to the serial port in **JSON format**.
- This string is then read by the ESP32 using its `Serial2` UART pins.

## 📦 Why DHT11 Works Well Here

| Reason                         | Benefit in Your Setup                     |
|--------------------------------|--------------------------------------------|
| Digital Output                 | No need for Arduino's analog pins         |
| 3.3V / 5V Compatible           | Works with both Arduino and ESP32         |
| Simple Library (`DHT.h`)       | Easy to code and integrate                |
| JSON-friendly readings         | Integrates cleanly into your ESP32 POST   |

## 🔌 UART Connection Recap (Arduino → ESP32)

### Arduino Serial Output

```cpp
Serial.print("{\"temperature\":");
Serial.print(temperature);
...
```

### ESP32 UART Input

```cpp
String jsonData = Serial2.readStringUntil('\n');
```

- The ESP32 reads the **sensor values as a complete JSON string** from UART2 (`GPIO16` as RX).
- It forwards this string to the server using an **HTTP POST** request.

## ✅ Summary: Sensor-to-Server Flow

1. **Arduino reads DHT11** (Digital Pin 2)
2. Converts data to **JSON**
3. Sends JSON via **Serial UART**
4. **ESP32 receives via UART2**
5. Sends JSON to **backend via HTTP POST**
