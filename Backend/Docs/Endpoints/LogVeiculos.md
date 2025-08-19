# Vehicle Logs

## Description
Endpoint for processing SCUM vehicle destruction and event logs. The system reads the most recent vehicle log, extracts information about destruction, disappearance, and vehicle inactivity, and returns structured data. Now includes history, duplicate control, and query endpoints.

---

## Duplicate Control
- The system maintains the last processed timestamp in `src/data/vehicles/lastVehicleRead.json`.
- Only events with timestamp greater than the last processed are saved and returned.
- This prevents duplication in history and webhook sending.

## History Database
- All processed events are saved in `src/data/vehicles/vehicles.json`.
- Allows queries, statistics, and complete history of vehicle events.

---

## Endpoints

### 1. Process Vehicle Log
**GET** `/api/LogVeiculos`

Reads the most recent vehicle log, extracts new information, and returns structured data.

#### Operation
1. Finds the most recent vehicle log file (`vehicle_destruction_*.log`)
2. Copies the file to temporary folder
3. Reads content in UTF-16LE (fallback UTF-8)
4. Extracts only new events (timestamp control)
5. Saves to history (`vehicles.json`)
6. Updates duplicate control (`lastVehicleRead.json`)
7. Removes temporary file
8. Returns structured data in JSON

#### Success Response Example
```json
{
    "success": true,
    "message": "Vehicle log processed successfully. 3 new events found.",
    "data": [
        {
            "timestamp": "2025.07.13-04.01.37",
            "event": "Destroyed",
            "vehicleType": "Kinglet_Duster_ES",
            "vehicleId": "1600649",
            "ownerSteamId": "76561198040636105",
            "ownerPlayerId": "1",
            "ownerName": "Pedreiro",
            "location": {
                "x": -311773.969,
                "y": 5480.525,
                "z": 36099.227
            },
            "processedAt": "2025-07-13T05:20:42.658Z"
        }
    ]
}
```

#### No New Events Response Example
```json
{
    "success": true,
    "message": "Vehicle log processed successfully. 0 new events found.",
    "data": []
}
```

---

### 2. Complete History
**GET** `/api/vehicles/history`

Returns all already processed and saved events in the database.

#### Response Example
```json
{
    "success": true,
    "message": "Vehicle history retrieved successfully",
    "data": [
        {
            "timestamp": "2025.07.13-04.01.37",
            "event": "Destroyed",
            ...
        }
    ]
}
```

---

### 3. Vehicles by Owner
**GET** `/api/vehicles/owner/:steamId`

Returns all vehicle events from a specific owner.

#### Response Example
```json
{
    "success": true,
    "message": "Vehicles from owner 76561198140545020 retrieved successfully",
    "data": [
        {
            "timestamp": "2025.07.13-04.01.37",
            "event": "Disappeared",
            ...
        }
    ]
}
```

---

### 4. Vehicle Statistics
**GET** `/api/vehicles/stats`

Returns aggregated statistics of vehicle events.

#### Response Example
```json
{
    "success": true,
    "message": "Vehicle statistics retrieved successfully",
    "data": {
        "totalEvents": 3,
        "eventsByType": {
            "Destroyed": 1,
            "Disappeared": 1,
            "VehicleInactiveTimerReached": 1
        },
        "topOwners": [
            { "steamId": "76561198140545020", "name": "mariocs10", "count": 2 },
            { "steamId": "76561198040636105", "name": "Pedreiro", "count": 1 }
        ],
        "topVehicleTypes": [
            { "type": "Tractor_ES", "count": 2 },
            { "type": "Kinglet_Duster_ES", "count": 1 }
        ]
    }
}
```

---

### 5. Send History to Discord
**POST** `/api/vehicles/send-history`

Sends complete vehicle history to Discord via webhook with beautiful formatting.

#### Parameters (optional)
```json
{
    "limit": 10
}
```

#### Operation
1. Reads entire vehicle history
2. Creates Discord embed with:
   - Statistics by event type
   - Top owners
   - Latest events (limited by parameter)
3. Sends to configured webhook

#### Success Response Example
```json
{
    "success": true,
    "message": "History of 4 vehicles sent to Discord successfully",
    "data": {
        "totalEvents": 4,
        "sentToDiscord": true
    }
}
```

#### Webhook Not Configured Response Example
```json
{
    "success": false,
    "message": "Error sending to Discord or webhook not configured",
    "data": {
        "totalEvents": 4,
        "sentToDiscord": false
    }
}
```

---

## Webhook for LogVeiculos

- To register a webhook for vehicle events:
  - **POST** `/api/webhook/LogVeiculos`
  - Body: `{ "url": "https://discord.com/api/webhooks/YOUR_WEBHOOK" }`
- To query the registered webhook:
  - **GET** `/api/webhook/LogVeiculos`

---

## Notes
- The endpoint only processes new events, never repeats already processed events.
- History is saved in `src/data/vehicles/vehicles.json`.
- Duplicate control is done by timestamp in `src/data/vehicles/lastVehicleRead.json`.
- The system supports UTF-16LE and UTF-8 encoding.
- Game version lines are ignored during processing.
- Coordinates are converted to floating point numbers.

---

## Usage Examples

### Process vehicle log:
```bash
curl http://localhost:3000/api/LogVeiculos
```

### Query history:
```bash
curl http://localhost:3000/api/vehicles/history
```

### Query vehicles by owner:
```bash
curl http://localhost:3000/api/vehicles/owner/76561198140545020
```

### Query statistics:
```bash
curl http://localhost:3000/api/vehicles/stats
```

### Send history to Discord:
```bash
curl -X POST http://localhost:3000/api/vehicles/send-history -H "Content-Type: application/json" -d '{"limit": 10}'
```

### Register webhook:
```bash
curl -X POST http://localhost:3000/api/webhook/LogVeiculos -H "Content-Type: application/json" -d '{"url": "https://discord.com/api/webhooks/YOUR_WEBHOOK"}'
```

## Event Types

The system recognizes the following vehicle event types:

- **Destroyed**: Vehicle was destroyed
- **Disappeared**: Vehicle disappeared
- **VehicleInactiveTimerReached**: Vehicle inactivity timer was reached

## Vehicle Log Structure

The system expects logs in the format:
```
2025.07.13-04.01.15: Game version: 1.0.1.2.96201
2025.07.13-04.01.37: [VehicleInactiveTimerReached] Tractor_ES. VehicleId: 670006. Owner: 76561198140545020 (12, mariocs10). Location: X=-176305.094 Y=-702604.250 Z=1444.222
2025.07.13-04.01.37: [Disappeared] Tractor_ES. VehicleId: 670006. Owner: 76561198140545020 (12, mariocs10). Location: X=-176305.094 Y=-702604.250 Z=1444.222
2025.07.13-04.58.18: [Destroyed] Kinglet_Duster_ES. VehicleId: 1600649. Owner: 76561198040636105 (1, Pedreiro). Location: X=-311773.969 Y=5480.525 Z=36099.227
```

## Response Fields

### Vehicle Event
- **timestamp**: Event date and time (format: YYYY.MM.DD-HH.MM.SS)
- **event**: Event type (Destroyed, Disappeared, VehicleInactiveTimerReached)
- **vehicleType**: Vehicle type/model
- **vehicleId**: Unique vehicle ID
- **ownerSteamId**: Vehicle owner's Steam ID
- **ownerPlayerId**: Player ID on server
- **ownerName**: Vehicle owner's name
- **location**: Vehicle coordinates
  - **x**: X coordinate
  - **y**: Y coordinate
  - **z**: Z coordinate

## Files Used

- **Vehicle log**: `{SCUM_LOG_PATH}/vehicle_destruction_*.log`
- **Temporary folder**: `src/data/temp/`

## HTTP Status Codes

- **200**: Success
- **500**: Internal server error

## Usage Example

### Process vehicle log:
```bash
curl http://localhost:3000/api/LogVeiculos
```

### cURL example:
```bash
curl -X GET http://localhost:3000/api/LogVeiculos \
  -H "Content-Type: application/json" \
  -H "Accept: application/json"
```

## Debug Logs

The system displays console logs for debug:
- Lines not recognized by regex
- Errors copying or reading files
- Processing errors

## Dependencies

- **SCUM_LOG_PATH**: Environment variable with SCUM logs path

## Notes

- The endpoint only processes the most recent log file
- Temporary files are automatically removed after processing
- The system supports UTF-16LE and UTF-8 encoding
- Game version lines are ignored during processing
- Coordinates are converted to floating point numbers 