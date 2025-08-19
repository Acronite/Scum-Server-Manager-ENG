# 🔒 Security - Discord Token

## ⚠️ SECURITY ALERT

The Discord token has been removed from the `config.json` file for security reasons. **NEVER** commit tokens or passwords to the repository!

## How to Configure the Token Securely

### 1. Revoke Current Token (IMMEDIATE)
1. Access [Discord Developer Portal](https://discord.com/developers/applications)
2. Select your application
3. Go to "Bot" → "Reset Token"
4. **IMMEDIATELY** revoke the old token

### 2. Generate New Token
1. In Discord Developer Portal, generate a new token
2. Copy the new token

### 3. Configure Token Locally
Edit the file `src/data/server/config.json` and replace `"YOUR_TOKEN_HERE"` with your new token:

```json
{
  "discord_bot": {
    "enabled": true,
    "token": "YOUR_NEW_TOKEN_HERE",
    // ... rest of configuration
  }
}
```

### 4. Add to .gitignore
Make sure the `config.json` file is in `.gitignore`:

```gitignore
# Configurations with tokens
src/data/server/config.json
```

### 5. Use Environment Variables (Recommended)
For greater security, use environment variables:

1. Create a `.env` file in the project root:
```env
DISCORD_BOT_TOKEN=your_token_here
```

2. Modify the code to read from environment variable:
```javascript
const token = process.env.DISCORD_BOT_TOKEN || config.discord_bot.token;
```

## Commands to Fix Repository

```bash
# 1. Remove file from Git history
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch src/data/server/config.json" \
  --prune-empty --tag-name-filter cat -- --all

# 2. Force push to clean history
git push origin --force --all

# 3. Add file to .gitignore
echo "src/data/server/config.json" >> .gitignore

# 4. Make new commit
git add .gitignore
git commit -m "fix: remove sensitive data and update gitignore"
git push origin main
```

## Security Verification

After making corrections, verify there are no more exposed tokens:

```bash
# Search for token patterns in repository
grep -r "MTM5NTQ5NjY1NDE1NjU5NTQwNA" .
grep -r "discord.*token" . --ignore-case
```

## Next Steps

1. ✅ Revoke old token
2. ✅ Generate new token
3. ✅ Configure locally
4. ✅ Clean Git history
5. ✅ Update .gitignore
6. ✅ Test application
