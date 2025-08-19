# Backend Scheduler API

## Overview

The backend scheduler system allows automatically executing a sequence of endpoints every 30 seconds, as an alternative to the frontend. The system implements a hybrid approach where the backend can run independently, but the frontend can still execute when needed.

## Available Endpoints

### 1. GET /api/scheduler/status

**Description:** Gets the current scheduler status

**Success Response:**
```json
{
  "success": true,
  "message": "Scheduler status retrieved successfully",
  "data": {
    "isRunning": true,
    "enabled": true,
    "interval": 30000,
    "lastExecution": 1703123456789,
    "executionSource": "backend",
    "timeSinceLastExecution": 15000,
    "stats": {
      "totalExecutions": 10,
      "successfulExecutions": 9,
      "failedExecutions": 1,
      "lastError": "2025-01-20T10:30:00.000Z",
      "lastSuccess": "2025-01-20T10:35:00.000Z"
    },
    "endpoints": [
      "/api/adminlog",
      "/api/chat_in_game",
      "/api/LogVeiculos",
      "/api/famepoints",
      "/api/bunkers/status",
      "/api/players/painelplayers"
    ],
    "nextExecution": 1703123486789
  }
}
```

### 2. POST /api/scheduler/start

**Description:** Starts the backend scheduler

**Success Response:**
```json
{
  "success": true,
  "message": "Scheduler started successfully",
  "data": {
    "isRunning": true,
    "enabled": true,
    "interval": 30000,
    "lastExecution": null,
    "executionSource": null,
    "timeSinceLastExecution": null,
    "stats": {
      "totalExecutions": 0,
      "successfulExecutions": 0,
      "failedExecutions": 0,
      "lastError": null,
      "lastSuccess": null
    },
    "endpoints": [
      "/api/adminlog",
      "/api/chat_in_game",
      "/api/LogVeiculos",
      "/api/famepoints",
      "/api/bunkers/status",
      "/api/players/painelplayers"
    ],
    "nextExecution": null
  }
}
```

### 3. POST /api/scheduler/stop

**Description:** Stops the backend scheduler

**Success Response:**
```json
{
  "success": true,
  "message": "Scheduler stopped successfully",
  "data": {
    "isRunning": false,
    "enabled": true,
    "interval": 30000,
    "lastExecution": 1703123456789,
    "executionSource": "backend",
    "timeSinceLastExecution": 15000,
    "stats": {
      "totalExecutions": 10,
      "successfulExecutions": 9,
      "failedExecutions": 1,
      "lastError": "2025-01-20T10:30:00.000Z",
      "lastSuccess": "2025-01-20T10:35:00.000Z"
    },
    "endpoints": [
      "/api/adminlog",
      "/api/chat_in_game",
      "/api/LogVeiculos",
      "/api/famepoints",
      "/api/bunkers/status",
      "/api/players/painelplayers"
    ],
    "nextExecution": null
  }
}
```

### 4. POST /api/scheduler/execute

**Description:** Manually executes the endpoint sequence

**Success Response:**
```json
{
  "success": true,
  "message": "Manual execution completed successfully",
  "data": {
    "success": true,
    "executionTime": 8500,
    "successCount": 5,
    "failureCount": 1,
    "results": [
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/adminlog"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/chat_in_game"
      },
      {
        "success": false,
        "error": "Request timeout",
        "endpoint": "/api/LogVeiculos"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/famepoints"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/bunkers/status"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/players/painelplayers"
      }
    ],
    "source": "manual"
  }
}
```

### 5. GET /api/scheduler/can-frontend-execute

**Description:** Checks if frontend can execute (to avoid conflicts)

**Success Response:**
```json
{
  "success": true,
  "message": "Frontend execution check completed",
  "data": {
    "canExecute": false,
    "schedulerStatus": {
      "isRunning": true,
      "enabled": true,
      "interval": 30000,
      "lastExecution": 1703123456789,
      "executionSource": "backend",
      "timeSinceLastExecution": 5000,
      "stats": {
        "totalExecutions": 10,
        "successfulExecutions": 9,
        "failedExecutions": 1,
        "lastError": "2025-01-20T10:30:00.000Z",
        "lastSuccess": "2025-01-20T10:35:00.000Z"
      },
      "endpoints": [
        "/api/adminlog",
        "/api/chat_in_game",
        "/api/LogVeiculos",
        "/api/famepoints",
        "/api/bunkers/status",
        "/api/players/painelplayers"
      ],
      "nextExecution": 1703123486789
    }
  }
}
```

### 6. POST /api/scheduler/frontend-execute

**Description:** Executes the sequence via frontend (if allowed)

**Success Response:**
```json
{
  "success": true,
  "message": "Frontend execution completed successfully",
  "data": {
    "success": true,
    "executionTime": 8200,
    "successCount": 6,
    "failureCount": 0,
    "results": [
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/adminlog"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/chat_in_game"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/LogVeiculos"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/famepoints"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/bunkers/status"
      },
      {
        "success": true,
        "status": 200,
        "endpoint": "/api/players/painelplayers"
      }
    ],
    "source": "frontend"
  }
}
```

**Error Response (when cannot execute):**
```json
{
  "success": false,
  "error": "Frontend cannot execute at this time"
}
```

## Configuration

The scheduler is configured in file `src/data/server/config.json`:

```json
{
  "scheduler": {
    "enabled": true,
    "interval": 30000,
    "frontend_fallback": true,
    "endpoints": [
      "/api/adminlog",
      "/api/chat_in_game",
      "/api/LogVeiculos",
      "/api/famepoints",
      "/api/bunkers/status",
      "/api/players/painelplayers"
    ],
    "retry_attempts": 3,
    "retry_delay": 5000,
    "timeout": 10000
  }
}
```

## Control Logic

### Execution Priority:
1. **Backend** has priority when active
2. **Frontend** only executes if backend is inactive or if enough time has passed
3. **Manual execution** always available

### Conflict Prevention:
- If backend executed < 24 seconds ago → frontend doesn't execute
- If frontend executed < 24 seconds ago → backend doesn't execute
- Detailed logs of which source executed

### System States:
- **Backend Active + Frontend Active**: Backend executes, frontend monitors
- **Backend Inactive + Frontend Active**: Frontend executes normally
- **Backend Active + Frontend Inactive**: Only backend executes
- **Both Inactive**: No automatic execution

## Frontend Implementation

### Check Before Executing:
```javascript
// Check if can execute before making the call
const response = await fetch('/api/scheduler/can-frontend-execute');
const data = await response.json();

if (data.data.canExecute) {
  // Execute normally
  await executeEndpoints();
} else {
  // Backend is executing, just monitor
  console.log('Backend is executing, waiting...');
}
```

### Status Monitoring:
```javascript
// Check status periodically
setInterval(async () => {
  const response = await fetch('/api/scheduler/status');
  const data = await response.json();
  
  updateSchedulerStatus(data.data);
}, 5000);
```

### Interface Controls:
```javascript
// Control buttons
const startScheduler = async () => {
  await fetch('/api/scheduler/start', { method: 'POST' });
};

const stopScheduler = async () => {
  await fetch('/api/scheduler/stop', { method: 'POST' });
};

const executeManual = async () => {
  await fetch('/api/scheduler/execute', { method: 'POST' });
};
```

## Logs and Monitoring

The system generates detailed logs for:
- Scheduler start/stop
- Successful executions and failures
- Conflicts between backend and frontend
- Performance statistics

## Gradual Migration

1. **Phase 1**: Implement backend (parallel)
2. **Phase 2**: Test and adjust configurations
3. **Phase 3**: Activate backend, keep frontend as fallback
4. **Phase 4**: Optional: disable frontend

## Benefits

- ✅ **Frontend independence**
- ✅ **Guaranteed execution**
- ✅ **Better error control**
- ✅ **Centralized logs**
- ✅ **Flexible configuration**
- ✅ **Redundancy in case of failures** 