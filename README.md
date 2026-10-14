# Actas CUSAFA

Aplicación web para transcribir reuniones de órganos colegiados con la voz de cada asistente identificada por su nombre, y para redactar el borrador del acta, nominal o anónima.

Desarrollada para el **Centro Universitario Sagrada Familia (Úbeda)**, aplicable a cualquier centro u organización.

## Cómo funciona

1. **Convocar.** Quien levanta acta crea la reunión: órgano, fecha, lugar y orden del día.
2. **Unirse.** Los asistentes escanean el QR (o escriben el código de 6 caracteres), indican su nombre, aceptan el consentimiento y activan el micrófono.
3. **Transcribir.** Cada dispositivo convierte en texto la voz de su usuario y envía solo el texto a la secretaría. Las intervenciones se archivan por puntos del orden del día.
4. **Redactar.** Al terminar se descarga la transcripción en Word o se genera el borrador del acta con IA. Después, **Finalizar y borrar** lo elimina todo.

## Funciones

- Orden del día vivo: cada intervención queda asociada al punto en curso.
- Fusión de ecos: si dos micrófonos captan la misma frase, se conserva la del que la oyó más fuerte.
- Sala de espera: la secretaría admite a cada persona antes de que su voz cuente.
- Turno de palabra: los asistentes piden la palabra desde su dispositivo.
- **Votación en directo** desde cada dispositivo, pública o secreta. Cada asistente vota en su pantalla (a favor, en contra o abstención) y puede cambiar su voto hasta el cierre; la secretaría ve la participación y, al cerrar, el recuento queda registrado en la transcripción. En la votación secreta solo se conserva el recuento.
- Marcas de **acuerdo** y **tarea**, registro manual de votaciones y **notas de secretaría**.
- Tramos **fuera de acta**: apagan todos los micrófonos.
- Corrección del texto y reasignación de quién dijo cada frase.
- Modo de **un solo dispositivo**, indicando a mano quién tiene la palabra.
- Exportación a **Word (.docx)** con nombres o anonimizada.
- **Acta con IA** en tres modos: nominal, sin nombres o solo acuerdos y tareas.
- Proyección del QR a pantalla completa y estadísticas orientativas de participación.
- Demostración con datos de ejemplo.

## Privacidad

**Lo que la aplicación garantiza**

- No tiene servidor ni base de datos. La transcripción existe solo en la memoria del navegador de la secretaría.
- No usa cookies, `localStorage`, `IndexedDB` ni analítica.
- No graba ni transmite audio: cada dispositivo envía únicamente texto.
- El texto viaja de navegador a navegador por un canal cifrado (WebRTC). El servicio de enlace solo pone en contacto a los dispositivos; el contenido no pasa por él.
- Cada asistente da su consentimiento antes de activar el micrófono.
- **Finalizar y borrar** elimina el contenido también en los dispositivos de los asistentes.

**Lo que depende de terceros**

- **Reconocimiento de voz.** Lo realiza el navegador. Chrome puede hacerlo dentro del dispositivo. Si no puede, envía el audio al servicio de su fabricante (Google en Chrome, Apple en Safari). Cada pantalla indica cuál se usa, y el **modo estricto** solo permite transcribir a los dispositivos que procesan la voz en local.
- **Redacción con IA (opcional).** El texto se envía a la API de Anthropic. En el acta sin nombres, estos se sustituyen en el dispositivo antes del envío.
- **Servicio de enlace.** Por defecto se usa el servidor público de [PeerJS](https://peerjs.com), que ve las direcciones IP de los dispositivos, pero no el contenido. Para uso institucional se recomienda un servidor propio (ver más abajo).

Antes de usarla con datos reales, conviene validarla con la persona responsable de protección de datos del centro.

## Publicar con GitHub Pages

El micrófono solo funciona en direcciones `https`, y GitHub Pages las ofrece gratis.

1. Sube estos archivos al repositorio.
2. En el repositorio, entra en **Settings → Pages**.
3. En **Source**, elige **Deploy from a branch**, la rama `main` y la carpeta `/ (root)`. Pulsa **Save**.
4. En uno o dos minutos la app estará en `https://agarcia-poz.github.io/generadordeactas/`.

El QR de cada reunión apunta automáticamente a esa dirección.

## Requisitos

- **Secretaría:** ordenador o tableta con Chrome o Edge actualizado.
- **Asistentes:** cualquier móvil o portátil con Chrome, Edge o Safari actualizado.
- Todos los dispositivos con conexión a internet. En algunas redes corporativas WebRTC puede estar restringido; si ocurre, usa el modo de un solo dispositivo o pide a informática que lo permita.

## Redacción con IA

En la pestaña **Acta con IA** se introduce una clave de API de Anthropic. La clave se guarda solo en la memoria de la pestaña y se pierde al cerrarla. Se recomienda una clave institucional.

Sin clave, el botón **Copiar texto con instrucciones para otra IA** (pestaña **Exportar**) prepara la transcripción y las instrucciones de redacción para pegarlas en cualquier asistente.

## Servidor de enlace propio (recomendado para producción)

Para no depender del servidor público de PeerJS, informática puede desplegar [PeerServer](https://github.com/peers/peerjs-server) y cambiar en `index.html` las dos llamadas `new Peer(...)` para añadir `{host: "servidor.del.centro", port: 443, secure: true, path: "/"}`.

## Estructura

```
index.html   La aplicación completa (HTML, CSS y JavaScript en un único archivo)
.nojekyll    Indica a GitHub Pages que sirva los archivos sin procesarlos
README.md    Este documento
```

Librerías cargadas desde CDN: `qrcode-generator` 1.4.4, `peerjs` 1.5.4 y `docx` 8.5.0.
