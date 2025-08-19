# Vehicle Control System - SCUM Server Manager

## Overview

This system automatically monitors registered player vehicles and maintains an updated list of active vehicles per player. When a vehicle is destroyed, disappears, or becomes inactive, it is automatically removed from the player's list.

## Features

- ✅ **Automatic Monitoring:** Processes vehicle events in real-time
- ✅ **Individual Embeds:** Each player has their own Discord embed
- ✅ **Numbered List:** Vehicles organized in numbered list
- ✅ **Dynamic Colors:** Green for players with vehicles, red for without vehicles
- ✅ **Periodic Updates:** Checks for new events every 5 minutes
- ✅ **Duplicate Control:** Prevents processing duplicate events

## System Files

### Main
- `src/vehicle_control.js` - Main control system
- `src/vehicle_control_integration.js` - Server integration
- `src/data/players/player_vehicles.json` - Player and vehicle data
- `src/data/vehicles/lastProcessedEvent.json` - Processed events control

### Input Data
- `src/data/bot/vehicle_registrations.json` - Vehicle registrations
- `src/data/vehicles/vehicles.json` - Vehicle event log
- `src/data/webhooks.json` - Webhook configuration

## How to Use

### 1. Initial Test
```bash
node test_vehicle_control.js
```

### 2. Server Integration
```javascript
const VehicleControlIntegration = require('./src/vehicle_control_integration');

const vehicleControl = new VehicleControlIntegration();
vehicleControl.start(); // Starts monitoring
```

### 3. Available Controls
```javascript
// Force update
vehicleControl.forceUpdate();

// Stop system
vehicleControl.stop();

// Check status
const status = vehicleControl.getStatus();
console.log(status);
```

## Data Structure

### player_vehicles.json
```json
{
  "76561198040636105": {
    "steamId": "76561198040636105",
    "playerName": "Pedreiro",
    "discordUserId": "592132368635265034",
    "activeVehicles": [
      {
        "vehicleId": "11001",
        "vehicleType": "QUAD MOUNTED",
        "status": "active"
      }
    ],
    "lastUpdated": "2025-08-02T20:15:30.000Z"
  }
}
```

## Discord Embeds

### Player with Vehicles
```
🚗 Pedreiro's Vehicles
Current status of your registered vehicles

📊 Summary
Total Vehicles: 3
Last Update: 2025-08-02T20:15:30.000Z

🚙 Active Vehicles
1. 11001 - QUAD MOUNTED
2. 11003 - MOUNTAIN BIKE
3. 11004 - ZE'S BICYCLE
```

### Player without Vehicles
```
🚗 Aqu1n0's Vehicles
Current status of your registered vehicles

📊 Summary
Total Vehicles: 0
Last Update: 2025-08-02T20:15:30.000Z

🚙 Active Vehicles
All vehicles have been lost
```

## Processed Events

The system monitors and processes the following events:
- `Destroyed` - Vehicle destroyed
- `Disappeared` - Vehicle disappeared
- `VehicleInactiveTimerReached` - Inactivity timer reached

## Configuration

### Webhook
The `player-vehicles` webhook must be configured in `src/data/webhooks.json`:
```json
{
  "player-vehicles": "https://discord.com/api/webhooks/..."
}
```

### Check Interval
By default, the system checks for new events every 5 minutes. To change, modify the value in `src/vehicle_control_integration.js`:
```javascript
setInterval(() => {
    // ...
}, 5 * 60 * 1000); // 5 minutes
```

## Logs

The system generates detailed logs:
- Initialization with current records
- Event processing
- Vehicle removal
- Embed updates
- Errors and exceptions

## Maintenance

### Reinitialization
To reinitialize the system with current records:
```javascript
vehicleControl.vehicleControl.initializeFromRegistrations();
```

### Backup
Important files for backup:
- `src/data/players/player_vehicles.json`
- `src/data/vehicles/lastProcessedEvent.json`

## Troubleshooting

### Problem: Embeds don't appear
- Check if webhook is configured correctly
- Check webhook permissions on Discord
- Check error logs

### Problem: Vehicles not being removed
- Check if events are being processed
- Check if `lastProcessedEvent.json` is being updated
- Check processing logs

### Problem: System doesn't start
- Check if all JSON files exist
- Check read/write permissions
- Check dependencies (axios) 