# Inicio en NaN.builders — De 0 a Agente IA con Voz

Una guía paso a paso para la gente normal (no ingenieros de NASA) que ha pagado NaN.builders y quiere tener su propio agente de inteligencia artificial funcionando, con voz, que pueda investigar cosas, acceder a APIs, desplegar webs, y ser su segundo cerebro.

Escrito por [@Ntizar](https://github.com/Ntizar) después de hacerlo él mismo.

---

## ¿Qué es NaN.builders para un mortal?

Imagina que alquilas un **ordenador pequeño en Internet** (una "microVM") que está encendido 24/7. Dentro de ese ordenador vive **Koldo** (o como quieras llamar a tu agente), que es como tener a ChatGPT pero funcionando en un sitio donde tú controlas todo: tus conversaciones, tus datos, tus claves, tus proyectos.

Además, tienes acceso a **modelos de IA potentes** (como DeepSeek) para que Koldo sea listo.

Cuesta ~74€/mes.

---

## Paso 1: Pagar y crear cuenta

1. Ve a [nan.builders](https://nan.builders)
2. Arriba a la derecha, **Get Started** o **Pricing**
3. Elige **NaN Member** (el que vale ~74€/mes)
4. Pagas con tarjeta de crédito/débito
5. Te llegará un email con acceso al dashboard

> 💡 **Tip:** Tras pagar, entras en el Discord de NaN.builders. La comunidad es maja, únete.

---

## Paso 2: Crear tu agente (microVM)

Una vez dentro del dashboard en [cloud.nan.builders](https://cloud.nan.builders/dashboard):

1. Busca un botón que ponga **New Agent** o **Create Agent**
2. Asígnale un nombre (ej: "mi-agente")
3. Selecciona la región **EU** (tus datos se quedan en Europa)
4. Haz clic en **Create**

⌛ Espera 1-2 minutos mientras se crea. Verás algo como:

```
Estado: ✅ Running
```

¡Enhorabuena! Ya tienes tu propio ordenador en Internet.

---

## Paso 3: Primer acceso — la consola

En el panel de tu agente, busca la pestaña **Console** (o terminal web).

Se abrirá una pantalla negra con texto blanco. Es la terminal de tu ordenador remoto. Escribe:

```bash
whoami
```

Te responderá `root`. Eres el administrador. Puedes hacer lo que quieras aquí.

> 💡 **Tip:** Si la consola se cuelga, recarga la página. Pasa a veces.

---

## Paso 4: Conectar Telegram

Tu agente vive dentro del ordenador, pero necesitas poder hablar con él desde el móvil. Para eso usamos **Telegram**.

1. En el panel de tu agente, busca la pestaña **Env** (Environment Variables)
2. Verás que ya hay dos variables:
   - `OPENAI_API_KEY` — la clave del clúster (no la toques)
   - `TELEGRAM_BOT_TOKEN` — aquí va el token de tu bot
3. Para conseguir el token:

   a. Abre Telegram y busca **@BotFather**
   b. Escribe `/newbot`
   c. Te pide nombre: ponle `Koldo` (o el que quieras)
   d. Te pide username: `TuNombreBot` (tiene que acabar en `bot`)
   e. BotFather te dará un token como: `7234567890:AAHdqTcvM3xCVBz-xyz...`
   f. Copia ese token

4. Vuelve a **cloud.nan.builders** > tu agente > **Env** tab
5. En la variable `TELEGRAM_BOT_TOKEN`, pega el token
6. Guarda los cambios
7. Reinicia el agente (suele haber un botón **Restart**)

> Puede tardar un minuto. Cuando se reinicie, abre Telegram, busca tu bot por el nombre que le pusiste, y escribe `/start` o "Hola".

🎉 **Ya puedes hablar con tu agente desde el móvil.**

---

## Paso 5: Primera conversación

Abre Telegram y escribe algo como:

```
Hola Koldo, ¿qué sabes hacer?
```

Te responderá. Ya estás hablando con tu agente de IA.

> 💡 **Tip:** No tengas miedo de hablarle informal. Responderá como tú le hables.

---

## Paso 6: Activar la voz 🎤🔊

Esto mola. Vas a poder mandarle **audios** y que te **responda con voz**. Como un walkie-talkie pero con IA.

Abre la **Console** de tu agente en cloud.nan.builders y pega esto:

```bash
source /opt/hermes/.venv/bin/activate
uv pip install faster-whisper
```

Espera 30 segundos mientras se instala. Cuando termine, escribe:

```bash
kill -TERM 1
```

Esto reinicia el agente (no te asustes, Kubernetes lo levanta solo en segundos).

Cuando vuelva a estar activo, necesitas restaurar la configuración de voz. Desde la consola escribe:

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

Ahora escribe en Telegram a tu agente:

```
Respóndeme en audio: prueba de voz
```

Te mandará un mensaje de voz. Dale al play. **Tu agente te habla.**

Para hablar tú, simplemente pulsa el micrófono de Telegram, graba un mensaje, y envíalo. El agente lo escuchará y te responderá.

> ⚠️ **Importante:** Si el agente se reinicia, la configuración de voz se pierde. Si pasa, repite el `cat` de arriba.

---

## Paso 7: Guardar tus claves de APIs de forma segura

Si quieres que tu agente pueda acceder a servicios externos (por ejemplo, datos de transporte público, electricidad, GitHub...), necesitas guardar las claves.

**Esto es crítico:** las claves NUNCA se escriben en repositorios de código. Se guardan como variables de entorno.

En el panel de cloud.nan.builders, pestaña **Env**, añade:

| Clave | Valor | Qué es |
|-------|-------|--------|
| `GITHUB_TOKEN` | `ghp_tu_token_aqui` | Para acceder a tus repos de GitHub |
| `NAP_API_KEY` | `tu-clave-nap` | Para datos de transporte público |
| `ESIOS_API_TOKEN` | `tu-clave-esios` | Para datos eléctricos |

Cada servicio te da su propia clave. Guárdalas aquí y estarán seguras.

---

## Paso 8: Desplegar una web 🕸️

Tu agente no solo habla — también puede servir páginas web. Vas a crear una URL pública como `tudashboard.apps.nan.builders`

1. En cloud.nan.builders > tu agente, pestaña **Web** o **HTTP**
2. Añade una nueva exposición:
   - **Puerto interno:** `3000`
   - **Subdominio:** el nombre que quieras (ej: `nap`)

3. Ahora desde la **Console**, prepara un proyecto web:

```bash
# Ejemplo: un dashboard simple
cd /persist
git clone https://github.com/tuusuario/tu-repo
cd tu-repo
npm install
npm run build
```

4. Arranca el servidor:

```bash
cd /persist/tu-repo && node server.js &
```

5. Tu web estará viva en `https://tudashboard.apps.nan.builders`

> 💡 **Tip:** Si la web no carga, asegúrate de que el servidor está en el puerto 3000 (el que pusiste en la exposición).

---

## Paso 9: Tu repositorio personal de configuración

Crea un repositorio PRIVADO en GitHub para guardar las skills de tu agente, notas y configuraciones:

```bash
source /root/.env   # carga tu token de GitHub
git clone https://github.com/tuusuario/mi-repo-privado.git
```

Ahí guardarás todo lo que tu agente va aprendiendo.

---

## Resumen visual de lo que tienes

```
📱 Tú (Telegram móvil)
   │
   ▼ audios y textos
🎙️ Koldo (tu agente IA en el microVM de NaN)
   │
   ├── 🤖 Cerebro: DeepSeek (modelo IA)
   ├── 🔧 Herramientas: terminal, archivos, web
   ├── 🎤 Oídos: faster-whisper (transcribe audios)
   ├── 🗣️ Boca: Edge TTS (genera voz)
   ├── 🔐 Caja fuerte: variables de entorno (APIs)
   └── 🕸️ Web: tudashboard.apps.nan.builders
```

---

## Lo que **NO** debes hacer

❌ **Meter claves en el código** → usar Env vars  
❌ **Compartir tu token de bot** → cualquiera hablaría con tu agente  
❌ **Olvidarte de la config de voz al reiniciar** → guarda el comando `cat` de antes  
❌ **Hacer `rm -rf` en la raíz** → te cargas todo

---

## Siguientes pasos

- Aprende a usar Git desde la consola
- Prueba a que tu agente te busque cosas en Internet
- Conéctalo a APIs de datos públicos
- Que despliegue webs el solo con un mensaje
- Únete al Discord de NaN y pregunta dudas

---

> Creado el 23 de mayo de 2026 por @Ntizar  
> NaN.builders Member EU  
> Preguntas: abre un Issue en este repo