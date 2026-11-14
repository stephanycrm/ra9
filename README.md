# XR CIRCUIT LAB EDU

Laboratorio educativo para primaria en Colombia: componentes eléctricos en 3D, actividad sobre LED, cámara con superposición de modelos, seguimiento de manos con MediaPipe y RV inmersiva cuando WebXR lo permite.

## Estructura

- `index.html`: interfaz y contenido pedagógico.
- `styles.css`: estilos adaptables.
- `script.js`: modelos geométricos, visor Three.js, cámara, MediaPipe, RV y actividades.

## Ejecutar

Necesita conexión a internet para cargar Three.js y MediaPipe desde CDN. Sirva los archivos desde un servidor web (no abra `index.html` mediante `file://`, porque los módulos ES pueden bloquearse):

```bash
python -m http.server 8000
```

Abra `http://localhost:8000` en el computador. Para cámara en teléfonos, publique mediante HTTPS; un servidor HTTP en la red local no es contexto seguro.

## Publicar en GitHub Pages

Cree un repositorio, suba los tres archivos y este README a la raíz. En Settings > Pages seleccione Deploy from a branch, `main` y `/ (root)`. Espere la publicación HTTPS. Abra la URL desde el teléfono y autorice la cámara solo al pulsar los botones correspondientes.

## Probar

1. Abra el sitio en computadora y celular, seleccione las diez piezas, rote y haga zoom.
2. Pulse «RA con cámara» y autorice el acceso; verá el modelo superpuesto sobre el video. **No hay anclaje a superficies ni seguimiento espacial.**
3. Pulse «Activar manos»: índice en movimiento para rotar, pinza para acercar, palma abierta para pausar. La precisión depende de luz, cámara, potencia del equipo y posición de la mano.
4. Pulse «Entrar en RV» en un dispositivo con WebXR `immersive-vr` compatible y HTTPS. En otros equipos seguirá disponible el modo 3D.
5. Complete la misión del LED; genere y descargue la guía docente.
6. Pruebe con teclado, pantalla pequeña y permiso de cámara denegado.

## Modelos

Los diez modelos iniciales son **representaciones geométricas educativas originales** creadas en `build(type)` dentro de `script.js`, sin descargas de modelos de terceros. No representan planos eléctricos de montaje. Para sustituir un modelo inicial por un GLB propio, puede modificar `choose()` y `loadGLB()` para cargar una ruta local (por ejemplo `models/mi-modelo.glb`), conservando el archivo en el repositorio. El botón «Importar modelo 3D» acepta GLB autocontenido de hasta 50 MB y solo conserva los modelos durante la sesión; los Object URLs se liberan al cerrar la página. La importación de `.gltf` con recursos separados no está implementada. Para persistencia entre visitas, agregue los archivos GLB al repositorio y regístrelos en el código.

Use solo modelos para los que tenga permiso; documente autor, título, licencia y enlace de origen en el sitio. No se incluyen modelos USDZ ni enlaces externos no verificados.

## Bibliotecas y privacidad

- Three.js `0.170.0` (ES Modules) y GLTFLoader desde jsDelivr.
- MediaPipe Tasks Vision `0.10.22` desde jsDelivr; modelo `hand_landmarker.task` desde el almacenamiento público de MediaPipe.
- WebXR a través de Three.js; depende de navegador y dispositivo.
- Web Speech API integrada en el navegador, si está disponible.

El procesamiento de cámara y gestos ocurre en el navegador; el código no envía imágenes a un servidor propio. Las bibliotecas y el archivo de modelo de MediaPipe sí se descargan desde proveedores externos. La cámara no se solicita al cargar la página.

## Límites y seguridad

El modo «RA con cámara» es composición visual de video + 3D, **no RA espacial** con detección de superficies. El modo RV necesita un dispositivo compatible y HTTPS. Los gestos son controles heurísticos de una sola mano; no permiten agarrar piezas individuales ni conectarlas físicamente. Los modelos son demostrativos. La misión del LED es conceptual, no un esquema eléctrico listo para construir.

No conectar nada a la red eléctrica doméstica. Cualquier experimento físico debe realizarse con materiales de bajo voltaje adecuados y supervisión adulta.

## Constructor interactivo de circuito (versión mejorada)

La sección **Construye tu circuito LED** permite seleccionar piezas y colocarlas en tres casillas mediante clic, teclado o arrastrar y soltar. La actividad comprueba la pila, la resistencia y el LED, muestra progreso y exige cerrar el interruptor para iluminar el LED virtual. Es una simulación conceptual, no un simulador físico de corriente o conexiones reales.

## Control gestual MediaPipe (integrado)

1. Publica la web con HTTPS (o abre en localhost con servidor local).
2. Pulsa **Activar manos** y acepta el permiso de cámara.
3. Mueve el dedo índice para girar el modelo; haz una pinza para acercarlo; abre la palma para pausar; muestra índice y corazón en V para cambiar al siguiente componente.
4. Pulsa **Desactivar manos** o **Detener cámara** para finalizar.

MediaPipe detecta puntos de la mano localmente mediante Tasks Vision; la biblioteca y el modelo de inferencia se descargan desde CDN y Google Storage. Los gestos pueden requerir buena iluminación y ajustes según el equipo. No se garantiza compatibilidad universal ni anclaje espacial en RA.
