# 🔨 Scum Server Manager Build

## 📋 Prerequisites

1. **Node.js** (version 18 or higher)
2. **npm** or **yarn**
3. **Pkg** (will be installed automatically)

## 🚀 How to Build

### 1. **Install dependencies**
```bash
npm install
```

### 2. **Build the project**
```bash
npm run build
```

### 3. **Result**
The executable will be generated in the `dist/` folder along with the necessary JSON files and .env.

## 📁 Build Structure

```
dist/
├── scum-server-manager-backend.exe    # Main executable
├── src/
│   ├── data/                          # JSON files (configurations)
│   ├── config/                        # Configurations
│   └── middleware/                    # Middlewares
├── routes/                            # API routes
├── scripts/                           # Utility scripts
├── .env                               # Environment variables
├── .env.example                       # Environment variables example
├── nodemon.json                       # Nodemon configuration
├── BUILD_INFO.json                    # Build information
└── README.md                          # Documentation
```

## ⚙️ Configuration

### **Files that can be edited after build:**
- `src/data/server/config.json` - Server configurations
- `src/data/webhooks.json` - Discord webhooks
- `src/data/funny_statistics.json` - Fun statistics
- `src/data/auth/users.json` - System users
- `src/data/players/players.json` - Player data
- `.env` - Environment variables
- `nodemon.json` - Nodemon configuration

### **Files that should NOT be edited:**
- The main executable
- Compiled JavaScript files

## 🔧 Available Commands

```bash
# Complete build (recommended)
npm run build

# Build only with pkg
npm run build-pkg

# Build for Windows
npm run build-win

# Build for Linux
npm run build-linux
```

## 📦 Distribution

To distribute the project:

1. **Copy the complete `dist/` folder**
2. **Configure the JSON and .env files** as needed
3. **Execute the .exe**

## 🔐 Security Configuration

### **File .env:**
```env
# Server Configuration
PORT=3000 
HOST=0.0.0.0 
NODE_ENV=development 
JWT_SECRET=your_jwt_secret_here
FRONTEND_URL=http://localhost:5173

# SCUM Logs Configuration
SCUM_LOG_PATH=C:\\Servers\\Scum\\SCUM\\Saved\\SaveFiles\\Logs
SCUM_LOG_CACHE_PATH=src/data/logs/cache
SCUM_LOG_MAX_RETRIES=2

# Webhook Configuration
WEBHOOK_URL=your_webhook_url
```

## 🐛 Troubleshooting

### **Dependency error:**
```bash
npm install
npm run build
```

### **Permission error:**
Run the terminal as administrator

### **JSON files not found:**
Check if the folder structure is correct

### **Error with .env:**
Make sure the `.env` file is in the `dist/` folder

## 📝 Important Notes

- ✅ JSON and .env files remain separate from the executable
- ✅ Configurations can be changed without recompiling
- ✅ The executable is standalone (doesn't need Node.js)
- ✅ Works on any Windows 10/11
- ✅ .env file preserves environment variables

## 🔄 Updates

To update the executable:

1. **Make changes to the code**
2. **Run `npm run build`**
3. **Replace the old executable**

JSON and .env files can be kept and don't need to be recopied.

## 🚨 Security

- **Never commit the `.env` file** to Git
- **Use `.env.example`** as a template
- **Configure passwords and tokens** only in the `.env` file
- **Keep the `.env` secure** in production