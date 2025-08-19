# 🔐 Authentication API - SCUM Server Manager

## 📋 Overview

JWT-based authentication system with local JSON storage. Includes session control, access logs, and rate limiting.

## 🗂️ File Structure

```
src/data/auth/
├── users.json          # Registered users
├── sessions.json       # Active sessions
└── access_logs.json    # Access logs
```

## 🔧 Initial Configuration

### 1. **Install Dependencies**
```bash
npm install bcrypt jsonwebtoken
```

### 2. **Configure First Password**
```bash
# Generate password hash for admin user
node scripts/generate-password.js admin mypassword123
```

### 3. **Check .env**
```env
JWT_SECRET=your_secret_key_here
```

## 📡 Endpoints

### **POST /api/auth/login**
Logs into the system.

**Request:**
```json
{
  "username": "admin",
  "password": "mypassword123"
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "1",
      "username": "admin",
      "role": "admin"
    }
  }
}
```

**Response (Error):**
```json
{
  "success": false,
  "message": "Invalid username or password"
}
```

### **POST /api/auth/logout**
Logs out and invalidates session.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "message": "Logout successful"
}
```

### **GET /api/auth/me**
Returns logged user information.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "1",
    "username": "admin",
    "role": "admin"
  }
}
```

### **GET /api/auth/logs**
Returns access logs (admin only).

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "logs": [
      {
        "id": "1705756800000",
        "username": "admin",
        "action": "login_success",
        "ip": "192.168.1.100",
        "user_agent": "Mozilla/5.0...",
        "timestamp": "2025-01-20T15:30:00Z",
        "success": true
      }
    ],
    "total": 1
  }
}
```

### **POST /api/auth/change-password**
Changes logged user password.

**Headers:**
```
Authorization: Bearer <token>
```

**Request:**
```json
{
  "currentPassword": "current_password",
  "newPassword": "new_password"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Password changed successfully"
}
```

## 🔒 Security

### **Rate Limiting**
- Maximum 5 login attempts per IP in 15 minutes
- Automatic blocking after exceeding limit

### **JWT Token**
- Expiration: 24 hours
- Stored in `sessions.json`
- Invalidated on logout

### **Access Logs**
- All login/logout attempts
- Real IP capture
- Browser User-Agent
- Success/failure of operation

### **Password Hashing**
- bcrypt with salt rounds = 10
- Passwords never stored in plain text

## 🛡️ Protection Middleware

### **requireAuth**
Protects routes that need authentication.

```javascript
const { requireAuth } = require('../src/middleware/auth');

// Apply to routes
app.get('/api/protected', requireAuth, (req, res) => {
  // req.user contains user data
  res.json({ user: req.user });
});
```

## 📊 Data Structure

### **users.json**
```json
{
  "users": [
    {
      "id": "1",
      "username": "admin",
      "password": "$2b$10$hash_here",
      "role": "admin",
      "created_at": "2025-01-20T10:00:00Z",
      "last_login": "2025-01-20T15:30:00Z",
      "active": true
    }
  ]
}
```

### **sessions.json**
```json
{
  "sessions": [
    {
      "token": "jwt_token_here",
      "user_id": "1",
      "username": "admin",
      "created_at": "2025-01-20T15:30:00Z",
      "expires_at": "2025-01-21T15:30:00Z",
      "ip": "192.168.1.100"
    }
  ]
}
```

### **access_logs.json**
```json
{
  "logs": [
    {
      "id": "1705756800000",
      "username": "admin",
      "action": "login_success",
      "ip": "192.168.1.100",
      "user_agent": "Mozilla/5.0...",
      "timestamp": "2025-01-20T15:30:00Z",
      "success": true
    }
  ]
}
```

## 🔧 Useful Commands

### **Generate Password Hash**
```bash
node scripts/generate-password.js admin mypassword123
```

### **Check Logs**
```bash
# Access via API
GET /api/auth/logs
```

### **Clear Expired Sessions**
```bash
# Restart server (clears automatically)
npm restart
```

## ⚠️ Status Codes

- **200**: Success
- **400**: Invalid data
- **401**: Not authenticated
- **403**: Access denied (not admin)
- **429**: Rate limit exceeded
- **500**: Internal error

## 🎯 Usage Examples

### **Login via Frontend**
```javascript
const login = async (username, password) => {
  const response = await fetch('/api/auth/login', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ username, password })
  });
  
  const data = await response.json();
  
  if (data.success) {
    // Save token
    localStorage.setItem('token', data.data.token);
    return data.data.user;
  } else {
    throw new Error(data.message);
  }
};
```

### **Authenticated Request**
```javascript
const fetchProtectedData = async () => {
  const token = localStorage.getItem('token');
  
  const response = await fetch('/api/protected-endpoint', {
    headers: {
      'Authorization': `Bearer ${token}`
    }
  });
  
  return response.json();
};
```

## 🔍 Troubleshooting

### **Problem: Invalid token**
- Check if JWT_SECRET is configured
- Check if token hasn't expired
- Check if session exists in `sessions.json`

### **Problem: Rate limit**
- Wait 15 minutes
- Check source IP
- Check logs in `access_logs.json`

### **Problem: Password doesn't work**
- Generate new hash with `generate-password.js`
- Check if user exists in `users.json`
- Check if `active: true`

---

**🔐 Authentication system ready for use!** 