# AdminLog Endpoint

## Description
Endpoint for reading and processing the SCUM server administration log. The endpoint reads the most recent admin log file, saves the data in JSON format, and sends new events to Discord via webhook.

## Endpoint
```
GET /api/adminlog
```

## Operation
1. **Reads the most recent admin log** from the configured directory
2. **Processes the content** separating each event into individual lines
3. **Saves to database** (`src/data/admin/adminlog.json`) with organized structure
4. **Sends to Discord** only new events via configured webhook
5. **Duplication control** - doesn't send already processed events

## Parameters
No parameters required.

## Success Response
```json
{
  "success": true,
  "message": "Admin log read successfully",
  "file": "admin_20250714050808.log",
  "data": "\n2025.07.14-05.08.08: Game version: 1.0.1.2.96201\n2025.07.14-05.08.14: Deleting players that haven't logged in for 180 day(s)...\n2025.07.14-05.08.14: Completed in 0.022s for 0 player profiles and 0 players.\n2025.07.14-05.11.46: '76561198040636105:Pedreiro(1)' Command: 'SpawnVehicle BPC_Kinglet_Duster'\n2025.07.14-05.12.12: '76561198040636105:Pedreiro(1)' Command: 'ListPlayers true'"
}
```

## Error Response
```json
{
  "success": false,
  "message": "No admin log file found",
  "data": []
}
```

## Database Structure
The endpoint saves data in `src/data/admin/adminlog.json`:

```json
{
  "file": "admin_20250714050808.log",
  "content": [
    "2025.07.14-05.08.08: Game version: 1.0.1.2.96201",
    "2025.07.14-05.08.14: Deleting players that haven't logged in for 180 day(s)...",
    "2025.07.14-05.08.14: Completed in 0.022s for 0 player profiles and 0 players.",
    "2025.07.14-05.11.46: '76561198040636105:Pedreiro(1)' Command: 'SpawnVehicle BPC_Kinglet_Duster'",
    "2025.07.14-05.12.12: '76561198040636105:Pedreiro(1)' Command: 'ListPlayers true'"
  ],
  "savedAt": "2025-07-14T05:47:53.630Z"
}
```

## Discord Integration
- **Webhook configured** in `src/data/webhooks.json` (key: `"adminlog"`)
- **Automatic sending** of new events to Discord
- **Duplication control** via `src/data/admin/lastAdminLogLine.json`
- **Sending status** returned in server console

## Event Examples
- Administrator commands: `'76561198040636105:Pedreiro(1)' Command: 'SpawnVehicle BPC_Kinglet_Duster'`
- Server information: `Game version: 1.0.1.2.96201`
- System operations: `Deleting players that haven't logged in for 180 day(s)...`

## Configuration
- **Log path**: Configured in `.env` (`SCUM_LOG_PATH`)
- **Discord webhook**: Configured in `src/data/webhooks.json`
- **Control files**: 
  - `src/data/admin/adminlog.json` (saved data)
  - `src/data/admin/lastAdminLogLine.json` (sending control)

## Frontend Usage
```javascript
// Call example
fetch('/api/adminlog')
  .then(response => response.json())
  .then(data => {
    if (data.success) {
      console.log('Log processed:', data.file);
      console.log('Content:', data.data);
    } else {
      console.error('Error:', data.message);
    }
  });
```

## Notes
- The endpoint automatically processes new events
- Each event is sent individually to Discord
- The system maintains control to avoid duplication
- Detailed logs are displayed in server console 