# 🔐 Password Manager - SCUM Server Manager

## 📁 Available Files

### 1. `alterar_senha_rapido.bat` - **Recommended for daily use**
- Simple and direct interface
- Change password for any user
- Password confirmation validation
- Shows available users

### 2. `alterar_senha.bat` - **Complete manager**
- Interactive menu with multiple options
- List registered users
- Create new users
- Activate/deactivate users
- Change passwords

## 🚀 How to Use

### **Quick Method (Recommended):**
1. Double-click `alterar_senha_rapido.bat`
2. Enter username (e.g., `admin`)
3. Enter new password
4. Confirm new password
5. Done! ✅

### **Complete Method:**
1. Double-click `alterar_senha.bat`
2. Choose desired option in menu
3. Follow on-screen instructions

## 📋 Features

### **alterar_senha_rapido.bat:**
- ✅ Lists available users
- ✅ Password validation
- ✅ Password confirmation
- ✅ Visual feedback
- ✅ Clean interface

### **alterar_senha.bat:**
- ✅ Complete interactive menu
- ✅ Manage users (CRUD)
- ✅ View user status
- ✅ Activate/deactivate users
- ✅ Create new users
- ✅ List complete details

## 🔧 Requirements

- Node.js installed
- Project dependencies installed (`npm install`)
- File `src/data/auth/users.json` exists

## ⚠️ Important

- **Never** edit `users.json` file manually
- **Always** use .bat files for changes
- Passwords are automatically encrypted
- Automatic backup before changes

## 🎯 Usage Example

```
Available users:
  - admin (Active)

Enter username: admin
Enter new password: mypassword123
Confirm new password: mypassword123

🔧 Generating new password for: admin

✅ Password changed successfully!
📝 New password: mypassword123
```

## 🛡️ Security

- Passwords always encrypted with bcrypt
- User input validation
- Mandatory password confirmation
- Automatic change logs
- Existing user verification

## 📞 Support

If there are problems:
1. Check if Node.js is installed
2. Run `npm install` in project folder
3. Check if `users.json` file exists
4. Test with `admin` user first

---

**Developed for SCUM Server Manager 2.0** 🎮 