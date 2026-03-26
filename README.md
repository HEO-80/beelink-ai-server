<div align="center">

<br/>

# 🦞 beelink-ai-server

### A personal AI server running 24/7 on a Mini PC — free, private, always on.

OpenClaw + Gemini 2.5 Flash via Google OAuth, connected to Telegram.  
No cloud subscription. No GPU required. ~1€/month in electricity.

<br/>

[![OpenClaw](https://img.shields.io/badge/OpenClaw-2026.2.6-ff4d4d?style=for-the-badge&logo=lobster&logoColor=white)](https://github.com/openclaw/openclaw)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Node.js](https://img.shields.io/badge/Node.js-22.x-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Telegram](https://img.shields.io/badge/Telegram-Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://core.telegram.org/bots)
[![Gemini](https://img.shields.io/badge/Gemini_2.5_Flash-Free_OAuth-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-c792ea?style=for-the-badge)](LICENSE)

<br/>

> Part of the **[HEO-AGENT](https://github.com/HEO-80/heo-agent)** project —  
> a publicly documented build of a personal autonomous AI ecosystem.

<br/>

</div>

---

<div align="center">

## 🏗️ Architecture

</div>

```
📱 Telegram (mobile)
        ↕
🖥️  Beelink SER5 MAX  ──  OpenClaw Gateway (systemd, ws://127.0.0.1:18789)
        ↕
☁️  Google Gemini 2.5 Flash  (free OAuth, no credit card)
```

The Beelink acts as a **lightweight bridge** — it orchestrates tools, manages sessions, and routes messages. All the heavy LLM processing happens in the cloud for free via Google OAuth.

---

<div align="center">

## 💻 Hardware

</div>

| Component | Detail |
|---|---|
| Device | Beelink SER5 MAX Mini PC |
| CPU | AMD Ryzen 7 6800U · 8C/16T · up to 4.7 GHz |
| RAM | 24 GB DDR5 |
| Storage | 500 GB NVMe SSD |
| OS | Ubuntu 24.04.4 LTS |
| Power (idle) | ~10–15W |
| Access | SSH remote |

---

<div align="center">

## 🛠️ Tech Stack

</div>

| Layer | Technology |
|---|---|
| AI Agent Framework | [OpenClaw](https://github.com/openclaw/openclaw) `2026.2.6-3` |
| LLM | Google Gemini 2.5 Flash (free via OAuth) |
| Messaging Channel | Telegram Bot API |
| Runtime | Node.js `22.22.0` via nvm |
| Process Manager | systemd user service |
| OS | Ubuntu 24.04 LTS |
| Monitoring (optional) | Grafana + Prometheus + Node Exporter |

---

<div align="center">

## 🚀 Setup Guide

</div>

### 1. Install Node 22 via nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22
node --version  # should be v22.x
```

### 2. Install OpenClaw globally

```bash
sudo npm install -g openclaw@latest --legacy-peer-deps
openclaw --version  # 2026.x.x
```

### 3. Configure your Telegram bot

Get your token from [@BotFather](https://t.me/BotFather), then edit `~/.openclaw/openclaw.json`:

```json
{
  "channels": {
    "telegram": {
      "botToken": "YOUR_BOT_TOKEN_HERE",
      "enabled": true
    }
  }
}
```

> ⚠️ Never commit this file. It contains credentials. It's in `.gitignore`.

### 4. Authenticate with Google (free model)

```bash
# Enable the auth plugin
openclaw plugins enable google-gemini-cli-auth

# OAuth login — opens a Google URL in your browser
# If on a remote server, open an SSH tunnel first:
# ssh -L 8085:localhost:8085 user@your-server-ip
openclaw models auth login --provider google-gemini-cli-auth --set-default

# Set Flash model (higher free quota)
openclaw models set google-gemini-cli/gemini-2.5-flash
```

After login, Google redirects to `localhost:8085/oauth2callback?...` — copy that full URL and paste it back in the terminal when prompted.

### 5. Install systemd service

```bash
# Copy the service file (edit the path if your username isn't 'heo')
mkdir -p ~/.config/systemd/user
cp openclaw-gateway.service ~/.config/systemd/user/

# Enable and start
systemctl --user daemon-reload
systemctl --user enable openclaw-gateway
systemctl --user start openclaw-gateway

# Verify
systemctl --user status openclaw-gateway
```

---

<div align="center">

## ⚙️ Gateway Management

</div>

```bash
# Check status
systemctl --user status openclaw-gateway

# Live logs
journalctl --user -u openclaw-gateway -f

# Restart after config changes
systemctl --user restart openclaw-gateway

# Clean stop
openclaw gateway stop
```

---

<div align="center">

## 📁 Repo Structure

</div>

```
beelink-ai-server/
├── README.md
├── openclaw-gateway.service       # systemd user service
├── docker-compose.monitoring.yml  # optional monitoring stack
└── .gitignore
```

---

<div align="center">

## 💰 Monthly Cost

</div>

| Service | Cost |
|---|---|
| Gemini 2.5 Flash (Google OAuth free tier) | **0 €** |
| Electricity — Beelink ~12W idle, 24/7 | **~1–2 €** |
| **Total** | **~1–2 €/month** |

---

<div align="center">

## 🗺️ Roadmap

</div>

- [x] OpenClaw running as systemd service
- [x] Telegram bot responding via Gemini 2.5 Flash (free)
- [ ] Telegram pairing — restrict access to owner only
- [ ] Enable skills: `notion`, `github`, `weather`, `summarize`
- [ ] Add n8n for automation flows
- [ ] Local model fallback via Ollama on main PC (RX 9070 XT · 16GB VRAM)
- [ ] Multi-agent setup: projects agent, ideas agent, teaching agent
- [ ] Voice interface integration

---

<div align="center">

## 🤝 Contributing

Issues and pull requests are welcome.  
If you replicate this setup, share it — would love to see other configs.

---

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

---

<br/>

Part of **[HEO-AGENT](https://github.com/HEO-80/heo-agent)** · Built by [HEO-80](https://github.com/HEO-80) · [LinkedIn](https://linkedin.com/in/hectorob)

<br/>

</div>

Configuración de un servidor de IA personal 24/7 sobre un **Beelink SER5 MAX** usando [OpenClaw](https://github.com/openclaw/openclaw), con acceso desde Telegram y modelo **Gemini 2.5 Flash** gratuito via Google OAuth.

> Parte del proyecto **HEO-AGENT** — construcción documentada de un asistente de IA personal autónomo.

---

## Hardware

| Componente | Detalle |
|---|---|
| Dispositivo | Beelink SER5 MAX Mini PC |
| CPU | AMD Ryzen 7 6800U (8C/16T, hasta 4.7GHz) |
| RAM | 24 GB |
| Almacenamiento | SSD 500 GB |
| SO | Ubuntu 24.04 LTS |
| Acceso | SSH remoto |

---

## Stack

```
Telegram (móvil)
      ↕
Beelink — OpenClaw Gateway (systemd, puerto 18789)
      ↕
Google Gemini 2.5 Flash (OAuth gratuito, sin tarjeta)
```

---

## Requisitos previos

- Ubuntu 22.04+ / Debian
- Node.js v22+ (via [nvm](https://github.com/nvm-sh/nvm))
- npm 9+
- Bot de Telegram creado via [@BotFather](https://t.me/BotFather)
- Cuenta de Google (para OAuth gratuito)

---

## Instalación

### 1. Instalar Node 22 via nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22
```

### 2. Instalar OpenClaw globalmente

```bash
sudo npm install -g openclaw@latest --legacy-peer-deps
openclaw --version
```

### 3. Configurar el bot de Telegram

Edita `~/.openclaw/openclaw.json` y añade tu bot token:

```json
{
  "channels": {
    "telegram": {
      "botToken": "TU_BOT_TOKEN_AQUI",
      "enabled": true
    }
  }
}
```

> ⚠️ Nunca subas este archivo al repo. Contiene credenciales.

### 4. Autenticar con Google (modelo gratuito)

```bash
# Habilitar el plugin de autenticación
openclaw plugins enable google-gemini-cli-auth

# Login OAuth (necesitas navegador)
openclaw models auth login --provider google-gemini-cli-auth --set-default

# Cambiar al modelo Flash (más cuota gratuita)
openclaw models set google-gemini-cli/gemini-2.5-flash
```

El comando de login te dará una URL de Google. Autorizas en el navegador y pegas la URL de callback de vuelta en el terminal.

> Si el servidor es remoto, abre un túnel SSH antes:
> ```bash
> ssh -L 8085:localhost:8085 usuario@ip-del-servidor
> ```

### 5. Instalar el servicio systemd

```bash
# Copiar el archivo de servicio
cp openclaw-gateway.service ~/.config/systemd/user/

# Activar y arrancar
systemctl --user daemon-reload
systemctl --user enable openclaw-gateway
systemctl --user start openclaw-gateway

# Verificar estado
systemctl --user status openclaw-gateway
```

---

## Gestión del gateway

```bash
# Ver estado
systemctl --user status openclaw-gateway

# Ver logs en tiempo real
journalctl --user -u openclaw-gateway -f

# Reiniciar
systemctl --user restart openclaw-gateway

# Parar correctamente
openclaw gateway stop
```

---

## Estructura del repo

```
beelink-ai-server/
├── README.md
├── openclaw-gateway.service   # Servicio systemd
├── docker-compose.monitoring.yml  # Stack de monitoring opcional
└── .gitignore
```

---

## Coste mensual

| Servicio | Coste |
|---|---|
| Gemini 2.5 Flash (OAuth gratuito) | 0 € |
| Electricidad Beelink (~10-15W idle) | ~1-2 € |
| **Total** | **~1-2 €/mes** |

---

## Próximos pasos

- [ ] Activar skills: `notion`, `github`, `weather`, `summarize`
- [ ] Configurar pairing de Telegram (acceso solo para ti)
- [ ] Añadir n8n para flujos de automatización
- [ ] Integrar modelo local con Ollama desde el PC principal (RX 9070 XT, 16GB VRAM)
- [ ] Multi-agente: agente de proyectos, agente de ideas, agente de clase

---

## Parte de

Este repo es la **Fase 1** del proyecto HEO-AGENT: construcción de un ecosistema de agentes de IA personales documentado públicamente.

- GitHub: [HEO-80](https://github.com/HEO-80)
- LinkedIn: [hectorob](https://linkedin.com/in/hectorob)
