# Chat Clásico

Chat con salas al estilo de los chats de los años 90 (mIRC, Windows 98). Se entra solo con un nombre o alias, sin cuentas ni contraseñas, y cualquiera puede crear su propia sala. Funciona en GitHub Pages porque no necesita un servidor propio: los mensajes pasan por un broker MQTT público mediante [MQTT.js](https://github.com/mqttjs/MQTT.js).

## Características

- **Lobby** de inicio con la lista de salas creadas y el número de personas en cada una, actualizada en vivo.
- Al tocar una sala, la app te pide tu nombre o alias y entras.
- Cualquiera crea una sala escribiendo un nombre; si ya existe, entra a ella en lugar de crear otra.
- Salas ocultas que no aparecen en la lista.
- Enlaces directos a una sala: `https://tu-usuario.github.io/tu-repo/#mi-sala`.
- Lista de usuarios por sala y avisos de entrada y salida.
- Comandos de chat.
- Diseño responsive para teléfono.
- Un solo archivo (`index.html`), sin compilación ni dependencias que instalar.

## Publicar en GitHub Pages

1. Crea un repositorio y sube `index.html` a la raíz.
2. Ve a **Settings → Pages**.
3. En **Source** elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`.
4. Guarda. En uno o dos minutos la app estará en `https://tu-usuario.github.io/tu-repo/`.

## Uso

1. En el lobby, toca una sala de la lista o escribe un nombre para crear una nueva.
2. Escribe tu alias cuando la app te lo pida.
3. Para cambiar de sala, usa la lista del chat o **← Volver al lobby**.
4. Para invitar a alguien, comparte el enlace con `#nombre-de-sala` al final: al abrirlo le pedirá su nombre y entrará a esa sala.

### Comandos

| Comando | Qué hace |
|---|---|
| `/nick alias` | Cambia tu alias |
| `/me acción` | Mensaje en tercera persona (`* Ana saluda`) |
| `/join sala` | Entra a otra sala |
| `/lobby` | Vuelve al lobby |
| `/ayuda` | Muestra los comandos |

Los nombres de sala se normalizan: minúsculas, sin tildes, y solo letras, números, `-` y `_` (máximo 30 caracteres).

## Cómo funciona

GitHub Pages solo sirve archivos estáticos, así que no hay backend propio. La app se conecta por WebSocket a un broker MQTT público (por defecto `broker.hivemq.com`, con `broker.emqx.io` como respaldo) y usa estos temas, todos bajo el prefijo `chatclasico-v1`:

- `…/m/<sala>`: mensajes de una sala. Solo se escucha la sala en la que estás.
- `…/p/<id>`: presencia de cada pestaña (alias, sala y si es oculta). Se publica como mensaje retenido y se renueva cada 25 segundos; el broker lo borra solo si la conexión se cae. Con esto se arman la lista de salas del lobby, el número de personas y la lista de usuarios.

Como los mensajes pasan por el broker, no importa que los celulares estén en redes distintas o con datos móviles. Todo el texto recibido se muestra con `textContent`, por lo que no se interpreta HTML.

## Limitaciones

- **No hay historial.** Los mensajes solo los ven quienes están conectados en ese momento.
- **No es privado.** Un broker público lo puede leer cualquiera que conozca el prefijo y los temas. No compartas información sensible. Las salas ocultas no se listan, pero su nombre viaja en los datos de presencia, así que no son seguridad real.
- **Las salas no tienen contraseña.** Quien conozca el nombre puede entrar.
- **Dependes de un servicio gratuito.** Si el broker público está caído, la app no conecta; pasa al respaldo automáticamente si el primero no responde. Para algo importante conviene tener un broker propio.
- Los alias no son únicos ni están verificados: dos personas pueden usar el mismo.

## Personalización

- **Tu propia red de salas:** cambia `NS` en `index.html` (`chatclasico-v1`). Solo se ven entre sí quienes usan el mismo valor.
- **Tu propio broker:** cambia la lista `BROKERS` por la dirección `wss://` de tu broker MQTT.
- **Apariencia:** los colores y tipografías están en las variables CSS al inicio del archivo (`:root`).

## ¿Necesitas historial o salas persistentes?

Hay que añadir una base de datos. Firebase Realtime Database o Supabase Realtime tienen plan gratuito y funcionan desde GitHub Pages, pero requieren crear un proyecto y configurar reglas de seguridad.
