# Ronquidos

App para iPhone que escucha durante la noche, reconoce los ronquidos, los cuenta minuto a minuto y graba un trozo de cada episodio. En la mañana muestra cuánto se roncó, un gráfico por hora, las grabaciones y el historial de todas las noches.

Todo queda guardado solo en el iPhone (no hay servidor ni cuentas).

## Cómo se usa

1. Al acostarte, abre la app y toca **Empezar la noche**. La primera vez pide permiso para el micrófono: elige **Permitir**.
2. Deja el iPhone **enchufado**, cerca de la cama (a 1 o 2 metros) y **con la app abierta**. La pantalla queda negra, con la hora en azul tenue; se atenúa sola y no se apaga.
3. En la mañana, **mantén presionado** el botón de abajo para terminar (o deja que termine sola a la hora que elijas en Ajustes).

Si la app se cierra durante la noche (por ejemplo, por una llamada o porque se salió de ella), la noche queda guardada hasta ese momento y se avisa al volver a abrirla.

## Qué muestra

- **Anoche**: minutos roncando, qué parte de la noche fue, número de ronquidos, episodios y el episodio más largo.
- **Por hora**: gráfico de barras con los minutos roncando (o el número de ronquidos) en cada hora. Al tocar una barra aparece el detalle.
- **Grabaciones**: un trozo de hasta 30 s del comienzo de cada episodio (máximo 20 por noche). Se pueden escuchar, compartir o eliminar.
- **Historial**: gráfico de las últimas 14, 30 o 90 noches, promedio, noches sin roncar y la noche más alta. Cada noche se abre con su propio gráfico y sus grabaciones.

## Cómo reconoce un ronquido

Mide el sonido en la banda grave (80 a 800 Hz), donde está el ronquido, y lo compara con el ruido de fondo de la pieza, que se recalcula solo durante la noche.

- Un sonido es **candidato** si sobresale del fondo, dura entre 0,25 y 3,5 segundos y la mayor parte de su energía es grave.
- Tres candidatos seguidos con el **ritmo de la respiración** (cada 1,5 a 12 segundos y regulares) confirman un **episodio**. Mientras dura, cada nuevo candidato cuenta como ronquido.
- El episodio termina tras 25 segundos sin ronquidos.
- La voz, la respiración, un ventilador, la lluvia, golpes y toses no cuentan: no son graves, no duran lo que dura un ronquido o no se repiten con ese ritmo.

La **sensibilidad** (Ajustes) cambia cuánto debe sobresalir del fondo y qué tan grave debe ser. **Probar la detección** muestra en vivo cómo clasifica cada sonido.

## Ajustes

- Sensibilidad: baja, media (recomendada) o alta.
- Grabar los ronquidos: sí o no. Conservar los audios de las últimas 7, 14 o 30 noches (las cifras se guardan siempre).
- Empezar a escuchar de inmediato, en 15 o en 30 minutos.
- Terminar sola a una hora fija.
- Borrar todo.

## Instalar en el iPhone

1. Crea el repositorio `pablocarvallo/ronquidos` en GitHub y sube todos los archivos de esta carpeta.
2. En el repositorio: **Settings → Pages → Build and deployment**: «Deploy from a branch», rama `main`, carpeta `/ (root)` → **Save**.
3. Después de un minuto, abre `https://pablocarvallo.github.io/ronquidos/` en **Safari** en el iPhone.
4. Toca **Compartir → Agregar a inicio**.

## Detalles técnicos

- Un solo `index.html` con el sistema de diseño común de las apps domésticas (marcas `DS:TOKENS` y `DS:LAYER`).
- El motor de detección está entre `/*MOTOR:START*/ … /*MOTOR:END*/`.
- Audio: Web Audio con AudioWorklet (o ScriptProcessor en iPhones antiguos). Las grabaciones se guardan como WAV mono de 16 bits a 8 kHz (unos 16 KB por segundo).
- Datos: IndexedDB `ronquidos`, con las noches (`nights`) y los audios (`audio`) por separado. Ajustes en `localStorage`.
- Pantalla encendida: Screen Wake Lock; en iPhones con iOS anterior a 16.4 (o como app de inicio antes de iOS 18.4) además reproduce `silencio.mp4`, un video negro y mudo de 2 s en bucle.
- `sw.js` deja la app disponible sin conexión.
