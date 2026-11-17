# Neon Rush 3D v9.3 — configuración centralizada

## Archivos
- `index.html`: un único juego público.
- `admin.html`: panel independiente de administración.
- `admin.js`: controles, borradores, publicación e historial.
- `firebase-config.js`: datos públicos de conexión Firebase.
- `firestore.rules`: permisos de acceso a los documentos.
- `heroine.glb`: personaje 3D.

## Puesta en marcha (obligatoria para sincronización)
1. En Firebase Console crea un proyecto. Registra una aplicación **Web** y copia `apiKey`, `authDomain`, `projectId` y `appId` a `firebase-config.js`. Estos datos son identificadores públicos, NO una contraseña.
2. Activa **Authentication → Sign-in method → Email/Password**. En **Users**, crea manualmente tu cuenta de administrador.
3. Activa **Cloud Firestore**. En **Rules**, pega el contenido de `firestore.rules` y publica las reglas.
4. En Firestore, crea la colección `admins`, con un documento cuyo ID sea exactamente el **UID** de tu usuario de Firebase Authentication y un campo booleano `enabled: true`. Esto se hace desde Firebase Console, no desde el juego.
5. Sube `index.html`, `admin.html`, `admin.js`, `firebase-config.js` y `heroine.glb` a tu repositorio de GitHub Pages. Mantén `firestore.rules` como referencia local o en el repositorio (no contiene secretos).
6. Abre `https://cvvnpm9h5m-afk.github.io/neon-rush/admin.html`, inicia sesión y configura el Boss, la carrera y los tiempos. Pulsa **GUARDAR BORRADOR** o **PUBLICAR CAMBIOS**.
7. El juego público consulta el documento `gameConfig/published` al cargar. Los cambios publicados aparecen cuando los jugadores recargan la página o inician una nueva sesión.

## Seguridad
`admin.html` puede abrirse desde una URL pública, pero las reglas de Firestore impiden a visitantes leer borradores o publicar. La autenticación real está en Firebase. NO guardes contraseñas ni claves privadas en los archivos de GitHub.

## Alcance y limitaciones
- La conexión **no está activa** hasta que completes los pasos anteriores.
- Vista previa privada en `admin.html`: abre el juego en un marco y aplica el borrador sin publicarlo.
- Los elementos de interfaz de carrera y Boss se pueden editar en X/Y, ancho, fuente y capa. Los objetos 3D del mundo (edificios, muros, enemigo, personaje) todavía no tienen editor de transformaciones.
- La vista previa permite arrastrar objetos táctiles, pero los controles de desplazamiento numéricos son la opción más fiable en iPhone.
- La versión 9.3 no incluye todavía sincronización en vivo durante partidas activas, ni infraestructura PvP.
- Los valores de HP y de ítems se aplican al iniciar la siguiente partida.
- El panel tiene hasta tres deshacer/rehacer por sesión y guarda hasta 10 versiones visibles en historial (el resto sigue almacenado en Firestore).
- La publicación y el número de versión pueden entrar en conflicto si dos administradores publican simultáneamente; pensado inicialmente para un único administrador.
