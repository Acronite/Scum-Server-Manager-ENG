# SCUM Server Configuration API

## 📋 Overview

This document describes the `/api/server/config` endpoint that allows getting and updating SCUM server configurations, including Discord bot configurations.

## 🔧 Configuration Endpoint

### Get Current Configuration
**GET** `http://localhost:3000/api/server/config`

Returns all current server configurations, including Discord bot configurations.

**Success Response:**
```json
{
  "success": true,
  "config": {
    "serverPath": "C:\\Servers\\Scum\\SCUM\\Binaries\\Win64",
    "steamCMDPath": "C:\\Servers\\steamcmd",
    "installPath": "C:\\Servers\\Scum",
    "batPath": "C:\\Servers\\start-server-no-pause.bat",
    "port": 8900,
    "maxPlayers": 64,
    "useBattleye": true,
    "autoRestart": false,
    "restartInterval": 3600000,
    "logLevel": "info",
    "checkInterval": 30000,
    "discord_bot": {
      "enabled": true,
      "token": "MTM5NTQ5NjY1NDE1NjU5NTQwNA.GEIINd.JT9KLS2ZPnC4WZr0O4ko8uqR4PdacB7u4tw42Q",
      "guild_id": "1343764652114575513",
      "webhook_key": "Chat_in_Game",
      "channels": {
        "vehicle_registration": "1395477789313994812",
        "vehicle_mount_registration": "1395634763733405756",
        "vehicle_denunciation": "1396238276808937567"
      },
      "features": {
        "vehicle_registration": {
          "enabled": true,
          "command_prefix": "/rv",
          "auto_register": true,
          "cooldown_seconds": 30,
          "embed_color": "#00ff00",
          "notification_channel": "vehicle_registration"
        },
        "vehicle_mount_registration": {
          "enabled": true,
          "command_prefix": "/rm",
          "auto_register": true,
          "cooldown_seconds": 30,
          "embed_color": "#ff8800",
          "notification_channel": "vehicle_mount_registration"
        },
        "vehicle_mount_complete": {
          "enabled": true,
          "command_prefix": "/mc",
          "auto_register": true,
          "cooldown_seconds": 30,
          "embed_color": "#00ff88",
          "notification_channel": "vehicle_registration"
        },
        "vehicle_denunciation": {
          "enabled": true,
          "command_prefix": "/dv",
          "auto_register": false,
          "cooldown_seconds": 60,
          "embed_color": "#ff0000",
          "notification_channel": "vehicle_denunciation",
          "required_roles": ["Staff", "STAFF", "Adm", "ADM"]
        },
        "user_linking": {
          "enabled": true,
          "auto_link": true,
          "link_expiration_hours": 24,
          "link_button_timeout": 300000
        },
        "notifications": {
          "error_notifications": true,
          "success_notifications": true
        }
      }
    }
  }
}
```

### Update Configuration
**PUT** `http://localhost:3000/api/server/config`

Updates server configurations.

**Request Body:**
```json
{
  "port": 8900,
  "maxPlayers": 64,
  "useBattleye": true,
  "autoRestart": false,
  "discord_bot": {
    "enabled": true,
    "token": "YOUR_NEW_TOKEN_HERE",
    "guild_id": "DISCORD_SERVER_ID",
    "webhook_key": "Chat_in_Game",
    "channels": {
      "vehicle_registration": "VEHICLE_CHANNEL_ID",
      "vehicle_mount_registration": "MOUNT_CHANNEL_ID",
      "vehicle_denunciation": "DENUNCIATION_CHANNEL_ID"
    },
    "features": {
      "vehicle_registration": {
        "enabled": true,
        "command_prefix": "/rv",
        "auto_register": true,
        "cooldown_seconds": 30,
        "embed_color": "#00ff00",
        "notification_channel": "vehicle_registration"
      },
      "vehicle_mount_registration": {
        "enabled": true,
        "command_prefix": "/rm",
        "auto_register": true,
        "cooldown_seconds": 30,
        "embed_color": "#ff8800",
        "notification_channel": "vehicle_mount_registration"
      },
      "vehicle_mount_complete": {
        "enabled": true,
        "command_prefix": "/mc",
        "auto_register": true,
        "cooldown_seconds": 30,
        "embed_color": "#00ff88",
        "notification_channel": "vehicle_registration"
      },
      "vehicle_denunciation": {
        "enabled": true,
        "command_prefix": "/dv",
        "auto_register": false,
        "cooldown_seconds": 60,
        "embed_color": "#ff0000",
        "notification_channel": "vehicle_denunciation",
        "required_roles": ["Staff", "STAFF", "Adm", "ADM"]
      },
      "user_linking": {
        "enabled": true,
        "auto_link": true,
        "link_expiration_hours": 24,
        "link_button_timeout": 300000
      },
      "notifications": {
        "error_notifications": true,
        "success_notifications": true
      }
    }
  }
}
```

**Success Response:**
```json
{
  "success": true,
  "message": "Configurations updated successfully",
  "config": {
    // ... updated configurations
  }
}
```

## 📊 Configuration Structure

### Server Configurations
| Field | Type | Description | Default |
|-------|------|-------------|---------|
| `serverPath` | string | Path to server binaries | `C:\Servers\Scum\SCUM\Binaries\Win64` |
| `steamCMDPath` | string | Path to SteamCMD | `C:\Servers\steamcmd` |
| `installPath` | string | SCUM installation path | `C:\Servers\Scum` |
| `batPath` | string | Path to original .bat file | `C:\Servers\start-server-no-pause.bat` |
| `port` | number | Server port (1-65535) | `8900` |
| `maxPlayers` | number | Maximum players (1-100) | `64` |
| `useBattleye` | boolean | Use Battleye anti-cheat | `true` |
| `autoRestart` | boolean | Restart automatically | `false` |
| `restartInterval` | number | Automatic restart interval (ms) | `3600000` |
| `logLevel` | string | Log level | `info` |
| `checkInterval` | number | Status check interval (ms) | `30000` |

### Discord Bot Configurations
| Field | Type | Description |
|-------|------|-------------|
| `enabled` | boolean | Enables/disables the bot |
| `token` | string | Discord bot token |
| `guild_id` | string | Discord server ID |
| `webhook_key` | string | Webhook key for message capture |
| `channels` | object | Discord channel IDs |
| `features` | object | Bot command configurations |

### Discord Channels
| Channel | Description | Example ID |
|---------|-------------|------------|
| `vehicle_registration` | Channel for vehicle registrations | `1395477789313994812` |
| `vehicle_mount_registration` | Channel for mount registrations | `1395634763733405756` |
| `vehicle_denunciation` | Channel for vehicle denunciations | `1396238276808937567` |

### Bot Commands
| Command | Prefix | Description | Cooldown |
|---------|---------|-------------|----------|
| Vehicle Registration | `/rv` | Registers vehicle in system | 30s |
| Mount Registration | `/rm` | Registers vehicle mount | 30s |
| Mount Completion | `/mc` | Completes vehicle mount | 30s |
| Vehicle Denunciation | `/dv` | Denounces unregistered vehicle | 60s |

### Permissions for Denunciations
The following roles can verify denunciations:
- `Staff` (lowercase)
- `STAFF` (uppercase)
- `Adm` (lowercase)
- `ADM` (uppercase)

## 🔄 Features

### Vehicle Commands
- **`/rv <ID> <TYPE>`** - Registers vehicle
- **`/rm <ID> <TYPE>`** - Registers mount
- **`/mc <ID>`** - Completes mount
- **`/dv <ID> <LOCATION>`** - Denounces vehicle

### Denunciation System
- Checks if vehicle is registered
- Shows denouncer information (name, Discord)
- Allows verification by Staff/Adm
- Maintains denunciation history

### User Linking
- Discord ↔ Steam linking system
- Buttons to link accounts
- Automatic link expiration
- Activity history

## 📝 Usage Examples

### Get Configuration
```bash
curl -X GET http://localhost:3000/api/server/config
```

### Update Configuration
```bash
curl -X PUT http://localhost:3000/api/server/config \
  -H "Content-Type: application/json" \
  -d '{
    "port": 8900,
    "maxPlayers": 64,
    "discord_bot": {
      "enabled": true,
      "channels": {
        "vehicle_denunciation": "1396238276808937567"
      }
    }
  }'
```

## ⚠️ Notes

1. **Discord Token**: Token is masked in response for security
2. **Channel IDs**: Use numeric IDs from Discord channels
3. **Permissions**: Configure roles correctly for denunciations
4. **Cooldowns**: Prevent command spam
5. **Webhooks**: Configure via `src/data/webhooks.json` 