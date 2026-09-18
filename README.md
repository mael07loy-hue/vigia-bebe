# Vigía

Una cámara temporal de celular a Mac mediante WebRTC: el vídeo y el audio se transmiten cifrados entre ambos dispositivos y no se graban en la aplicación. Detecta movimiento en el celular, hace sonar una alarma en la Mac y permite hablarle al bebé desde la Mac.

## Publicarla y usarla

1. Sube la carpeta a un alojamiento **HTTPS** (por ejemplo, GitHub Pages, Netlify o Cloudflare Pages). No la abras como `file://` ni desde un sitio `http://`: iPhone y Android sólo permiten cámara y micrófono en una página segura.
2. Abre la URL HTTPS desde el celular, selecciona **Usar este celular como cámara**, acepta los permisos y pulsa **Iniciar cámara**.
3. Copia el enlace privado que se genera y ábrelo en Safari o Chrome de la Mac. El enlace usa un identificador aleatorio nuevo cada vez que se inicia una cámara. Ese identificador vive en el fragmento (`#…`) de la URL, por lo que el alojamiento estático no lo recibe en la petición.

Para la mejor calidad práctica, el modo 1080p a 30 fps es el predeterminado. Deja el celular conectado, no bloquees la pantalla y usa una buena red Wi-Fi. El modo “Máxima compatible” intenta 4K, pero puede calentar el celular y consumir mucha batería. Mientras transmite, la página pide a iOS que no apague la pantalla (Wake Lock).

## Si en la Mac se escucha muy bajo

El sonido se amplifica en dos puntos, y ése es el orden en que conviene ajustarlo:

1. **En el celular · “Sensibilidad del micrófono” (100–600%).** Sube el audio *antes* de enviarlo, con filtro de graves, compresor y limitador. Es lo que más ayuda, porque el micrófono del iPhone capta muy bajo el sonido de una habitación en silencio. La barra de nivel del celular confirma que hay señal: si no se mueve, el problema es el permiso o el micrófono, no la Mac.
2. **En la Mac · “Volumen” (0–500%).** Amplifica lo que llega, con “Realzar sonidos suaves” activado (compresor + limitador) para oír la respiración sin que un llanto sature. La barra se mueve aunque el sonido esté silenciado: sirve para distinguir “no llega audio” de “llega y está bajo”.

Pulsa **Activar sonido** cuando entre el vídeo: el navegador exige ese primer clic. El audio va por Opus a 128 kbps con DTX desactivado, para que no recorte los sonidos suaves. El amplificador no puede vencer el límite físico de las bocinas de la Mac; sube también el volumen del sistema y baja el 500% si escuchas distorsión.

## Hablar al bebé

Con el vídeo en directo, pulsa **Hablar** en la Mac (o mantén la **barra espaciadora** para hablar sólo mientras la sostienes). La primera vez el navegador pedirá permiso del micrófono. Tu voz sale por la bocina del celular; mientras hablas se silencian los dos micrófonos para evitar acoples. Sube el volumen del iPhone: cuando hay una captura de micrófono activa, iOS puede enrutar la voz al auricular en lugar de la bocina y se oye bajo.

## Alarma automática

- **Avisarme automáticamente** (activado): al detectar movimiento suena la alarma, aparece un aviso rojo y parpadea el título de la pestaña, aunque estés en otra aplicación.
- **Probar alarma**: escucha cómo suena. Pulsarlo una vez también desbloquea el sonido del navegador, requisito para que la alarma pueda sonar sola más tarde.
- **Repetir hasta que la silencie** (activado): la alarma insiste hasta 90 segundos o hasta que pulses **Silenciar**.
- **Notificación de macOS**: opcional, pide permiso al activarla.
- Si la cámara deja de transmitir, la Mac también avisa: para un vigilabebés, quedarse en silencio sin saberlo es el peor fallo.
- Ajusta la sensibilidad en el celular si hay falsos avisos (sombras, cortinas) o si no detecta. Entre avisos hay 6 segundos de margen.

## Privacidad y alcance

- El audio y vídeo usan WebRTC (DTLS-SRTP), cifrado durante el transporte entre los navegadores. La página no guarda grabaciones ni envía fotogramas a un servidor de aplicación.
- El CDN de PeerJS y su servicio de señalización público se usan únicamente para que los dos dispositivos se encuentren. El enlace de vista contiene un secreto aleatorio; trátalo como una contraseña y no lo compartas.
- De forma predeterminada sólo se permite una Mac conectada a la vez.
- En la misma Wi-Fi normalmente se conecta de forma directa. Para una conexión fiable desde otra red o datos móviles, configura un servidor TURN propio en la sección avanzada del celular; los datos TURN quedan codificados en el fragmento del enlace y no se envían al alojamiento de esta página.
- Ninguna cámara web debe ser el único mecanismo de seguridad para un niño. Comprueba que el vídeo esté transmitiendo y que haya batería/corriente antes de salir de la habitación.

## Servirla en desarrollo

Para probar la interfaz local en la Mac puede usarse cualquier servidor estático (`python3 -m http.server`), y `http://localhost` cuenta como contexto seguro para la cámara de la propia Mac. El celular sí requiere HTTPS: el alojamiento público es la ruta más simple para el uso entre celular y Mac.
