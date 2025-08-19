# Discord Webhooks - SCUM Server Manager

## Overview

The Discord webhook system allows configuring automatic notifications for different SCUM server events. Currently, the **"Player Panel"** webhook is integrated with the backend API.

## Available Webhooks

### 1. Player Panel WebHook ✅ (Integrated with API)
- **Endpoint:** `/api/webhook/painelplayers`
- **Functionality:** Player panel notifications
- **Status:** Integrated with real backend
- **Storage:** In backend file `src/data/webhooks.json`

### 2. Vehicles WebHook 🔄 (Simulated)
- **Functionality:** Vehicle-related notifications
- **Status:** Simulated (prepared for integration)

### 3. Admin Log WebHook ✅ (Integrated with API)
- **Endpoint:** `/api/webhook/adminlog`
- **Functionality:** Administrative logs
- **Status:** Integrated with real backend
- **Storage:** In backend file `src/data/webhooks.json`

### 4. Chat in Game 🔄 (Simulated)
- **Functionality:** In-game chat messages
- **Status:** Simulated (prepared for integration)

### 5. Bunkers WebHook 🔄 (Simulated)
- **Functionality:** Bunker-related events
- **Status:** Simulated (prepared for integration)

### 6. Fame WebHook ✅ (Integrated with API)
- **Endpoint:** `/api/webhook/famepoints`
- **Functionality:** Fame system events
- **Status:** Integrated with real backend
- **Storage:** In backend file `src/data/webhooks.json`

## How to Use

### Webhook Configuration

#### Player Panel WebHook
1. **Access Discord section:**
   - Click "DISCORD" in sidebar
   - Locate "Player Panel WebHook" card

2. **Configure webhook:**
   - Enter Discord webhook URL
   - Example: `https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE`

3. **Save configuration:**
   - Click "SAVE"
   - System will send URL to backend
   - Backend will save in `src/data/webhooks.json`

4. **Test webhook:**
   - Click "TEST"
   - A test message will be sent to Discord
   - Check if message arrived in channel

#### Admin Log WebHook
1. **Access Discord section:**
   - Click "DISCORD" in sidebar
   - Locate "Admin Log WebHook" card

2. **Configure webhook:**
   - Enter Discord webhook URL
   - Example: `https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE`

3. **Save configuration:**
   - Click "SAVE"
   - System will send URL to backend
   - Backend will save in `src/data/webhooks.json`

4. **Test webhook:**
   - Click "TEST"
   - A test message will be sent to Discord
   - Check if message arrived in channel

#### Bunkers WebHook
1. **Access Discord section:**
   - Click "DISCORD" in sidebar
   - Locate "Bunkers WebHook" card

2. **Configure webhook:**
   - Enter Discord webhook URL
   - Example: `https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE`

3. **Save configuration:**
   - Click "SAVE"
   - System will send URL to backend
   - Backend will save in `src/data/webhooks.json`

4. **Test webhook:**
   - Click "TEST"
   - A test message will be sent to Discord
   - Check if message arrived in channel

#### Fame WebHook
1. **Access Discord section:**
   - Click "DISCORD" in sidebar
   - Locate "Fame WebHook" card

2. **Configure webhook:**
   - Enter Discord webhook URL
   - Example: `https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE`

3. **Save configuration:**
   - Click "SAVE"
   - System will send URL to backend
   - Backend will save in `src/data/webhooks.json`

4. **Test webhook:**
   - Click "TEST"
   - A test message will be sent to Discord
   - Check if message arrived in channel

## Backend Configuration

### Environment Variable (Optional)
```env
VITE_BACKEND_URL=http://localhost:3000
```

### API Endpoints

#### Player Panel
```http
POST http://localhost:3000/api/webhook/painelplayers
Content-Type: application/json

{
  "url": "https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE"
}
```

#### Admin Log
```http
POST http://localhost:3000/api/webhook/adminlog
Content-Type: application/json

{
  "url": "https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE"
}
```

#### Bunkers
```http
POST http://localhost:3000/api/webhook/bunkers
Content-Type: application/json

{
  "url": "https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE"
}
```

#### Fame
```http
POST http://localhost:3000/api/webhook/famepoints
Content-Type: application/json

{
  "url": "https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE"
}
```

## File Structure

```
src/
├── components/
│   └── DiscordWebhookCard.tsx    # Card component
├── services/
│   └── webhookService.ts         # API service
├── config/
│   └── api.ts                    # API configuration
└── pages/
    └── Dashboard.tsx             # Main page
```

## Next Steps

To integrate other webhooks with backend:

1. **Create backend endpoints:**
   - `/api/webhook/veiculos`
   - `/api/webhook/chatgame`
   - `/api/webhook/bunkers`

2. **Update service:**
   - Add methods in `WebhookService`
   - Implement real API calls

3. **Update Dashboard:**
   - Modify `handleSaveWebhook` and `handleTestWebhook`
   - Remove simulations

## Troubleshooting

### Connection Error
- Check if backend is running on `http://localhost:3000`
- Confirm if `VITE_BACKEND_URL` variable is configured correctly

### Webhook Error
- Check if Discord URL is correct
- Confirm if webhook has send permissions
- Test URL manually in Postman

### Error Messages
- **"Error connecting to server"**: Backend offline
- **"Error testing webhook"**: Invalid URL or Discord inaccessible
- **"HTTP error! status: 404"**: Endpoint not found 