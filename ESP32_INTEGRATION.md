# ESP32 Integration

An ESP32 on Bob reads the IMU and keeps track of the boss's health. The game polls it over the local network and stays in sync.

## Endpoints

The ESP32 runs at `172.20.10.2`.

- `GET /status` returns health and force, for example `{"currentNetForce":3.060,"maxForceScore":4.446,"bossHealth":96.583,"bossMaxHealth":100.000}`
- `POST /set_health?value=<0-100>` sets the boss's health

## How the game uses it

- Polls `/status` every 100 ms in a background thread
- Moves the targets whenever health changes, from a camera-detected punch or a hit on Bob
- Resets the ESP32's health when you start a new boss or reset the campaign
- Keeps running on the last known health if the ESP32 goes offline
- Shows the connection status in the bottom-left corner

`test_esp32.py` checks both endpoints and the network latency.
