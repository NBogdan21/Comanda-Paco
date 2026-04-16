# 🚂 GTA V Shop Bot — Railway Deployment Guide

## Files in this project

```
bot.py            ← the bot
requirements.txt  ← Python dependencies
Procfile          ← tells Railway how to start the bot
runtime.txt       ← pins Python 3.11
.env.example      ← template for local testing
.gitignore        ← keeps secrets out of git
```

---

## Step 1 — Create the Discord Bot (skip if done)

1. Go to https://discord.com/developers/applications → New Application
2. Bot tab → Add Bot → Reset Token → copy the token
3. Enable Message Content Intent under Privileged Gateway Intents
4. OAuth2 → URL Generator: scopes bot + applications.commands,
   permissions: Send Messages, Embed Links, Read Message History, Manage Messages
5. Open the generated URL and invite the bot to your server

---

## Step 2 — Push to GitHub

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/YOUR_USERNAME/gta-shop-bot.git
git push -u origin main
```

Never commit your .env file — it is listed in .gitignore.

---

## Step 3 — Deploy on Railway

1. Go to https://railway.app and sign in with GitHub
2. Click New Project → Deploy from GitHub repo
3. Select your gta-shop-bot repository
4. Railway auto-detects Procfile and requirements.txt — no extra config needed

---

## Step 4 — Add Your Bot Token

1. In your Railway project, click the service → Variables tab
2. Add a new variable:
   Name:  DISCORD_TOKEN
   Value: your_actual_bot_token_here
3. Railway will automatically redeploy

---

## Step 5 — Add a Persistent Volume (orders survive redeploys)

1. In your Railway project, click New → Volume
2. Set Mount Path to /data
3. Attach it to your bot service

The bot saves orders to /data/orders.json.
With the volume this file survives every redeploy. Without it, orders reset.

---

## Step 6 — Verify it's running

In the Railway Logs tab you should see:
  Health server running on port XXXX
  Bot pornit ca YourBot#1234

---

## Using the Bot in Discord

Admin commands (type in any channel, messages auto-delete):
  !shop         → posts the interactive shop panel
  !adminpanel   → posts the admin control panel
  !orders       → shows all current orders

Member buttons:
  Product buttons   → opens popup to enter quantity
  📦 Vezi comanda   → shows your order (private)
  🗑️ Șterge comanda → clears your cart

Admin panel buttons:
  📋 Vezi toate comenzile    → all orders + grand total (private)
  🗑️ Șterge TOATE comenzile  → asks for CONFIRM, then wipes everything

---

## Local Testing

```bash
cp .env.example .env        # then paste your token inside
pip install -r requirements.txt
python bot.py
```

---

## Products & Pricing

Edit the PRODUCTS dict at the top of bot.py to change prices or add items.

  Amoniac    $4,500
  Bicarbonat $4,500
  Plicuri    $150
  Brichete   $150
  Detergent  $350
  Seringa    $200
