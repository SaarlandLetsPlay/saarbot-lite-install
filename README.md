# SaarlandBot – Lite Edition

> **Diese kostenlose Lite-Edition ist auf 1 Server und grundlegende Module beschränkt.**
> Für unbegrenzte Server, alle Features und Premium-Support:
> **[👉 saarlandbot.de](https://saarlandbot.de)**

Ein modularer Discord-Bot mit Web-Dashboard — einfach per Docker Compose installierbar.

---

## ✨ Verfügbare Module (Lite)

| Modul | Beschreibung |
|-------|-------------|
| 👋 Welcome | Begrüßungsnachrichten für neue Mitglieder |
| 🎂 Birthday | Automatische Geburtstagsnachrichten |
| 🎉 Giveaway | Einfache Verlosungen |
| 🎫 Ticket | Support-Ticket-System |
| ⭐ Level | XP & Level-System |
| 🎯 Self-Role | Selbst-zuweisbare Rollen |
| 🎟️ Polls | Abstimmungen |
| 🔢 Counting | Zähl-Spiel |
| 📜 Rules | Regeln-Verifikation |

> **Nicht in der Lite-Edition:** Radio, Live-Notifications, VOD, Sales, Voice-XP, Voice-Rooms, AFK, Website-Monitoring, Admin-Panel, Zahlungen.

---

## 🚀 Installation

### Voraussetzungen

- **Docker** ≥ 24.0 → [docs.docker.com/get-docker](https://docs.docker.com/get-docker/)
- **Docker Compose v2** (eingebaut in Docker Desktop / `docker compose` Befehl)
- Ein **Discord Bot** → [discord.com/developers/applications](https://discord.com/developers/applications)

### Schritt 1: Dateien herunterladen

```bash
git clone https://github.com/SaarlandLetsPlay/saarbot-lite-install.git
cd saarbot-lite-install
cp .env.example .env
```

### Schritt 2: `.env` konfigurieren

```bash
nano .env
```

**Pflichtfelder:**

| Variable | Wo bekomme ich das? |
|----------|---------------------|
| `DISCORD_TOKEN` | Discord Developer Portal → Bot → Token |
| `DISCORD_CLIENT_ID` | Discord Developer Portal → Allgemeines → Anwendungs-ID |
| `DISCORD_CLIENT_SECRET` | Discord Developer Portal → OAuth2 → Client Secret |
| `DISCORD_CALLBACK_URL` | `https://DEINE-DOMAIN/api/auth/callback` |
| `DB_PASSWORD` | Selbst wählen – z.B. `openssl rand -hex 16` |
| `REDIS_PASSWORD` | Selbst wählen – z.B. `openssl rand -hex 16` |
| `SESSION_SECRET` | `openssl rand -hex 32` |
| `FRONTEND_URL` | URL deines Dashboards (z.B. `https://bot.deine-domain.de`) |
| `VITE_DISCORD_CLIENT_ID` | Gleich wie `DISCORD_CLIENT_ID` |

### Schritt 3: Starten

```bash
docker compose up -d
```

Das Dashboard ist direkt unter Port `80` erreichbar.

### Schritt 4: Bot zu Discord hinzufügen

```
https://discord.com/api/oauth2/authorize?client_id=DEINE_CLIENT_ID&permissions=8&scope=bot%20applications.commands
```

> ⚠️ **Lite-Limit:** Der Bot verlässt automatisch jeden Server nach dem ersten.

---

## 🔄 Updates

```bash
docker compose pull
docker compose up -d
```

---

## 📋 Nützliche Befehle

```bash
# Status aller Container
docker compose ps

# Logs des Bots anzeigen
docker compose logs -f bot

# Einzelnen Service neustarten
docker compose restart bot

# Alles stoppen
docker compose down

# Alles stoppen + Daten löschen
docker compose down -v
```

---

## 🌐 OAuth2-Einrichtung (Discord Developer Portal)

Damit der Dashboard-Login funktioniert:

1. [discord.com/developers/applications](https://discord.com/developers/applications) → Deine App → **OAuth2**
2. Unter **Redirects** eintragen:
   - `http://localhost:3001/api/auth/callback` *(lokal)*
   - `https://DEINE-DOMAIN/api/auth/callback` *(Produktion)*

---

## 🔒 Reverse Proxy mit HTTPS (empfohlen)

**Caddy** (einfachste Option, automatisches HTTPS):

```bash
# Caddyfile
bot.deine-domain.de {
  reverse_proxy localhost:80
}
```

**nginx** (Beispiel):

```nginx
server {
    listen 443 ssl;
    server_name bot.deine-domain.de;
    location / {
        proxy_pass http://localhost:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## ❓ Support & Vollversion

- **Bug melden:** [GitHub Issues](https://github.com/SaarlandLetsPlay/saarbot-lite-install/issues)
- **Vollversion mit allen Features:** [lythea.de](https://lythea.de)

---

## ⚖️ Lizenz

Diese Software wird als Lite-Edition zur freien, nicht-kommerziellen Nutzung bereitgestellt.
Reverse-Engineering, Dekompilierung und die Verbreitung modifizierter Versionen sind nicht gestattet.

© SaarlandBot – [lythea.de](https://lythea.de)
