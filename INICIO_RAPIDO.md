# Guía de Inicio Rápido

Esta guía te ayudará a configurar y ejecutar el agente de Telegram en menos de 10 minutos.

## Paso 1: Crear un Bot de Telegram

1. Abre Telegram en tu teléfono o computadora
2. Busca el usuario `@BotFather`
3. Inicia una conversación y envía el comando `/newbot`
4. BotFather te pedirá un nombre para tu bot. Ejemplo: `Mediaciones Bot`
5. Luego te pedirá un username (debe terminar en 'bot'). Ejemplo: `mediaciones_familia_bot`
6. BotFather te dará un **token**. Guárdalo, lo necesitarás en el siguiente paso.

Ejemplo de token: `1234567890:ABCdefGHIjklMNOpqrsTUVwxyz`

## Paso 2: Obtener API Key de OpenAI

1. Ve a https://platform.openai.com/api-keys
2. Inicia sesión o crea una cuenta
3. Haz clic en "Create new secret key"
4. Copia la API key (solo se muestra una vez)
5. Asegúrate de tener crédito en tu cuenta de OpenAI

## Paso 3: Instalar Dependencias

```bash
npm install
```

## Paso 4: Configurar Variables de Entorno

1. Crea un archivo `.env` en la raíz del proyecto:
```bash
cp .env.example .env
```

2. Edita el archivo `.env` con tus credenciales:
```env
TELEGRAM_BOT_TOKEN=pega_aqui_tu_token_de_telegram
OPENAI_API_KEY=pega_aqui_tu_api_key_de_openai
PORT=3000
CONVERSATIONS_FILE=./conversations.json
```

## Paso 5: Iniciar el Servidor

```bash
npm run dev
```

Deberías ver en la consola:
```
Bot de Telegram iniciado correctamente
Servidor Express escuchando en puerto 3000
```

## Paso 6: Probar el Bot

1. En Telegram, busca tu bot por el username que le diste (ej: `@mediaciones_familia_bot`)
2. Presiona el botón "Start" o envía `/start`
3. El bot te responderá: "¡Hola! Soy tu asistente de mediaciones familiares..."
4. Prueba escribirle algo como: "¿Qué haces?"

## Paso 7: Obtener tu Chat ID

Para usar el endpoint de API, necesitas tu Chat ID de Telegram:

1. Con el bot aún ejecutándose, abre tu navegador
2. Ve a: http://localhost:3000/api/conversations
3. Verás un JSON con las conversaciones. Tu Chat ID es el número largo que aparece como clave del objeto.

Ejemplo:
```json
{
  "123456789": {
    "userId": "123456789",
    ...
  }
}
```

En este caso, tu Chat ID es `123456789`.

## Paso 8: Probar el Endpoint de Mediación

Ahora puedes enviar mensajes de citas de mediación usando el API:

### Opción A: Con curl (Terminal)

```bash
curl -X POST http://localhost:3000/api/send-mediation \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "TU_CHAT_ID_AQUI",
    "appointmentData": {
      "nombre": "María González",
      "fecha": "2025-12-15",
      "hora": "14:30",
      "lugar": "Centro de Mediación, Calle Principal 456",
      "mediador": "Dr. Carlos Ramírez"
    }
  }'
```

Reemplaza `TU_CHAT_ID_AQUI` con el Chat ID que obtuviste en el paso 7.

### Opción B: Con el archivo examples.http (VS Code)

1. Abre el archivo `examples.http`
2. Reemplaza `TU_CHAT_ID_AQUI` con tu Chat ID real
3. Si tienes la extensión "REST Client" instalada, verás botones "Send Request"
4. Haz clic en el botón para enviar la petición

## Verificación

Después de enviar la petición, deberías:
1. Recibir un mensaje en Telegram con la información de la cita
2. Ver una respuesta JSON exitosa en la terminal/VS Code

## Solución de Problemas Comunes

### "Error: No se pudo enviar el mensaje"
- Asegúrate de haber iniciado una conversación con el bot primero (enviar `/start`)
- Verifica que el Chat ID sea correcto

### "Error al comunicarse con OpenAI"
- Verifica que tu API Key sea correcta
- Asegúrate de tener crédito en tu cuenta de OpenAI
- Revisa tu conexión a internet

### "Cannot find module..."
- Ejecuta `npm install` nuevamente
- Verifica que estés en el directorio correcto del proyecto

### El bot no responde
- Verifica que el token de Telegram sea correcto
- Asegúrate de que el servidor esté ejecutándose (`npm run dev`)
- Revisa la consola para ver mensajes de error

## Próximos Pasos

Una vez que todo funcione:

1. Revisa la documentación completa en `README.md`
2. Explora los diferentes endpoints del API
3. Personaliza los mensajes del bot editando `src/services/openaiService.ts`
4. Considera migrar de JSON a una base de datos para producción

## Comandos Útiles

```bash
# Iniciar en modo desarrollo (recarga automática)
npm run dev

# Compilar TypeScript a JavaScript
npm run build

# Ejecutar versión compilada
npm start

# Ver todas las conversaciones
curl http://localhost:3000/api/conversations

# Health check del servidor
curl http://localhost:3000/health
```

## ¿Necesitas Ayuda?

Si encuentras problemas:
1. Revisa la sección "Solución de Problemas" en `README.md`
2. Verifica los logs en la consola donde ejecutaste `npm run dev`
3. Asegúrate de que todas las variables de entorno estén configuradas correctamente
