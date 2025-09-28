# SMARTs IOT System + AgroMate Web App


This is the **web dashboard** and backend API server for **AgroMate** — a smart soil health monitoring system. Built with **Next.js**, it displays real-time and historical data from soil sensors (NPK, DHT11, Soil Moisture) stored in **MongoDB**.

![Home](./output-snapshots/Home-Screen.png)


##  System Architecture 

##  Agromate + SMARTs

```mermaid
graph TD

    1767["User<br>External Actor"]
    subgraph 1757["External Systems"]
        1766["Database<br>MongoDB"]
    end
    subgraph 1758["Arduino Firmware<br>Arduino/C++"]
        1765["Sensor &amp; Comm Firmware<br>Arduino/C++"]
    end
    subgraph 1759["Backend Services<br>Node.js/Express"]
        1764["API Server<br>Node.js/Express"]
    end
    subgraph 1760["Web Application<br>Next.js/React"]
        1761["Application Shell<br>Next.js/React"]
        1762["Application Pages<br>Next.js/React"]
        1763["UI Components &amp; Utilities<br>React/JS"]
        %% Edges at this level (grouped by source)
        1761["Application Shell<br>Next.js/React"] -->|renders| 1762["Application Pages<br>Next.js/React"]
        1761["Application Shell<br>Next.js/React"] -->|uses| 1763["UI Components &amp; Utilities<br>React/JS"]
        1762["Application Pages<br>Next.js/React"] -->|uses| 1763["UI Components &amp; Utilities<br>React/JS"]
    end
    %% Edges at this level (grouped by source)
    1767["User<br>External Actor"] -->|interacts with| 1761["Application Shell<br>Next.js/React"]
    1762["Application Pages<br>Next.js/React"] -->|fetches data from| 1764["API Server<br>Node.js/Express"]
    1765["Sensor &amp; Comm Firmware<br>Arduino/C++"] -->|sends sensor data| 1764["API Server<br>Node.js/Express"]
    1764["API Server<br>Node.js/Express"] -->|persists/queries| 1766["Database<br>MongoDB"]


```


##  Getting Started

### 1. Clone & Install

```bash
git clone https://github.com/your-username/agromate.git
cd web-app
npm install
```

### 2. Configure Environment

Create a `.env.local` file:

```
MONGO_URI=mongodb://localhost:27017/agromate
NEXT_PUBLIC_API_BASE=http://localhost:3000
```

### 3. Run the Dev Server

```bash
npm run dev
```


##  Features

*  **Dashboard View**: Real-time NPK + Temp/Humidity + Soil Moisture
*  **Historical Charts**: Line/Bar graphs using Chart.js or Recharts
*  **API Integration**: REST endpoints under `/api`
*  **Data Sync**: Push data from ESP32 or Arduino using HTTP POST


##  Live Dashboard Preview

| Dashboard | Sensor Data History |
|-----------|---------------------|
|![Home](./output-snapshots/DashBoard.jpg)|![Home](./output-snapshots/History.png)|





## ⚙️ Tech Stack Overview

| Technology                                                                                                                     | Description        | Role in Project                                          |
| ------------------------------------------------------------------------------------------------------------------------------ | ------------------ | -------------------------------------------------------- |
| ![Next.js](https://img.shields.io/badge/Next-black?style=for-the-badge\&logo=next.js\&logoColor=white)                         | React framework    | Builds server-side rendered (SSR) & static web dashboard |
| ![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge\&logo=react\&logoColor=%2361DAFB)                | JS library         | Builds dynamic client-side UI components                 |
| ![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge\&logo=node.js\&logoColor=white)                      | JavaScript runtime | Runs the Express-style backend APIs inside Next.js       |
| ![Express.js](https://img.shields.io/badge/express.js-%23404d59.svg?style=for-the-badge\&logo=express\&logoColor=%2361DAFB)    | Web framework      | Handles HTTP requests from sensors and dashboard         |
| ![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge\&logo=mongodb\&logoColor=white)              | NoSQL database     | Stores sensor readings (NPK, DHT11, Moisture)            |
| ![TailwindCSS](https://img.shields.io/badge/tailwindcss-%2338B2AC.svg?style=for-the-badge\&logo=tailwind-css\&logoColor=white) | CSS framework      | Styles dashboard responsively                            |
| ![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge\&logo=github\&logoColor=white)                 | Code hosting       | Hosts repository for collaboration & CI/CD               |
| ![Figma](https://img.shields.io/badge/figma-%23F24E1E.svg?style=for-the-badge\&logo=figma\&logoColor=white)                    | Design tool        | UI mockups & wireframes                                  |




### Modules & Flow

| Module                | Description                                                       |
| --------------------- | ----------------------------------------------------------------- |
| Arduino Firmware      | Reads sensors, sends JSON via Wi-Fi (ESP32)                       |
| API Server (Express)  | Receives sensor data, stores to MongoDB, serves data to dashboard |
| MongoDB               | Persists NPK, temp/humidity, moisture history                     |
| Next.js Web Dashboard | Displays real-time + historical charts to farmers                 |



###  Installation

```bash
git clone https://github.com/your-username/agromate.git
cd web-app
npm install
```

###  Configuration

Create `.env.local`:

```bash
MONGO_URI=mongodb://localhost:27017/agromate
NEXT_PUBLIC_API_BASE=http://localhost:3000
```


###  Running Dev Server

```bash
npm run dev
```

Visit: [http://localhost:3000](http://localhost:3000)

---

##  Features

| Feature                | Description                                          |
| ---------------------- | ---------------------------------------------------- |
|  Real-time Dashboard | Displays live NPK, Temp, Humidity, Moisture data     |
|  Historical Charts   | Shows past trends with line/bar charts (Chart.js)    |
|  REST API           | Sensors POST JSON data, dashboard fetches with GET   |
|  Modular UI         | Reusable card & chart components under `/components` |
|  MongoDB Storage    | Archives all sensor readings for historical analysis |


##  Build & Deploy

```bash
npm run build
npm start
```



##  Developer Notes

* **API + Web are monolithic:** Next.js serves both UI & API.
* **MongoDB models isolated:** Located in `/server/models/`.
* **Easy to extend:** Plug new sensors, add fields to MongoDB schema, and display in charts.



##  Resources

* 📖 [Next.js Documentation](https://nextjs.org/docs)
* 📖 [MongoDB Docs](https://www.mongodb.com/docs/)
* 📖 [Chart.js](https://www.chartjs.org/docs/latest/)
* 📖 [Arduino + ESP32 WiFi](https://randomnerdtutorials.com/esp32-http-post-esp8266/)

