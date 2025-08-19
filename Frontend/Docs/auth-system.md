# 🔐 Authentication System - Frontend

## 📋 **Overview**

The authentication system was implemented in the frontend to connect with the real backend. The system includes login, logout, route protection, log viewing, and password change.

## 🎯 **Implemented Features**

### ✅ **Login/Logout**
- Functional login form with validation
- Automatic logout with data cleanup
- Redirect after login/logout
- Default credentials: `admin` / `admin123`

### ✅ **Route Protection**
- Routes protected by authentication
- Admin-specific routes (`/logs`)
- Automatic redirect to login
- Loading state during verification

### ✅ **State Management**
- Global authentication context (`useAuth`)
- Data persistence in localStorage
- Automatic token verification
- Loading and error states

### ✅ **Logs Interface (Admin)**
- Access log viewing
- Date and status filters
- Manual log updates
- Responsive interface

### ✅ **Password Change**
- Modal for password change
- Password validation
- Visual error feedback
- Backend integration

## 🔧 **Implemented Files**

### **1. Authentication Service**
- **File:** `src/services/authService.ts`
- **Function:** Backend communication
- **Methods:**
  - `login(username, password)`
  - `logout()`
  - `getMe()`
  - `getLogs()`
  - `changePassword(currentPassword, newPassword)`

### **2. Authentication Hook**
- **File:** `src/hooks/useAuth.tsx`
- **Function:** Global state management
- **States:**
  - `user`: User data
  - `token`: JWT token
  - `isLoading`: Loading state
  - `error`: Error messages
  - `isAuthenticated`: If authenticated
  - `isAdmin`: If administrator

### **3. Route Protection**
- **File:** `src/components/ProtectedRoute.tsx`
- **Function:** Protect routes by authentication
- **Features:**
  - Authentication verification
  - Admin permission verification
  - Loading state
  - Automatic redirect

### **4. Logs Viewing**
- **File:** `src/components/AuthLogsView.tsx`
- **Function:** Interface for access logs
- **Features:**
  - Responsive table
  - Visual filters
  - Manual updates
  - Date formatting

### **5. Password Change**
- **File:** `src/components/ChangePasswordModal.tsx`
- **Function:** Modal to change password
- **Features:**
  - Field validation
  - Visual feedback
  - Backend integration

### **6. Route Configuration**
- **File:** `src/main.tsx`
- **Function:** Application route configuration
- **Routes:**
  - `/login`: Login page
  - `/dashboard`: Main dashboard (protected)
  - `/logs`: Access logs (protected, admin)

## 🚀 **How to Use**

### **1. Login**
1. Access the application
2. Enter credentials: `admin` / `admin123`
3. Click "ACCESS SYSTEM"
4. Will be redirected to dashboard

### **2. View Logs (Admin)**
1. Login as admin
2. Click logs icon in header (📄)
3. View access logs
4. Use "Update" button to reload

### **3. Change Password**
1. Click lock icon in header (🔒)
2. Enter current password
3. Enter new password
4. Confirm new password
5. Click "Change Password"

### **4. Logout**
1. Click logout icon in header (🚪)
2. Will be redirected to login page
3. Data will be cleaned automatically

## 🔍 **Data Structure**

### **User Interface**
```typescript
interface User {
  id: string;
  username: string;
  steamId?: string | null;
  role: string;
  permissions: string[];
  profile: {
    firstName: string;
    lastName: string;
    avatar?: string | null;
    timezone: string;
    language: string;
  };
  isFirstLogin: boolean;
  requiresPasswordChange: boolean;
}
```

### **Auth Log Interface**
```typescript
interface AuthLog {
  id: string;
  timestamp: string;
  username: string;
  action: string;
  ip: string;
  success: boolean;
}
```

## 🔧 **Configuration**

### **Backend URL**
The system is configured to connect with:
```
http://localhost:3000/api/auth
```

### **Used Endpoints**
- `POST /api/auth/login` - Login
- `POST /api/auth/logout` - Logout
- `GET /api/auth/me` - Verify user
- `GET /api/auth/logs` - Fetch logs
- `POST /api/auth/change-password` - Change password

### **LocalStorage Keys**
- `scum_auth_token` - JWT token
- `scum_user_data` - User data

## 🎨 **Interface**

### **Login Page**
- Responsive design
- Smooth animations
- Real-time validation
- Visible test credentials

### **Dashboard Header**
- Server status
- Player count
- Action buttons (logs, change password, logout)
- Visual indicators

### **Logs View**
- Responsive table
- Visual filters
- Colored status
- Manual updates

### **Change Password Modal**
- Responsive modal
- Field validation
- Visual feedback
- Smooth animations

## 🔒 **Security**

### **JWT Token**
- Stored in localStorage
- Automatic validity verification
- Automatic cleanup if invalid

### **Route Protection**
- Authentication verification
- Permission verification
- Automatic redirect

### **Validation**
- Required field validation
- Password validation
- Error feedback

## 🐛 **Troubleshooting**

### **Problem: Login doesn't work**
- Check if backend is running on port 3000
- Check credentials: `admin` / `admin123`
- Check browser console for errors

### **Problem: Invalid token**
- Check if JWT_SECRET is configured in backend
- Clear localStorage and login again
- Check backend logs

### **Problem: Routes don't protect**
- Check if `ProtectedRoute` is being used
- Check if `AuthProvider` wraps the application
- Check if context is working

### **Problem: Logs don't appear**
- Check if user has admin permission
- Check if endpoint `/api/auth/logs` is working
- Check console for network errors

## 📋 **Implementation Checklist**

- [x] **Authentication service** (`authService.ts`)
- [x] **Authentication context** (`useAuth.tsx`)
- [x] **Route protection** (`ProtectedRoute.tsx`)
- [x] **Logs viewing** (`AuthLogsView.tsx`)
- [x] **Password change** (`ChangePasswordModal.tsx`)
- [x] **Route configuration** (`main.tsx`)
- [x] **Login interface** (`Login.tsx`)
- [x] **Header with actions** (`DashboardHeader.tsx`)

## 🎉 **System Ready!**

The authentication system is **100% functional** and integrated with the backend. All features are implemented and tested.

**Default credentials:**
- **User:** `admin`
- **Password:** `admin123`

---

**📞 Support:** In case of questions, check backend logs and browser console for debugging. 