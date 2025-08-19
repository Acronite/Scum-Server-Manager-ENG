# Responses for Frontend Dev – Chat In Game Integration

## Responses to Questions

### 1. Is the endpoint available?
✅ **YES** - The endpoint `GET /api/chat_in_game` is implemented and active in the backend.

✅ **YES** - It always returns JSON, even in case of error. Never returns HTML.

### 2. Endpoint response

#### When there are no messages:
```json
{
  "success": false,
  "message": "No chat log found.",
  "data": []
}
```

#### When there are messages:
```json
{
  "success": true,
  "message": "Messages read successfully.",
  "data": [
    {
      "timestamp": "2025.07.13-00.28.58",
      "steamId": "123456789",
      "playerName": "Wolf",
      "chatType": "Local",
      "message": "flw"
    },
    {
      "timestamp": "2025.07.13-00.29.15",
      "steamId": "987654321",
      "playerName": "Player123",
      "chatType": "Global",
      "message": "oi galera"
    }
  ]
}
```

#### In case of internal error:
```json
{
  "success": false,
  "message": "Error reading chat log.",
  "error": "Specific error details"
}
```

### 3. Proxy/Port
- **Default port**: 3000
- **Complete URL**: `http://localhost:3000/api/chat_in_game`
- **CORS**: Backend has CORS configured to accept frontend requests
- **Proxy**: If using Vite, configure in `vite.config.js`:
  ```javascript
  export default {
    server: {
      proxy: {
        '/api': {
          target: 'http://localhost:3000',
          changeOrigin: true
        }
      }
    }
  }
  ```

### 4. Correct path
✅ **YES** - The endpoint is exactly `/api/chat_in_game`
- There is no additional prefix like `/v1/`
- Complete URL: `http://localhost:3000/api/chat_in_game`

### 5. Logs and debug

#### Backend logs when frontend makes request:
```
GET /api/chat_in_game - 200 OK
```

#### If endpoint not found (404):
```json
{
  "success": false,
  "message": "Endpoint not found"
}
```

#### If there's internal error (500):
```json
{
  "success": false,
  "message": "Internal server error",
  "error": "Error details"
}
```

### 6. Real response example
```json
{
  "success": true,
  "message": "Messages read successfully.",
  "data": [
    {
      "timestamp": "2025.07.13-00.28.58",
      "steamId": "76561198123456789",
      "playerName": "Wolf",
      "chatType": "Local",
      "message": "flw galera"
    },
    {
      "timestamp": "2025.07.13-00.29.15",
      "steamId": "76561198987654321",
      "playerName": "Player123",
      "chatType": "Global",
      "message": "oi pessoal"
    },
    {
      "timestamp": "2025.07.13-00.30.22",
      "steamId": "76561198111222333",
      "playerName": "SquadMember",
      "chatType": "Squad",
      "message": "vamos jogar"
    }
  ]
}
```

### 7. Dependencies

#### Required environment variables:
- `SCUM_LOG_PATH`: Path to SCUM logs (e.g., `C:/SCUM/Logs/`)

#### Configurations:
- Backend must be running
- Chat logs must exist in configured path
- Discord webhook (optional, only for sending messages)

#### Authentication:
❌ **NO** - No authentication required for this endpoint

## Troubleshooting

### If receiving HTML instead of JSON:

1. **Check if backend is running**:
   ```bash
   curl http://localhost:3000/api/chat_in_game
   ```

2. **Check if port is correct**:
   - Backend: port 3000
   - Frontend: check if accessing correct port

3. **Check Vite proxy**:
   ```javascript
   // vite.config.js
   export default {
     server: {
       proxy: {
         '/api': {
           target: 'http://localhost:3000',
           changeOrigin: true
         }
       }
     }
   }
   ```

4. **Test directly in browser**:
   - Access: `http://localhost:3000/api/chat_in_game`
   - Should return JSON, not HTML

### If endpoint not found:

1. **Check if server is running**:
   ```bash
   npm start
   # or
   node server.js
   ```

2. **Check if route is registered**:
   - File `routes/chat.js` must be imported in `server.js`

## Frontend implementation example

```javascript
// Function to fetch chat messages
async function fetchChatMessages() {
  try {
    const response = await fetch('/api/chat_in_game');
    const data = await response.json();
    
    if (data.success) {
      // Process messages
      data.data.forEach(message => {
        console.log(`${message.playerName}: ${message.message} (${message.chatType})`);
      });
    } else {
      console.log('No new messages:', data.message);
    }
  } catch (error) {
    console.error('Error fetching messages:', error);
  }
}

// Call every X seconds for real-time updates
setInterval(fetchChatMessages, 5000); // Every 5 seconds
```

## Contact

If there are still problems, check:
1. Backend console logs
2. Browser network tab
3. If backend is running on correct port
4. If environment variables are configured 