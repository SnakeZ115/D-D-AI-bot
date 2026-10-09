# 🐲 D&D AI Dungeon Master & Companion Bot

Un bot de Discord impulsado por **IA Local (Ollama)** diseñado para actuar como asistente de **Dungeons & Dragons (D&D 5e)**, narrador de historias (Dungeon Master assistant), generador de encuentros, NPCs y consulta de reglas en tiempo real.

---

## 📐 Arquitectura del Proyecto

El proyecto está diseñado siguiendo una arquitectura modular y desacoplada para garantizar escalabilidad, facilidad de mantenimiento y seguridad en la gestión de credenciales.

```text
  [ Jugadores en Discord ]
             │
             ▼
   [ Discord Client (bot/) ]
     ├── Carga variables ──► [ .env (Local / No versionado) ]
     └── Procesa comandos / eventos
             │
             ▼
   [ Services Layer (services/) ]
     ├── ai_service.py ──► Prompt engineering & llamadas asíncronas
     └── dice_service.py ──► Lógica de tirada de dados (d4, d6, d20, d100)
             │
             ▼ (HTTP / API REST localhost:11434)
   [ Ollama Engine (Modelo Local) ]
     └── Llama 3.2 / Mistral / Gemma 2 con System Prompt de D&D
```

---

## 📁 Estructura del Repositorio

```text
dnd-discord-ai-bot/
├── .env                  # ⚠️ Credenciales locales (NUNCA SUBIR A GIT)
├── .env.example          # Plantilla de variables de entorno públicas
├── .gitignore            # Archivos ignorados por Git
├── README.md             # Documentación general
├── requirements.txt      # Dependencias del proyecto
└── src/
    ├── config.py         # Validación y carga de variables de entorno
    ├── main.py           # Punto de entrada de la aplicación
    ├── bot/
    │   ├── client.py     # Inicialización e intenciones del bot
    │   └── cogs/         # Comandos divididos por categorías
    │       ├── ai_dm.py  # Interacción con la IA narradora
    │       └── dice.py   # Sistema de tirada de dados
    └── services/
        ├── ai_service.py # Interfaz de comunicación con Ollama
        └── prompts.py    # System Prompts y personalidades de D&D
```

---

## 🔒 Configuración de Seguridad y Variables de Entorno

### 1. Archivo `.env` (Local)
Crea un archivo llamado `.env` en la raíz de tu proyecto con tus credenciales:

```env
DISCORD_TOKEN=tu_token_secreto_de_discord_aqui
OLLAMA_HOST=http://localhost:11434
MODEL_NAME=llama3.2
SYSTEM_PROMPT_TYPE=dungeon_master
```

### 2. Archivo `.env.example` (Plantilla pública)
Este archivo sí se incluye en el control de versiones para guiar a otros desarrolladores:

```env
DISCORD_TOKEN=tu_discord_bot_token_aqui
OLLAMA_HOST=http://localhost:11434
MODEL_NAME=llama3.2
SYSTEM_PROMPT_TYPE=dungeon_master
```

### 3. Archivo `.gitignore`
Asegúrate de incluir las siguientes directivas para evitar filtrar claves secretas o archivos innecesarios:

```gitignore
# Secretos y Entorno
.env
*.env

# Entornos Virtuales
venv/
.venv/
env/

# Caché de Python
__pycache__/
*.py[cod]

# Logs
*.log
```

---

## 🚀 Requisitos e Instalación

### Requisitos Previos
1. **Python 3.10+**
2. **Ollama** instalado y corriendo localmente ([ollama.com](https://ollama.com/))
3. Un modelo de IA descargado en Ollama (ej. `ollama run llama3.2`)

### Pasos de Instalación

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/tu-usuario/dnd-discord-ai-bot.git
   cd dnd-discord-ai-bot
   ```

2. **Crear y activar entorno virtual:**
   ```bash
   python -m venv venv
   # En Windows:
   venv\Scripts\activate
   # En Linux/macOS:
   source venv/bin/activate
   ```

3. **Instalar dependencias:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configurar el archivo `.env`:**
   Copia `.env.example` a `.env` y coloca tu `DISCORD_TOKEN`.

5. **Ejecutar el bot:**
   ```bash
   python src/main.py
   ```

---

## 🛠️ Funcionalidades de D&D Planificadas

* **`/narrar [acción]`**: Pide a la IA local que describa el resultado de una acción o la ambientación de una taberna, mazmorra o combate.
* **`/npc [raza] [clase]`**: Genera al instante un NPC con trasfondo, personalidad y motivación según las reglas de D&D 5e.
* **`/regla [consulta]`**: Consulta rápida sobre reglas, hechizos o estados de D&D 5e.
* **`/r [fórmula]`**: Sistema integrado para tirar dados de rol (ejemplo: `/r 1d20+5` o `/r 2d6+3`).