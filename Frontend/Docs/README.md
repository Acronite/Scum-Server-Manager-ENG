# 📚 ScumServerManager 2.0 Documentation

## 🎯 Overview

This directory contains all the technical documentation for the ScumServerManager 2.0 system, including implementation guides, API specifications, and usage tutorials.

## 📋 Documentation Index

### 🏰 Bunker System
- **[Bunker Status System](./bunker-status-system.md)** - Complete documentation of the new bunker system with persistent database and detailed interface

### 🔗 Discord Webhooks
- **[Discord Webhooks](./discord-webhooks.md)** - Complete guide for configuring and using webhooks
- **[Webhook Admin Log](./webhook-adminlog.md)** - Webhook configuration for administration logs
- **[Webhook Bunkers](./webhook-bunkers.md)** - Webhook configuration for bunker notifications
- **[Webhook Fame](./webhook-fama.md)** - Webhook configuration for fame system

### 🏆 Fame System
- **[Top 3 Card](./fame-top3-card.md)** - Implementation of the static top 3 players card
- **[Breaking News Bar](./fame-breaking-news-bar.md)** - News bar for displaying top 3

### 🚗 Vehicle System
- **[Bunkers Card](./bunkers-card.md)** - Bunker system documentation (previous version)

### 🔧 Configurations and Endpoints
- **[Backend Network Access](./backend-network-access.md)** - Network and access configurations
- **[Backend Chat in Game Questions](./duvidas-backend-chat-in-game.md)** - FAQ about in-game chat
- **[GET Webhook PainelPlayers Endpoint Request](./solicitacao-endpoint-get-webhook-painelplayers.md)** - Endpoint specification

## 🚀 Major Updates

### ✅ Bunker System Redesigned
The bunker system has been **completely redesigned** with the following improvements:

1. **Persistent Database**
   - Data maintained between restarts
   - File: `src/data/bunkers/bunkers.json`

2. **Detailed Formatting**
   - Complete status of active and blocked bunkers
   - Coordinate information, activation time and methods
   - Discord-optimized formatting

3. **Complete Interface**
   - Dedicated card on dashboard
   - Automatic update every 5 minutes
   - Manual update button
   - Last update indicator

### ✅ New Webhooks Implemented
- **Webhook Admin Log** - For administration logs
- **Webhook Bunkers** - For bunker notifications
- **Webhook Fame** - For reputation system

### ✅ Steam ID Hiding System
- Global toggle to hide Steam IDs in all sections
- Ideal for video recording
- Persistence in localStorage

## 🔧 API Structure

### Main Endpoints

#### Bunkers
- `GET /api/bunkers/status` - Current bunker status
- `POST /api/bunkers/force-update` - Force update

#### Webhooks
- `GET /api/webhooks/painel-players` - Query Painel Players webhook
- `POST /api/webhooks/painel-players` - Save Painel Players webhook
- `GET /api/webhooks/veiculos` - Query Vehicles webhook
- `POST /api/webhooks/veiculos` - Save Vehicles webhook
- `GET /api/webhooks/admin-log` - Query Admin Log webhook
- `POST /api/webhooks/admin-log` - Save Admin Log webhook
- `GET /api/webhooks/chat-in-game` - Query Chat in Game webhook
- `POST /api/webhooks/chat-in-game` - Save Chat in Game webhook
- `GET /api/webhooks/bunkers` - Query Bunkers webhook
- `POST /api/webhooks/bunkers` - Save Bunkers webhook
- `GET /api/webhooks/fama` - Query Fame webhook
- `POST /api/webhooks/fama` - Save Fame webhook

#### Players
- `GET /api/players/painel` - Player list
- `GET /api/fame/points` - Fame points

#### Vehicles
- `GET /api/vehicles/log` - Vehicle event log

## 🎨 Main Components

### Dashboard
- **DashboardHeader** - Main header with navigation
- **DashboardSidebar** - Side menu with sections
- **ScumBackground** - SCUM thematic background

### Information Cards
- **BunkerStatusCard** - Detailed bunker status (NEW)
- **ChatMessagesCard** - In-game chat messages
- **DiscordWebhookCard** - Webhook configuration
- **FameTop3Card** - Top 3 fame players

### Tables and Lists
- **PlayersTable** - Complete player list
- **VehiclesTable** - Vehicle event history
- **AdminLogTable** - Administration log
- **FamePlayersList** - Player list by fame

### Breaking News
- **FameTop3BreakingNewsBar** - Top 3 bar
- **VehicleEventBreakingNewsBar** - Vehicle events bar

## 🔄 Automatic Updates

### Update Frequencies
- **Players**: 30 seconds
- **Top 3 Fame**: 30 seconds
- **Bunkers**: 30 minutes
- **Vehicles**: 30 seconds
- **Admin Log**: On demand

### Update Indicators
- Timestamp of last update on each card
- Loading spinners during updates
- Manual update buttons

## 🎯 Special Features

### Steam ID Hiding System
```typescript
// Global toggle to hide Steam IDs
const [hideSteamIds, setHideSteamIds] = useState(false);

// Persistence in localStorage
useEffect(() => {
  localStorage.setItem('hideSteamIds', hideSteamIds.toString());
}, [hideSteamIds]);
```

### Webhook Compatibility
- All webhooks follow the same pattern
- Independent error handling
- Detailed logs for debugging

## 📱 Responsiveness

The system is fully responsive with:
- Adaptive grid (1 column mobile, 2+ columns desktop)
- Cards that adjust to screen size
- Mobile-optimized navigation

## 🚀 Next Implementations

1. **Map Visualization** for bunkers
2. **Advanced Filters** by region/status
3. **Push Notifications** for important events
4. **Detailed History** of activations
5. **Richer Discord Integration**

## 📞 Support

For technical questions or implementation issues, consult the specific documentation for each functionality or contact the development team.

---

**Last update:** January 2025  
**Version:** 2.0  
**Status:** ✅ 100% Functional 