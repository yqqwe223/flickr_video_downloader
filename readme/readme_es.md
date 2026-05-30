# Flickr Video Downloader 📥

> Herramienta web sencilla para guardar vídeos públicos de Flickr en tu dispositivo. Sin complicaciones, sin rastreo, solo funciona.

## 👋 ¿Por qué creé esto?

Seamos honestos: a veces navegas por Flickr y encuentras un vídeo que realmente te sirve — un tutorial de fotografía, un clip de detrás de cámaras, o incluso algo que subiste tú mismo y ahora quieres respaldar. Pero, ¿descargarlo de forma sencilla? No siempre es fácil.

Por eso escribí esta herramienta, primero para mí, y luego pensé: "¿Por qué no compartirla?". Sin funciones innecesarias, sin seguimiento de usuarios, sin obligación de crear cuenta. Solo pegas un enlace público de Flickr, pulsas "Analizar", y si el vídeo es accesible, aparecen las opciones para descargarlo. Así de simple.

Todo el procesamiento ocurre en el servidor: no registro qué descargas, no guardo historiales, no recojo datos personales. Tu privacidad es tuya.

## ✨ Lo que realmente hace

- **Compatible con enlaces comunes de Flickr**: Funciona con álbumes públicos, páginas de vídeo de usuarios y enlaces compartidos directos — siempre que no sean privados ni estén protegidos con contraseña
- **Muestra las calidades disponibles**: Cuando Flickr ofrece varias resoluciones (Original, Alta, Estándar), puedes elegir cuál descargar
- **Sin necesidad de iniciar sesión**: Solo procesa contenido accesible públicamente; nunca pide tus credenciales de Flickr
- **Interfaz limpia y adaptable**: Se ve bien en móvil, tablet o escritorio sin necesidad de frameworks frontend pesados
- **Límite de peticiones básico**: Restringe automáticamente las solicitudes por IP para evitar abusos y mantener el servicio estable para todos
- **Procesamiento sin bloqueo**: Incluso al analizar vídeos largos, tu pestaña del navegador no se congela, la experiencia sigue fluida

## 🛠 Qué hay bajo el capó

| Capa | Tecnologías |
|------|-------------|
| Backend | Python 3.11, Django 4.2 LTS |
| Parsing | httpx, lxml, regex para extracción de metadatos |
| Frontend | HTML5 semántico, CSS3 ligero, JavaScript vanilla |
| Despliegue | Gunicorn + Nginx, compatible con Docker |
| Utilidades | python-dotenv, django-ratelimit, whitenoise |

Cero librerías de inteligencia artificial. Cero llamadas API externas que "llamen a casa". Solo peticiones HTTP estándar y un análisis HTML escrito con cuidado — código que puedes leer, entender y modificar sin dolores de cabeza.

## 🚀 Ejecútalo en tu máquina

### Lo que necesitas
- Python 3.10 o superior
- pip + venv (o virtualenv)
- Conocimiento básico de la estructura de proyectos Django

### Configuración para desarrollo

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/flickr-downloader-sp.git
cd flickr-downloader-sp

# Crear y activar entorno virtual
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# o .venv\Scripts\activate  # Windows

# Instalar dependencias
pip install -r requirements.txt

# Configurar variables de entorno
cp .env.example .env
# Editar .env con tus valores (SECRET_KEY, DEBUG, etc.)

# Ejecutar migraciones y arrancar servidor de desarrollo
python manage.py migrate
python manage.py runserver
```

Luego abre `http://127.0.0.1:8000` en tu navegador.

### Notas para producción

- Establece `DEBUG=False` y configura correctamente `ALLOWED_HOSTS` con tus dominios
- Ejecuta detrás de Nginx usando Gunicorn (o uWSGI si lo prefieres)
- Activa HTTPS a nivel de proxy
- Recopila archivos estáticos: `python manage.py collectstatic`
- Si el tráfico aumenta, considera añadir Redis para caché

Ejemplo de comando Gunicorn:
```bash
gunicorn config.wsgi:application \
  --bind 127.0.0.1:8000 \
  --workers 2 \
  --timeout 90
```

Si aumentas el número de workers, vigila el uso de memoria — el análisis de vídeo puede ser algo pesado.

## 📋 Cómo se usa

1. Busca una página pública de Flickr que contenga un vídeo (álbum, perfil de usuario o enlace compartido)
2. Copia la URL y pégala en el campo de entrada de la herramienta
3. Haz clic en "Analizar" — el backend extraerá los streams de vídeo disponibles
4. Si tiene éxito, aparecerán botones de descarga con etiquetas de resolución
5. Selecciona la opción que prefieras; el archivo se descargará a través de tu navegador

> Nota: Solo funcionan los vídeos accesibles públicamente. Álbumes privados, contenido solo para amigos, vídeos protegidos con contraseña o con restricciones regionales devolverán un error. Esto es intencional — la herramienta respeta la configuración de privacidad de Flickr.

## ⚠️ Por favor, lee esto

Esta herramienta está diseñada **exclusivamente para uso personal y no comercial**. Ejemplos de uso adecuado:
- Hacer copias de seguridad de vídeos que tú mismo subiste a Flickr
- Guardar contenido educativo o de referencia compartido públicamente para estudio offline
- Investigación o propósitos de accesibilidad dentro de las directrices de fair use

**Tú eres responsable de**:
- Cumplir con los [Términos de Servicio de Flickr](https://www.flickr.com/help/terms)
- Respetar los derechos de autor y licencias Creative Commons de los creadores
- Seguir la legislación aplicable en tu jurisdicción sobre copia de contenido digital

No superviso las descargas y no asumo responsabilidad por uso indebido. Por favor, no uses esta herramienta para:
- Scraping masivo o recolección automatizada de contenido
- Redistribuir material protegido por derechos de autor sin permiso explícito
- Eludir configuraciones de privacidad o controles de acceso
- Servicios comerciales o re-alojamiento sin autorización previa

Si tienes dudas sobre si tu caso de uso es apropiado, probablemente no lo sea. En caso de incertidumbre, consulta primero con el creador del contenido.

## 🤝 ¿Quieres echar una mano?

¿Encontraste un bug? ¿Crees que el parsing podría ser más robusto? ¿Tienes una idea para mejorar la interfaz? Las contribuciones son bienvenidas — sin barreras.

### Cómo contribuir
1. Haz un fork del repo y crea una rama para tu funcionalidad (`git checkout -b fix/interfaz-movil`)
2. Realiza cambios en commits pequeños y lógicos, con mensajes claros
3. Prueba en local — asegúrate de que las funcionalidades existentes siguen operativas
4. Abre un Pull Request con una descripción concisa de qué cambió y por qué

### Estilo de código
- Backend: Sigue PEP 8, añade type hints donde mejoren la legibilidad
- Frontend: Mantén JavaScript al mínimo; prioriza la mejora progresiva sobre frameworks pesados
- Commits: Usa prefijos convencionales (`feat:`, `fix:`, `docs:`, `chore:`, etc.)

### Reportar problemas
Al abrir un bug, por favor incluye:
- La URL de Flickr afectada (si es compartible)
- Nombre + versión del navegador, sistema operativo, tipo de dispositivo
- Pasos para reproducir el problema
- Comportamiento esperado vs. comportamiento observado

Las capturas de pantalla o logs de la consola también ayudan, especialmente para problemas frontend.

## 🔧 Opciones de configuración

| Variable | Propósito | Ejemplo |
|----------|-----------|---------|
| `DEBUG` | Activa/desactiva modo debug de Django | `False` |
| `SECRET_KEY` | Clave de seguridad de Django | `tu-cadena-aleatoria-segura` |
| `MAX_VIDEO_SIZE_MB` | Rechaza archivos mayores a X MB | `500` |
| `RATE_LIMIT_PER_MIN` | Máximo de peticiones por IP por minuto | `10` |
| `ALLOWED_HOSTS` | Dominios permitidos (separados por coma) | `.tudominio.es` |

Todas las configuraciones se cargan mediante `python-dotenv`; ningún dato sensible está hardcodeado en el código fuente. En producción, rota tu `SECRET_KEY` periódicamente.

## 📄 Licencia

Licencia MIT — consulta el archivo [LICENSE](./LICENSE) para el texto completo.  
Puedes usar, modificar y distribuir este software libremente, siempre que conserves el aviso de copyright original.

## 📬 Contacto y soporte

- Reportes de bugs y sugerencias de funcionalidades: usa la pestaña Issues de GitHub
- Preguntas generales: support@twittervideodownloaderx.com
- Vulnerabilidades de seguridad: por favor, notifícalas por email antes de cualquier divulgación pública

Intento responder a las issues en unos pocos días. Si ha pasado más tiempo y no has recibido respuesta, no dudes en volver a escribir — a veces las cosas se escapan.

---

*Este proyecto no está afiliado, respaldado ni relacionado con Flickr / SmugMug, Inc. Todas las marcas registradas, logotipos y derechos de contenido pertenecen a sus respectivos propietarios.*

*Última actualización: mayo  | Versión 1.2.0*

*Demo en vivo: https://twittervideodownloaderx.com/flickr_downloader_sp*

*Escrito por una persona, para personas. Ninguna inteligencia artificial participó en la redacción de este README ni del código del proyecto.*