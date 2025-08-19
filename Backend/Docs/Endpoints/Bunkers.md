# Bunkers System - SCUM Server Manager

## Overview

The bunkers system has been completely redesigned to include a persistent database that controls the status of all SCUM server bunkers. The system is now intelligent and maintains activation history, coordinates, and activation times.

## Features

### ✅ Persistent Database
- Stores information of all bunkers in `src/data/bunkers/bunkers.json`
- Control of processed files in `src/data/bunkers/lastProcessed.json`
- Avoids unnecessary log reprocessing

### ✅ Intelligent Detection
- Automatically detects active and locked bunkers
- Identifies keycard activations
- Maintains coordinates when available
- Calculates time elapsed since activation

### ✅ Detailed Formatting
- Complete information for each bunker
- Time elapsed since activation
- Precise coordinates
- Update status

## Endpoints

### GET `/api/bunkers/status`

**Description:** Gets current status of all bunkers

**Success Response:**
```json
{
  "success": true,
  "message": "Bunker status retrieved successfully.",
  "data": {
    "active": [
      {
        "name": "A1",
        "status": "active",
        "activated": "00h 00m 00s",
        "coordinates": {
          "x": -348529.312,
          "y": -469201.781,
          "z": 4247.645
        },
        "lastUpdate": "2025.07.15-20.16.51",
        "activationTime": "2025.07.15-20.16.51"
      }
    ],
    "locked": [
      {
        "name": "D1",
        "status": "locked",
        "nextActivation": "21h 53m 18s",
        "coordinates": {
          "x": -537889.562,
          "y": 540004.312,
          "z": 81279.648
        },
        "lastUpdate": "2025.07.15-20.16.51"
      }
    ],
    "lastUpdate": "2025.07.15-20.16.51"
  },
  "logFile": "gameplay_2025.07.15.log"
}
```

### POST `/api/bunkers/force-update`

**Description:** Forces bunker update without sending webhook

**Success Response:**
```json
{
  "success": true,
  "message": "Bunker status updated (no webhook sent).",
  "data": {
    "active": [...],
    "locked": [...],
    "lastUpdate": "2025.07.15-20.16.51"
  }
}
```

## Discord Webhook Format

The webhook sends detailed information in the following format:

```
🏰 Bunker Status - SCUM Server

Active Bunkers:
A1 Bunker
Status: Active
Activated: 00h 00m 00s ago (02h 15m 30s elapsed)
Coordinates: X=-348529.312 Y=-469201.781 Z=4247.645

A3 Bunker
Status: Active
Activated: Via Keycard 00h 30m 15s ago
Coordinates: X=230229.672 Y=-447157.625 Z=9555.422

Locked Bunkers:
D1 Bunker
Status: Locked
Next activation: 21h 53m 18s (updated 01h 45m 20s ago)
Coordinates: X=-537889.562 Y=540004.312 Z=81279.648

C4 Bunker
Status: Locked
Next activation: 21h 53m 18s (updated 01h 45m 20s ago)
Coordinates: X=446323.000 Y=263051.188 Z=18552.514
```

## Database Structure

### File: `src/data/bunkers/bunkers.json`
```json
{
  "A1": {
    "name": "A1",
    "status": "active",
    "activated": "00h 00m 00s",
    "coordinates": {
      "x": -348529.312,
      "y": -469201.781,
      "z": 4247.645
    },
    "lastUpdate": "2025.07.15-20.16.51",
    "activationTime": "2025.07.15-20.16.51"
  },
  "D1": {
    "name": "D1",
    "status": "locked",
    "nextActivation": "21h 53m 18s",
    "coordinates": {
      "x": -537889.562,
      "y": 540004.312,
      "z": 81279.648
    },
    "lastUpdate": "2025.07.15-20.16.51"
  }
}
```

### File: `src/data/bunkers/lastProcessed.json`
```json
{
  "fileName": "gameplay_2025.07.15.log",
  "fileMTimeMs": 1731622611000,
  "processedAt": "2025-07-15T20:16:51.000Z"
}
```

## Detected Log Patterns

### Active Bunkers
- `A1 Bunker is Active. Activated 00h 00m 00s ago. X=-348529.312 Y=-469201.781 Z=4247.645`
- `A3 Bunker Activated 00h 06m 41s ago`
- `C2 Bunker Activated via Keycard 00h 30m 15s ago`

### Locked Bunkers
- `D1 Bunker is Locked. Locked initially, next Activation in 23h 53m 18s. X=-537889.562 Y=540004.312 Z=81279.648`

## Webhook Configuration

To configure Discord webhook, add in file `src/data/webhooks.json`:

```json
{
  "bunkers": "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"
}
```

## Implemented Improvements

1. **Persistent Database**: Information is saved and maintained between restarts
2. **Processing Control**: Avoids reprocessing already analyzed logs
3. **Intelligent Detection**: Automatically identifies new bunkers
4. **Detailed Formatting**: Complete information on Discord
5. **Time Calculation**: Shows time elapsed since activation
6. **Keycard Support**: Detects keycard activations
7. **Precise Coordinates**: Maintains exact bunker location

## Usage Example

```bash
# Get current status
curl http://localhost:3000/api/bunkers/status

# Force update
curl -X POST http://localhost:3000/api/bunkers/force-update
```

The system is now much more robust and provides detailed information about SCUM server bunker status.

## Important Notes

1. **Log File**: API reads the most recent `gameplay_*.log` file
2. **Coordinates**: May be null for simple activated bunkers (no coordinates in log)
3. **Webhook**: Only works if configured in `src/data/webhooks.json`
4. **Encoding**: Native UTF-16LE SCUM support
5. **Performance**: Optimized processing with automatic temporary file cleanup

## Troubleshooting

### Problem: "No gameplay log file found"
**Solution:** Check if `SCUM_LOG_PATH` in `.env` is correct

### Problem: Bunkers don't appear in response
**Solution:** Check if there are bunker logs in the most recent `gameplay_*.log` file

### Problem: Webhook doesn't work
**Solution:** Check if webhook is configured in `src/data/webhooks.json`

---

**Developed for SCUM Server Manager v2.0**  
**Compatible with SCUM v1.0+ logs** 