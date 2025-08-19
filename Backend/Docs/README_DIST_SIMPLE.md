# Scum Server Manager - Simple Distribution

## 🚀 How to run:

### Windows:
1. Double-click `start.bat`
2. Wait for dependencies installation
3. Server will start automatically

### Linux/Mac:
```bash
chmod +x start.sh
./start.sh
```

### Manual:
```bash
npm install
node server.js
```

## 📁 Configuration files:
- `src/data/server/config.json` - Server configurations
- `src/data/webhooks.json` - Discord webhooks
- `src/data/funny_statistics.json` - Fun statistics
- `.env` - Environment variables

## ✅ Advantages of this distribution:
- ✅ No compilation needed
- ✅ Works on any system
- ✅ Easy to install and run
- ✅ Separate and editable JSON files
- ✅ No dependency issues

## 🔧 Configuration:
1. Edit `.env` with your configurations
2. Configure `src/data/server/config.json`
3. Add webhooks in `src/data/webhooks.json`
4. Run `start.bat` (Windows) or `start.sh` (Linux/Mac)

## 📡 Access:
- API: http://localhost:3000
- Health: http://localhost:3000/health
- Statistics: http://localhost:3000/funny-stats
- Players: http://localhost:3000/players
