# Inicio en NaN.builders

Una guía paso a paso para la gente que no sabe por dónde empezar.

---

## ¿Qué es esto?

NaN.builders es un servicio donde alquilas un **ordenador pequeño en Internet** (lo llaman "agente" o "microVM") que está siempre encendido. Dentro de ese ordenador puedes tener un **asistente de inteligencia artificial** que habla contigo, investiga cosas por ti, y hasta puede servir páginas web.

El asistente no es ChatGPT ni nada de fuera — **vive en tu ordenador**. Tú controlas todo.

---

## Paso 1: Registrarse

1. Ve a [nan.builders](https://nan.builders)
2. Pincha en **Plans** o **Pricing**
3. Elige el plan **NaN Member** (el que incluye el agente)
4. Te pedirá tarjeta de crédito
5. Cuando pagues, te llegará un email con un enlace al dashboard

> 💡 **Nota:** Si no sabes inglés, no pasa nada. La web tiene un diseño visual, puedes guiarte por los dibujos.

---

## Paso 2: Crear tu ordenador (el "agente")

Una vez dentro del panel de control en [cloud.nan.builders](https://cloud.nan.builders/dashboard):

1. Aparecerá un botón grande que pone **New Agent** o **Create**
2. Dale un nombre (por ejemplo: `mi-asistente`)
3. Elige región **EU** — tus datos se quedan en Europa
4. Pincha en **Create** o **Deploy**

Aparecerá un círculo que gira. Espera. Cuando se ponga verde y ponga **Running**, ya está.

Acabas de alquilar un ordenador. Enhorabuena 🎉

---

## Paso 3: La consola (el teclado de tu ordenador)

En el panel de control de tu agente, busca una pestaña llamada **Console**. Puede tener un icono de pantalla o de terminal.

Se abrirá una ventana negra con letras blancas. Es como tener el teclado de tu ordenador remoto.

Puedes escribir comandos aquí. Prueba a escribir:

```
whoami
```

Te responderá `root`. Eso significa que eres el dueño. No pasa nada si la primera vez no entiendes nada — iremos paso a paso.

> 💡 **Tip:** Si la pantalla se queda congelada, cierra la pestaña y vuelve a abrir la consola.

---

## Paso 4: Conectar tu asistente con Telegram (tu móvil)

Ahora tu asistente vive dentro del ordenador, pero no puedes hablar con él todavía. Necesitas un **teléfono** para chatear. Telegram es la aplicación que usaremos.

Si no tienes Telegram, instálala desde Google Play o App Store. Es gratis.

### 4.1 Crear un bot en Telegram

Telegram tiene un usuario especial llamado **BotFather**. Es quien crea bots.

1. Abre Telegram
2. Busca **@BotFather** en el buscador (la lupa)
3. Entra en su chat y escribe: `/newbot`
4. BotFather te preguntará el **nombre** de tu bot. Pon el que quieras, por ejemplo: `Mi Asistente`
5. Luego te pedirá un **username**. Tiene que acabar en `bot`. Por ejemplo: `MiAsistenteBot`
6. Si el nombre está libre, BotFather te dará un **token**. Es una cadena larga de letras y números como esta:

```
7234567890:AAHdqTcvM3xCVBz-NTcf4H7D7E
```

**Ese token es la llave de tu bot.** Sin ella no funciona. Cópiala y no la compartas con nadie.

### 4.2 Poner el token en tu ordenador

1. Ve a [cloud.nan.builders](https://cloud.nan.builders/dashboard)
2. Pincha en tu agente
3. Busca la pestaña **Env** (puede poner "Environment" o "Variables")
4. Verás una variable que se llama `TELEGRAM_BOT_TOKEN`
5. Pega ahí el token que te dio BotFather (el número largo)
6. Guarda los cambios (suele haber un botón **Save**)

### 4.3 Reiniciar

Busca un botón que ponga **Restart** o **Reboot**. Púlsalo.

Espera un minuto a que se reinicie. Cuando vuelva a estar **Running**:

1. Abre Telegram
2. Busca tu bot por el nombre que le pusiste
3. Entra en su chat y escribe: `Hola`

Te responderá. **Ya puedes hablar con tu asistente desde el móvil** 📱

---

## Paso 5: Darle voz 🎤🔊

Esto es lo mejor. Tu asistente puede **escuchar audios** y **responderte con voz**. Como un walkie-talkie con superpoderes.

### 5.1 Instalar el oído (STT)

Abre la **Console** de tu agente (la pantalla negra).

Copia y pega este texto (pincha en el icono para copiarlo):

```bash
source /opt/hermes/.venv/bin/activate
uv pip install faster-whisper
```

Espera unos segundos. Cuando termine de escribir cosas, estará instalado.

### 5.2 Configurar la voz

Ahora pega esto:

```bash
cat > /hermes-home/config.yaml << 'EOF'
model:
  default: "deepseek-v4-flash"
  provider: custom
  base_url: "https://api.nan.builders/v1"
stt:
  enabled: true
  provider: local
  local:
    model: tiny
tts:
  provider: edge
  edge:
    voice: es-ES-AlvaroNeural
EOF
```

### 5.3 Reiniciar para que coja los cambios

Pega esto:

```bash
kill -TERM 1
```

No te asustes. El ordenador se reinicia solo (lo gestiona NaN). Espera 30 segundos.

### 5.4 Probar

Abre Telegram.

**Para que te hable:**
Escribe: `Respóndeme en audio: hola, ¿qué tal?`
Te mandará un mensaje de voz. Pulsa play y te escucharás.

**Para hablarle tú:**
Pulsa el micrófono de Telegram, graba lo que quieras, y envíalo. 
Tu asistente lo escuchará y te contestará.

> ⚠️ **Importante:** Si el agente se reinicia alguna vez (por cualquier motivo), se te olvida la configuración de la voz. Solo tienes que volver al paso 5.2 (el comando `cat`) y repetirlo. No pasa nada.

---

## Paso 6: Guardar claves para servicios externos

Tu asistente puede usar servicios de fuera: datos del gobierno, APIs del tiempo, lo que sea. Para eso necesita **claves** (como contraseñas).

**Esto es muy importante:** las claves nunca se escriben en el código. Se guardan en un sitio seguro.

### Dónde guardarlas

En el panel de cloud.nan.builders, pestaña **Env** (la misma donde pusiste el token del bot).

Allí puedes añadir variables. Por ejemplo:

| Clave (nombre) | Valor (lo que te den) |
|-------|--------|
| `MI_API_KEY` | `abc123def456...` |

Cada servicio te dará una clave distinta. Las pones aquí y tu asistente podrá usarlas.

---

## Paso 7: Publicar una web 🕸️

Tu ordenador también puede servir páginas web. Vas a conseguir una dirección como `tumola.apps.nan.builders`

### 7.1 Preparar la exposición web

En cloud.nan.builders > tu agente, busca la pestaña **Web** o **HTTP Exposure**.

Añade una entrada:

- **Puerto:** `3000`
- **Subdominio:** el nombre que quieras (ej: `miproyecto`)

Guarda. Ahora `miproyecto.apps.nan.builders` apunta a tu ordenador.

### 7.2 Subir un proyecto web

Abre la **Console** y escribe:

```bash
cd /persist
git clone https://github.com/tu-usuario/tu-repo
cd tu-repo
```

Si tu proyecto necesita instalarse:

```bash
npm install
npm run build
```

Luego arrancas el servidor:

```bash
node server.js &
```

Y tu web estará viva en `miproyecto.apps.nan.builders` 🌐

> No sabes qué proyecto poner? Crea un repositorio en GitHub con un `index.html` básico y clónalo. Luego sirve el archivo.

---

## Lo que NO debes hacer

❌ **Compartir el token de tu bot** — cualquiera que lo tenga podría hablar con tu asistente  
❌ **Meter claves de APIs en el código** — usa las variables de entorno (Env tab)  
❌ **Poner passwords o tokens en repositorios públicos** — aunque sean privados, mejor no  
❌ **Hacer `rm -rf /`** — te cargas el ordenador entero  

---

## No entiendo algo

Este tutorial es abierto. Si algo no se entiende, puedes:

- Preguntar en el Discord de NaN.builders (la gente ayuda)
- Buscar en Google "qué es un comando en Linux"
- Leer el párrafo otra vez más despacio

Todo lo que aquí se explica se hace desde el navegador (cloud.nan.builders) y Telegram. No necesitas instalar nada en tu ordenador personal.

---

## Glosario para novatos

| Palabra | Significa |
|---------|-----------|
| **Agente** | Tu ordenador remoto + el asistente que vive dentro |
| **MicroVM** | El ordenador pequeño en Internet |
| **Console** | La pantalla negra donde escribes órdenes |
| **Terminal** | Lo mismo que consola |
| **Comando** | Una orden que le das al ordenador escribiendo |
| **Token** | Una contraseña larga para conectar servicios |
| **Bot** | Un programa que habla por Telegram |
| **STT** | Speech-to-text: convertir voz en texto (que te escuche) |
| **TTS** | Text-to-speech: convertir texto en voz (que te hable) |
| **Env** | Environment: las variables de entorno, donde guardas claves |
| **Repo / Repositorio** | Una carpeta de código en GitHub |
| **Push** | Subir código a GitHub |
| **Clone / Clonar** | Descargar un repositorio a tu ordenador |
| **Build** | Compilar / preparar el código para funcionar |
| **Servidor** | Un programa que se queda esperando peticiones |
| **Puerto** | Un número que identifica un servicio (como el 3000) |

---

> Hecho por alguien que empezó sin saber nada  
> NaN.builders Member — 2026