# Web Agent N8N - Sistema de Chat Conversacional Inteligente

Agente conversacional inteligente para sitios web que proporciona atención automatizada 24/7 usando n8n, OpenAI y memoria contextual.

## 📋 Descripción

Sistema completo de automatización de chat web que permite a los visitantes de tu sitio obtener respuestas instantáneas, agendar citas y recibir atención personalizada mediante conversaciones naturales. El agente procesa mensajes de texto, mantiene contexto conversacional, captura leads automáticamente y deriva a humanos cuando es necesario.

## ✨ Características Principales

- **Conversación Natural**: Interacción fluida usando GPT-4/Gemini para comprensión de intenciones
- **Atención 24/7**: Disponibilidad continua sin límites de horario
- **Memoria Contextual**: Mantiene el historial de conversación con Redis
- **Búsqueda Semántica**: Utiliza Qdrant para respuestas basadas en conocimiento del negocio
- **Captura de Leads**: Registro automático de datos de visitantes interesados
- **Gestión de Citas**: Reserva, consulta y modificación de citas directamente desde el chat
- **Widget Personalizable**: Diseño adaptable a la identidad visual de tu marca
- **Integración MCP**: Conecta con calendario, email y otros servicios externos
- **Multi-idioma**: Soporte para español e inglés

## 🏗️ Arquitectura del Sistema

```
Visitante Web
    ↓
Widget de Chat (Frontend)
    ↓
Webhook n8n
    ↓
Cola de Mensajes (Redis)
    ↓
├── Recuperación de Contexto (Redis)
├── Búsqueda en Base de Conocimiento (Qdrant)
├── Procesamiento IA (Google Gemini/OpenAI)
├── Integración MCP (Calendar, Email)
└── Captura de Leads (Google Sheets)
    ↓
Respuesta al Visitante
```

## 🛠️ Stack Tecnológico

- **n8n**: Orquestación de workflows
- **Google AI (Gemini)**: Procesamiento de lenguaje natural (primary)
- **OpenAI GPT-4**: Modelo alternativo de IA
- **Redis**: Gestión de sesiones y colas
- **Qdrant**: Base de datos vectorial para búsqueda semántica
- **Google Sheets**: Almacenamiento de leads
- **MCP Services**: Integración con calendario y email
- **Docker**: Contenedorización de servicios

## 📦 Requisitos Previos

- Docker y Docker Compose instalados
- Node.js 18+ (para n8n)
- Cuenta de Google AI Studio o OpenAI con créditos disponibles
- n8n (self-hosted o cloud)
- Dominio con certificado SSL (para webhook HTTPS)
- Mínimo 2GB RAM disponible

## 🚀 Instalación

### Paso 1: Clonar el repositorio

```bash
git clone https://github.com/tuusuario/web-agent-n8n.git
cd web-agent-n8n
```

### Paso 2: Configurar variables de entorno

```bash
cp .env.example .env
```

Edita el archivo `.env` con tus credenciales:

```bash
# n8n Configuration
N8N_HOST=http://localhost:5678
N8N_WEBHOOK_URL=https://tu-dominio.com/webhook/AGENTE_WEB

# Google AI (Primary)
GOOGLE_AI_API_KEY=tu-api-key-google
GOOGLE_AI_MODEL=gemini-pro

# OpenAI (Alternative)
OPENAI_API_KEY=sk-tu-api-key-openai

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=tu-password-seguro

# Qdrant
QDRANT_HOST=localhost
QDRANT_PORT=6333
QDRANT_API_KEY=tu-api-key-qdrant

# Google Sheets (Lead Capture)
GOOGLE_SHEETS_DOCUMENT_ID=tu-sheet-id
GOOGLE_SERVICE_ACCOUNT_EMAIL=tu-service-account@project.iam.gserviceaccount.com

# Configuración de Negocio
BUSINESS_NAME=Tu Negocio
BUSINESS_TIMEZONE=Europe/Madrid
BUSINESS_EMAIL=contact@tunegocio.com
```

### Paso 3: Iniciar servicios con Docker

```bash
docker-compose up -d
```

### Paso 4: Importar workflow en n8n

1. Accede a tu instancia de n8n: `http://localhost:5678`
2. Ve a **Workflows → Import from File**
3. Selecciona el archivo `workflows/web-agent-workflow.json`
4. Configura las credenciales necesarias:
   - Google AI API Key o OpenAI API Key
   - Redis connection
   - Qdrant API Key
   - Google Sheets credentials
5. Activa el workflow

### Paso 5: Integrar widget en tu sitio web

Añade este código antes del cierre de la etiqueta `</body>` en tu HTML:

```html
<!-- Web Agent Chat Widget -->
<div id="chat-widget"></div>
<script src="https://tu-dominio.com/chat-widget.js"></script>
<script>
  ChatWidget.init({
    webhookUrl: 'https://tu-dominio-n8n.com/webhook/AGENTE_WEB',
    primaryColor: '#007bff',
    position: 'bottom-right',
    greeting: '¡Hola! ¿En qué puedo ayudarte?'
  });
</script>
```

## 📁 Estructura del Proyecto

```
web-agent-n8n/
├── workflows/
│   ├── web-agent-workflow.json           # Workflow principal
│   └── lead-notification.json            # Notificaciones de leads
├── docs/
│   ├── INSTALLATION.md                   # Guía de instalación detallada
│   ├── CONFIGURATION.md                  # Configuración avanzada
│   ├── TECHNICAL.md                      # Documentación técnica
│   └── TROUBLESHOOTING.md                # Solución de problemas
├── scripts/
│   ├── setup-qdrant.sh                   # Script de inicialización Qdrant
│   └── test-webhook.sh                   # Prueba de webhook
├── examples/
│   ├── chat-widget.html                  # Ejemplo de integración
│   └── chat-client.js                    # Cliente JavaScript
├── screenshots/                           # Capturas de ejemplo
├── docker-compose.yml                     # Configuración Docker
├── .env.example                           # Plantilla de variables
├── .gitignore
├── LICENSE
└── README.md
```

## 💬 Ejemplo de Uso

**Conversación típica de un visitante:**

```
Visitante: Hola, quisiera saber sobre sus servicios de consultoría

Bot: ¡Hola! Encantado de ayudarte. Ofrecemos consultoría especializada en:
     • Automatización de procesos con IA
     • Integración de agentes conversacionales
     • Optimización de workflows empresariales
     
     ¿Sobre cuál te gustaría más información?

Visitante: Automatización de procesos

Bot: Perfecto. Nuestra automatización de procesos te permite:
     ✅ Reducir tareas manuales hasta un 80%
     ✅ Integrar sistemas existentes
     ✅ Escalar operaciones sin aumentar personal
     
     ¿Te gustaría agendar una consulta gratuita de 30 minutos?

Visitante: Sí, me interesa

Bot: Excelente. Para agendar tu consulta necesito:
     • Nombre completo
     • Email
     • Teléfono (opcional)
     • Preferencia de horario
     
     ¿Cómo te llamas?

Visitante: Juan Pérez, mi email es juan@empresa.com

Bot: Gracias Juan. ¿Qué horario prefieres?
     Disponibilidad esta semana:
     • Martes 10:00 - 12:00
     • Miércoles 15:00 - 18:00
     • Jueves 10:00 - 14:00

Visitante: Miércoles a las 15:00

Bot: ¡Perfecto! ✅
     
     📅 Consulta confirmada:
     • Fecha: Miércoles 4 de diciembre
     • Hora: 15:00 (30 minutos)
     • Con: Equipo de consultoría
     
     Te enviaremos un email de confirmación a juan@empresa.com
     con el enlace de la videollamada.
     
     ¿Hay algo más en lo que pueda ayudarte?
```

## 🔧 Configuración Avanzada

### Personalización del Comportamiento del Agente

El comportamiento del agente se puede personalizar editando el prompt en el nodo "AI Agent" del workflow:

```javascript
// Personaliza el tono y estilo
BUSINESS_TONE: "profesional y cercano"
BUSINESS_STYLE: "formal/informal/casual"

// Define respuestas rápidas
QUICK_REPLIES: [
  "Ver servicios",
  "Agendar consulta",
  "Hablar con humano"
]

// Configura derivación a humano
ESCALATION_TRIGGERS: [
  "hablar con persona",
  "atención personalizada",
  "precio especial"
]
```

### Configuración del Widget

Personaliza el aspecto del widget en el archivo de configuración:

```javascript
ChatWidget.init({
  // Colores
  primaryColor: '#007bff',
  secondaryColor: '#6c757d',
  
  // Posición
  position: 'bottom-right', // bottom-left, top-right, top-left
  
  // Mensajes
  greeting: '¡Hola! ¿En qué puedo ayudarte?',
  placeholder: 'Escribe tu mensaje...',
  title: 'Asistente Virtual',
  
  // Comportamiento
  autoOpen: false,
  autoOpenDelay: 5000, // milisegundos
  soundEnabled: true
});
```

## 🐛 Troubleshooting

### El chat no aparece en el sitio web

**Verificar:**
- El script está correctamente incluido en el HTML
- La URL del webhook es correcta y accesible
- No hay errores en la consola del navegador
- El workflow está activado en n8n

**Logs:**
```bash
# Ver logs del navegador
Console del navegador (F12)

# Ver logs de n8n
docker-compose logs -f n8n
```

### El bot no responde

**Verificar:**
- n8n está ejecutándose: `docker ps | grep n8n`
- Redis está activo: `docker-compose ps redis`
- Webhook está correctamente configurado
- La API key de Google AI/OpenAI es válida

**Prueba el webhook:**
```bash
./scripts/test-webhook.sh
```

### Error de conexión con Redis

```bash
# Reiniciar servicio Redis
docker-compose restart redis

# Verificar logs
docker-compose logs redis

# Probar conexión
redis-cli -h localhost -p 6379 PING
```

### Respuestas lentas o timeout

**Optimizaciones:**
- Aumentar `max_tokens` en configuración de IA
- Configurar timeout más alto en webhook (60s)
- Revisar uso de memoria de Redis
- Optimizar tamaño de colección en Qdrant
- Implementar caché para preguntas frecuentes

## 📊 Casos de Uso

- **E-commerce**: Atención al cliente, recomendaciones de productos, seguimiento de pedidos
- **SaaS**: Onboarding de usuarios, soporte técnico, demos de producto
- **Servicios Profesionales**: Consultoría, agendamiento de citas, calificación de leads
- **Educación**: Información de cursos, matriculación, tutorías
- **Inmobiliaria**: Consultas de propiedades, visitas, información de precios
- **Salud**: Agendamiento de citas, información de servicios, teleconsulta
- **Restaurantes**: Reservas, información de menú, delivery
- **Turismo**: Información de destinos, reservas, recomendaciones

## 🔒 Seguridad

- ✅ Todas las credenciales en variables de entorno
- ✅ Comunicación HTTPS obligatoria
- ✅ Tokens de autenticación para webhooks
- ✅ Validación de mensajes entrantes
- ✅ Rate limiting implementado
- ✅ Sanitización de inputs
- ✅ CORS configurado correctamente
- ✅ Logs de seguridad activados

## 📈 Roadmap

- [ ] Integración nativa con CRMs (HubSpot, Salesforce)
- [ ] Dashboard de analytics en tiempo real
- [ ] Soporte para más idiomas (francés, alemán, portugués)
- [ ] Transferencia en vivo a agentes humanos
- [ ] Análisis de sentimientos en conversaciones
- [ ] A/B testing de respuestas
- [ ] Exportación de conversaciones
- [ ] API REST para integración externa
- [ ] App móvil de gestión
- [ ] Integración con WhatsApp Business API

## 🤝 Contribuir

Las contribuciones son bienvenidas. Para contribuir:

1. Fork el proyecto
2. Crea una rama para tu feature: `git checkout -b feature/NuevaFuncionalidad`
3. Commit tus cambios: `git commit -m 'Añade nueva funcionalidad'`
4. Push a la rama: `git push origin feature/NuevaFuncionalidad`
5. Abre un Pull Request

## 📝 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

## 👨‍💻 Autor

**José Luis Zapata**

- 📍 Dos Hermanas, Sevilla, España
- 💼 AI Automation Specialist
- 🏢 José Luis Zapata IA - Consultancy

## 🙏 Agradecimientos

- [n8n.io](https://n8n.io) - Plataforma de automatización de workflows
- [Google AI Studio](https://ai.google.dev) - Gemini API
- [OpenAI](https://openai.com) - GPT-4 API
- [Qdrant](https://qdrant.tech) - Base de datos vectorial
- [Redis](https://redis.io) - Sistema de caché y colas

## 📞 Soporte

¿Necesitas ayuda o tienes preguntas?

- 📧 [Abre un Issue](https://github.com/tuusuario/web-agent-n8n/issues)
- 📚 Consulta la [documentación completa](docs/)
- 💬 Contacta directamente para implementaciones personalizadas

---

⭐ **Si este proyecto te resulta útil, dale una estrella en GitHub** ⭐

Desarrollado con ❤️ en Sevilla, España
