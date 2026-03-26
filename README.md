# 🦞 beelink-ai-server

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
