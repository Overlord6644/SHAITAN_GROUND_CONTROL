# SHAITAN Ground Control Station (GCS)

Ground Control Station software for the SHAITAN rocket project, providing real-time telemetry visualization, 3D orientation tracking, GPS mapping, and data logging.

## Features

### Real-Time Telemetry Display
- Serial communication with rocket via COM port (115200 baud)
- Live display of all telemetry data including:
  - Linear and absolute acceleration (3-axis)
  - Gyroscope data (3-axis)
  - Euler angles and quaternions
  - Altitude (GPS and atmospheric)
  - GPS data (position, speed, heading, satellites)
  - Battery voltage and pyrotechnic channel status
  - Environmental data (pressure, temperature)

### 3D Orientation Visualization
- Real-time 3D rendering of rocket orientation using OpenGL
- STL model display (AIM-9 Sidewinder missile model)
- Quaternion-based orientation updates
- Visual axes representation (X: Red, Y: Green, Z: Blue)

### 3D GPS Mapping
- Interactive 3D map powered by Cesium.js
- Real-time rocket position tracking
- Flight path visualization
- Terrain integration with world elevation data
- Customizable default location

### Real-Time Graphs
- Live plotting of telemetry data over time
- Multiple synchronized plots:
  - Linear Acceleration (X, Y, Z)
  - Absolute Acceleration (X, Y, Z)
  - Gyroscope (X, Y, Z)
  - Altitude (GPS and Atmospheric)
  - GPS Speed
- 10-second rolling window for optimal visualization

### IMU Calibration Monitor
- Dedicated calibration display mode
- Real-time calibration status for:
  - System calibration
  - Gyroscope calibration
  - Accelerometer calibration
  - Magnetometer calibration
- Configuration and offset display

### Data Logging
- Automatic logging of all received telemetry
- Timestamped log files in `Log/` directory
- Persistent storage for post-flight analysis

### Launch Controls
- **Launch** button: Sends launch command to rocket
- **Simulate Launch** button: Generates fake telemetry for testing
- **Restart** button: Resets rocket (only available in Recovery state)

### Additional Features
- Automatic COM port detection and refresh
- RSSI (Received Signal Strength Indicator) monitoring
- SNR (Signal-to-Noise Ratio) display
- State machine monitoring
- Continuous widget updates
- Multi-threaded serial reading for responsiveness


### Cesium Token Configuration
1. Copy `config.example.json` to `config.json`:
   ```bash
   copy config.example.json config.json
   ```

2. Get a free Cesium Ion access token:
   - Go to [https://cesium.com/ion/signup](https://cesium.com/ion/signup)
   - Create a free account
   - Navigate to "Access Tokens" in your dashboard
   - Copy your default access token

3. Edit `config.json` and replace `YOUR_CESIUM_TOKEN_HERE` with your token:
   ```json
   {
     "cesium_token": "your_actual_token_here"
   }
   ```

**Note:** The `config.json` file is gitignored to keep your token private. Never commit your actual token to version control.

### 3D Model
Place the `AIM-9_Sidewinder.stl` file in the same directory as the script for 3D orientation visualization.

## Rocket Firmware

This Ground Control Station is designed to work with the SHAITAN rocket avionics system. The rocket firmware is available at:

**[https://github.com/Overlord6644/SAS-shaitanAv](https://github.com/Overlord6644/SAS-shaitanAv)**

The firmware handles:
- Sensor data acquisition (IMU, barometer, GPS)
- State machine management
- Telemetry transmission via LoRa
- Flight control and recovery system deployment

## License

MIT License - see [LICENSE](LICENSE) file for details

Copyright (c) 2025 Noé Renevey

## Telemetry Format

The GCS expects telemetry messages in CSV format starting with `Telem:`:

```
Telem:<packet_number>,<timestamp_ms>,<state>,<sensor_data>,...
```

RSSI and SNR are transmitted separately:
```
RSSI: <value> dBm, SNR: <value> dB
```

Calibration messages use key-value pairs:
```
State:IMUCALIB Sys: <val> Gyro: <val> Accel: <val> Mag: <val>
```
