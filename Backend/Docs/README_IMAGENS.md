# Project Presentation Images

This folder contains the SCUM Server Manager presentation images for the GitHub README.

## Suggested Structure

Save interface screenshots with the following names:

### Main Screenshots:
- `01-dashboard.png` - Main dashboard with statistics
- `02-players.png` - Online/offline players screen
- `03-fame-system.png` - Fame and ranking system
- `04-discord-settings.png` - Discord settings
- `05-administration.png` - Administration panel
- `06-vehicles.png` - Vehicle history
- `07-system-settings.png` - System settings
- `08-server-configuration.png` - Server configuration

### How to use in README.md:

```markdown
## Screenshots

### Main Dashboard
![Dashboard](docs/images/01-dashboard.png)

### Player System
![Players](docs/images/02-players.png)

### Fame System
![Fame System](docs/images/03-fame-system.png)

### Discord Settings
![Discord Settings](docs/images/04-discord-settings.png)

### Administration
![Administration](docs/images/05-administration.png)

### Vehicle History
![Vehicles](docs/images/06-vehicles.png)

### System Settings
![System Settings](docs/images/07-system-settings.png)

### Server Configuration
![Server Configuration](docs/images/08-server-configuration.png)
```

## Screenshot Tips:

1. **Resolution**: Use high resolution (1920x1080 or higher)
2. **Format**: Save in PNG for better quality
3. **Size**: Keep files under 1MB each
4. **Content**: Show real data when possible
5. **Interface**: Capture complete interface, not just parts

## Commands to add to Git:

```bash
# Add images
git add docs/images/*.png

# Make commit
git commit -m "Adding interface screenshots for README"

# Send to GitHub
git push origin master
```
