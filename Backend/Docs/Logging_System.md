# Improved Logging System

## 📋 Overview

The logging system has been completely redesigned to provide cleaner, more organized, and controllable logs. The new system replaces verbose `console.log` statements with a centralized logger with different verbosity levels.

## 🎯 Benefits of the New System

### ✅ **Clean and Organized Logs**
- **No spam**: Unnecessary logs have been removed
- **Structured**: Information organized in JSON
- **Colored**: Different colors for different levels
- **Context**: Additional information in each log

### ✅ **Configurable Log Levels**
- **error**: Critical errors (red)
- **warn**: Important warnings (yellow)
- **info**: General information (cyan)
- **debug**: Technical details (gray)

### ✅ **Module-Specific Logs**
- **Bot**: 🤖 Discord bot logs
- **Server**: 🖥️ Server logs
- **Vehicle**: 🚗 Vehicle logs
- **User**: 👤 User logs
- **Webhook**: 📡 Webhook logs
- **Database**: 💾 Database logs
- **Config**: ⚙️ Configuration logs

## 🔧 Configuration

### Configuration File
```javascript
// src/config/logger.config.js
module.exports = {
    level: 'info',           // Log level
    file: 'src/data/logs/app.log',  // Log file
    maxSize: 10 * 1024 * 1024,     // 10MB
    maxFiles: 5,                    // 5 backup files
    colors: true,                   // Colors in console
    debug: false                    // Debug logs
};
```

### Environment Variables
```bash
LOG_LEVEL=info              # error, warn, info, debug
LOG_FILE_PATH=logs/app.log  # File path
LOG_MAX_SIZE=10485760       # Maximum size (10MB)
LOG_MAX_FILES=5            # Number of backups
```

## 📝 How to Use

### Import Logger
```javascript
const logger = require('./src/logger');
```

### Basic Logs
```javascript
logger.error('Critical error');
logger.warn('Important warning');
logger.info('General information');
logger.debug('Detailed debug');
logger.success('Operation completed');
```

### Specific Logs
```javascript
// Discord Bot
logger.bot('Bot connected');

// Server
logger.server('Server started');

// Vehicles
logger.vehicle('Vehicle registered');

// Users
logger.user('User linked');

// Webhooks
logger.webhook('Message received');

// Database
logger.database('Data saved');

// Configuration
logger.config('Config updated');
```

### Command Logs
```javascript
// Command executed
logger.command('rv', 'Pedreiro', '76561198040636105', '11005', {
    vehicleType: 'RANGER'
});

// Vehicle registration
logger.registration('Vehicle', '11005', 'Pedreiro', '76561198040636105');

// Command error
logger.commandError('rv', new Error('Vehicle already registered'), {
    vehicleId: '11005'
});
```

### Linking Logs
```javascript
// Link created
logger.linking('created', '76561198040636105', '123456789012345678');

// Link updated
logger.linking('updated', '76561198040636105', '123456789012345678');
```

### Webhook Logs
```javascript
logger.webhookMessage('Chat_in_Game', '🎯 Pedreiro (76561198040636105): /rv 11005 RANGER');
```

### Status Logs
```javascript
logger.serverStatus('Online', { players: 15, maxPlayers: 64 });
```

### Performance Logs
```javascript
logger.performance('Command processing', 150, { command: 'rv' });
```

### Cooldown Logs
```javascript
logger.cooldown('76561198040636105', 'rv', 25);
```

### Denunciation Logs
```javascript
logger.denunciation('created', '11005', 'Pedreiro', '76561198040636105', {
    location: '{X=123 Y=456 Z=789}'
});
```

### Permission Logs
```javascript
logger.permission('verified', 'Pedreiro', 'Staff', {
    action: 'denunciation_verify'
});
```

## 📊 Output Example

### Console (Colored)
```
🤖 Discord Bot connected
🖥️ Server started on port 3000
🚗 Vehicle registered successfully
👤 User linked to Discord
📡 Message received via webhook
💾 Data saved to database
⚙️ Configuration updated
🎮 Command /rv executed {"player":"Pedreiro","steamId":"7656****6105","vehicleId":"11005"}
```

### Log File (JSON)
```json
{
  "timestamp": "2025-07-20T04:08:52.358Z",
  "level": "info",
  "message": "🤖 Discord Bot connected",
  "context": {}
}
{
  "timestamp": "2025-07-20T04:08:52.378Z",
  "level": "info",
  "message": "🎮 Command /rv executed",
  "context": {
    "player": "Pedreiro",
    "steamId": "7656****6105",
    "vehicleId": "11005",
    "vehicleType": "RANGER"
  }
}
```

## 🔍 Filters and Search

### By Level
```bash
# Only errors
grep '"level":"error"' src/data/logs/app.log

# Only commands
grep '"message":"🎮 Command' src/data/logs/app.log

# Only vehicles
grep '"message":"🚗' src/data/logs/app.log
```

### By Steam ID
```bash
# Logs from a specific player
grep '7656****6105' src/data/logs/app.log
```

### By Vehicle
```bash
# Logs from a specific vehicle
grep '"vehicleId":"11005"' src/data/logs/app.log
```

## 📁 File Structure

```
src/
├── logger.js                    # Logging system
├── config/
│   └── logger.config.js        # Logger configuration
└── data/
    └── logs/
        ├── app.log             # Current log
        ├── app.log.1           # Backup 1
        ├── app.log.2           # Backup 2
        └── ...
```

## ⚙️ Advanced Configurations

### Automatic Rotation
- **Maximum size**: 10MB per file
- **Backups**: 5 backup files
- **Rotation**: Automatic when limit is reached

### Levels by Module
```javascript
modules: {
    bot: { level: 'info', enabled: true },
    server: { level: 'info', enabled: true },
    webhook: { level: 'debug', enabled: true },
    database: { level: 'debug', enabled: true },
    vehicle: { level: 'info', enabled: true },
    user: { level: 'info', enabled: true }
}
```

### Sensitive Data Masking
- **Steam IDs**: `76561198040636105` → `7656****6105`
- **Tokens**: `MTM5NTQ5NjY1NDE1NjU5NTQwNA...` → `***`
- **Discord IDs**: Kept for tracking

## 🚀 Migration from Old System

### Before (Console.log)
```javascript
console.log(`🔍 Processing chat message: ${messageContent}`);
console.log(`✅ Vehicle ${vehicleId} registered automatically`);
console.log(`⚠️ Steam ID ${steamId} already linked`);
```

### After (Logger)
```javascript
logger.debug('Processing chat message', { message: messageContent.substring(0, 100) });
logger.registration('Vehicle', vehicleId, 'automatic', steamId);
logger.linking('already linked', steamId, discordUserId);
```

## 📈 Performance Benefits

### ✅ **Less I/O**
- Structured logs reduce processing
- Automatic rotation prevents huge files
- Efficient filters by level

### ✅ **Improved Debugging**
- Rich context in each log
- Precise timestamps
- Action tracking

### ✅ **Monitoring**
- Specific logs by functionality
- Performance metrics
- Automatic alerts

## 🎯 Next Steps

1. **Implement performance logs** for slow commands
2. **Add alerts** for critical errors
3. **Create real-time** log dashboard
4. **Integrate with** monitoring tools

## 📞 Support

For questions about the logging system:
- Check `src/logger.js` for implementation
- Verify `src/config/logger.config.js` for configuration
- Test with `node test_logger.js` for examples

**The new logging system is ready for use! 🎉** 