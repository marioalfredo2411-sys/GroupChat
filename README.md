# Chat Clásico

Chat con salas al estilo de los chats de los años 90 (mIRC, Windows 98). Se entra solo con un nombre o alias, sin cuentas ni contraseñas, y cualquiera puede crear su propia sala. Funciona en GitHub Pages porque no necesita servidor: los participantes se conectan directamente entre sí con WebRTC, usando [Trystero](https://github.com/dmotz/trystero).

## Características

- **Lobby** de inicio con la lista de salas creadas y el número de personas en cada una, actualizada en vivo.
- Al tocar una sala, la app te pide tu nombre o alias y entras. Sin cuentas ni contraseñas.
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

GitHub Pages solo sirve archivos estáticos, así que no hay backend. Trystero usa servidores públicos de señalización solo para que los navegadores se encuentren; después los mensajes viajan directamente de un navegador a otro.

- Cada sala es una sala de Trystero (`r:nombre`).
- Una sala especial, `lobby`, se usa para que todos anuncien en qué sala están y así armar la lista de salas activas. Las salas ocultas no se anuncian.
- Todo el texto recibido se muestra con `textContent`, por lo que no se interpreta HTML.

## Limitaciones

- **No hay historial.** Los mensajes solo los ven quienes están conectados en ese momento. Si la sala queda vacía, desaparece.
- **Las salas no tienen contraseña.** Quien conozca el nombre puede entrar. Una sala oculta con un nombre difícil de adivinar funciona como sala privada informal, pero no es seguridad real.
- **Algunas redes bloquean WebRTC**, como ciertos Wi-Fi corporativos o escolares. Ahí las personas pueden no verse entre sí.
- Los alias no son únicos ni están verificados: dos personas pueden usar el mismo.
- Las personas que estén en la misma sala desde distintas redes dependen de que los servidores públicos de señalización estén disponibles.

## Personalización

- **Tu propia red de salas:** cambia `appId` en `index.html` (`chat-clasico-ghpages-v1`). Solo se ven entre sí quienes usan el mismo valor.
- **Apariencia:** los colores y tipografías están en las variables CSS al inicio del archivo (`:root`).

## ¿Necesitas historial o salas persistentes?

Hay que añadir un backend. Firebase Realtime Database o Supabase Realtime tienen plan gratuito y funcionan desde GitHub Pages, pero requieren crear un proyecto y configurar reglas de seguridad.
