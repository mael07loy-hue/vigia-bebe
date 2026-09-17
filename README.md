# Vigía

Una cámara temporal de celular a Mac mediante WebRTC: el vídeo se transmite cifrado entre ambos dispositivos y no se graba en la aplicación. También detecta cambios de imagen en el celular y avisa en la Mac.

## Publicarla y usarla

1. Sube la carpeta a un alojamiento **HTTPS** (por ejemplo, GitHub Pages, Netlify o Cloudflare Pages). No la abras como `file://` ni desde un sitio `http://`: iPhone y Android sólo permiten cámara en una página segura.
2. Abre la URL HTTPS desde el celular, selecciona **Usar este celular como cámara**, acepta los permisos y pulsa **Iniciar cámara**.
3. Copia el enlace privado que se genera y ábrelo en Safari o Chrome de la Mac. El enlace usa un identificador aleatorio nuevo cada vez que se inicia una cámara. Ese identificador vive en el fragmento (`#…`) de la URL, por lo que el alojamiento estático no lo recibe en la petición.

Para la mejor calidad práctica, el modo 1080p a 30 fps es el predeterminado. Deja el celular conectado, no bloquees la pantalla y usa una buena red Wi-Fi. El modo “Máxima compatible” intenta 4K, pero puede calentar el celular y consumir mucha batería.

## Privacidad y alcance

- El audio y video usan WebRTC (DTLS-SRTP), cifrado durante el transporte entre los navegadores. La página no guarda grabaciones ni envía fotogramas a un servidor de aplicación.
- El CDN de PeerJS y su servicio de señalización público se usan únicamente para que los dos dispositivos se encuentren. El enlace de vista contiene un secreto aleatorio; trátalo como una contraseña y no lo compartas.
- De forma predeterminada sólo se permite una Mac conectada a la vez.
- En la misma Wi-Fi normalmente se conecta de forma directa. Para una conexión fiable desde otra red o datos móviles, configura un servidor TURN propio en la sección avanzada del celular; los datos TURN quedan codificados en el fragmento del enlace y no se envían al alojamiento de esta página.
- Ninguna cámara web debe ser el único mecanismo de seguridad para un niño. Comprueba que el vídeo esté transmitiendo y que haya batería/corriente antes de salir de la habitación.

## Servirla en desarrollo

Para probar la interfaz local en la Mac puede usarse cualquier servidor estático, pero la cámara del celular requerirá HTTPS. El alojamiento público HTTPS es la ruta más simple para el uso entre celular y Mac.
