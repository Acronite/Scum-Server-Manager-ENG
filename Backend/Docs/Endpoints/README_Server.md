# 🖥️ SCUM Server Management Endpoint

## Overview

This endpoint allows complete management of the SCUM server through a REST API, including:

- ✅ **Start** the SCUM server
- 🛑 **Stop** the SCUM server  
- 🔄 **Restart** the SCUM server
- 📊 **Monitor** status in real-time
- ⚙️ **Configure** server parameters
- 📱 **Notifications** via Discord

## 🚀 Main Features

### 1. Intelligent Process Detection
- Automatically checks if `SCUMServer.exe` is running
- Updates status every 30 seconds
- Records process PID for precise control

### 2. Dynamic .bat Generation
- Creates temporary .bat file with current configurations
- Allows changing parameters via API
- Maintains compatibility with existing configurations

### 3. Webhook System
- Sends Discord notifications about status changes
- Configured via `src/data/webhooks.json` with `serverstatus` key
- Includes detailed information about actions

### 4. Logs and Monitoring
- Records all start/stop/restart actions
- Maintains error history
- Counts number of restarts

## 📋 Available Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/server/status` | Get current server status |
| `POST` | `/api/server/start` | Start SCUM server |
| `POST` | `/api/server/stop` | Stop SCUM server |
| `POST` | `/api/server/restart` | Restart SCUM server |
| `GET` | `/api/server/config` | Get current configurations |
| `PUT` | `/api/server/config` | Update configurations |

## ⚙️ Configurations

### File: `src/data/server/config.json`

```json
{
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
  "checkInterval": 30000
}
```

### Configurable Parameters

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

## 🔧 How to Use

### 1. Check Status
```bash
curl -X GET http://localhost:3000/api/server/status
```

### 2. Start Server
```bash
curl -X POST http://localhost:3000/api/server/start
```

### 3. Stop Server
```bash
curl -X POST http://localhost:3000/api/server/stop
```

### 4. Restart Server
```bash
curl -X POST http://localhost:3000/api/server/restart
```

### 5. Update Configurations
```bash
curl -X PUT http://localhost:3000/api/server/config \
  -H "Content-Type: application/json" \
  -d '{
    "port": 8900,
    "maxPlayers": 64,
    "useBattleye": true
  }'
```

## 📱 Discord Notifications

The system sends automatic Discord notifications when:

- ✅ Server started successfully
- 🛑 Server stopped successfully  
- 🔄 Server restarted successfully
- ❌ Error starting/stopping/restarting server
- ⚙️ Configurations updated

### Configure Webhooks

Edit file `src/data/webhooks.json`:

```json
{
  "serverstatus": "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"
}
```

## 🧪 Tests

Run the test file to verify everything is working:

```bash
node test_server_endpoint.js
```

Or test a specific endpoint:

```bash
node test_server_endpoint.js /status GET
node test_server_endpoint.js /start POST
```

## 📁 File Structure

```
Backend/
├── routes/
│   └── server.js              # Main endpoint
├── src/data/server/
│   ├── config.json            # Server configurations
│   └── status.json            # Current server status
├── src/data/temp/
│   ├── start-server-temp.bat  # .bat file to start server
│   ├── stop-server.bat        # .bat file to stop server
│   └── restart-server.bat     # .bat file to restart server
├── Docs/Endpoints/
│   ├── Server.md              # Complete documentation
│   └── README_Server.md       # This file
├── test_server_endpoint.js    # API test file
├── test_bat_files.js          # .bat files test file
└── SOLUCAO_PERMISSOES.md     # Permission problems solution
```

## 🔒 Security

- ✅ Input parameter validation
- ✅ File permission verification
- ✅ Timeout for long operations
- ✅ Detailed logs of all operations
- ✅ Robust error handling

## ⚠️ Important Notes

1. **Permissions**: Backend needs permission to execute system commands
2. **Paths**: Check if paths in `config.json` are correct
3. **Webhooks**: Configure webhooks to receive notifications
4. **Backup**: Backup configurations before changing
5. **Monitoring**: Status is automatically updated every 30 seconds

## 🐛 Troubleshooting

### Server doesn't start
- Check if paths in `config.json` are correct
- Confirm SteamCMD is installed
- Verify user permissions

### Error stopping/restarting server
- Run process test: `node test_server_process.js`
- Check if `SCUMServer.exe` process is really running
- Confirm backend has permissions to execute `taskkill`
- Check detailed logs in backend console

### Webhooks don't work
- Check if webhook URL is correct
- Confirm webhook has send permissions
- Check console logs

### Incorrect status
- Wait 30 seconds for automatic update
- Execute manually: `GET /api/server/status`
- Check if `SCUMServer.exe` process is running

### Advanced Debugging

#### API Test:
```bash
node test_server_endpoint.js
```

#### .bat files test:
```bash
node test_bat_files.js
```

#### Process test:
```bash
node test_server_process.js
```

These tests will:
- Check if `SCUMServer.exe` is running
- Test all .bat files individually
- List all SCUM-related processes
- Test stop commands
- Show detailed logs of each operation

## 📞 Support

For questions or problems:

1. Check console logs
2. Test endpoints individually
3. Confirm configurations in `config.json`
4. Verify system permissions

---

**Developed for SCUM Server Manager 2.0** 🎮 