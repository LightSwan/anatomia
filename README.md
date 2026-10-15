# Anatomía 3D

Visor anatómico 3D interactivo en una sola página web (HTML + three.js): capas de huesos, músculos, articulaciones, sistema circulatorio, nervioso, órganos y linfático; modo Simple y Avanzado; slider de profundidad para ir quitando capas desde el exterior; buscador, lista de músculos visibles y ficha de cada estructura.

Basado en el modelo abierto **Z-Anatomy** (anatomía de un varón adulto). Lee `LICENCIAS.md` antes de publicarlo.

## Estructura de la carpeta

```
anatomia3d/
├── index.html              página y lógica 3D
├── .nojekyll               evita el procesado de Jekyll en GitHub Pages
├── datos/
│   ├── estructuras.json    2.913 estructuras: id, nombre ES, latín, EN, lado, capa, modo, grupo, nivel de profundidad, importancia (músculos y huesos), zona del cuerpo
│   ├── fichas.json         fichas de todas las capas salvo el sistema nervioso (253 grupos)
│   └── lista-musculos.csv  lista de músculos por importancia
├── modelos/                21 archivos .glb comprimidos: 14 de calidad completa (cada capa en parte -s "simple" y -x "extra") y 7 «-lp» de low poly (una por capa)
├── README.md
└── LICENCIAS.md
```

Peso total: unos 36 MB (31 MB de calidad completa y 5,7 MB de low poly). El archivo más grande es `cardiovascular-x.glb` (6,5 MB). El selector **Calidad** de la página alterna entre «Completa» y «Low poly» (unos 1,8 millones de triángulos en total), pensado para móviles lentos; al cambiar, se descartan los modelos de la otra calidad para liberar memoria.

## Publicarlo con GitHub Pages

1. Sube la carpeta `anatomia3d` a tu repositorio, por ejemplo a `lightswan.github.io/anatomia3d/`. Respeta las subcarpetas `datos/` y `modelos/`.
2. Si es un repositorio nuevo: Settings → Pages → Source: «Deploy from a branch» → rama `main`, carpeta `/ (root)`.
3. Abre `https://TU-USUARIO.github.io/anatomia3d/`.

Importante: **no funciona abriendo `index.html` con doble clic**, porque el navegador bloquea la carga de los archivos. Para probarlo en local:

```
cd anatomia3d
python3 -m http.server 8000
# abre http://localhost:8000
```

### Subirlo desde el móvil (sin ordenador)

La web de GitHub no deja subir carpetas desde el móvil. Truco: en el repositorio pulsa «Add file → Create new file», escribe `anatomia3d/modelos/.gitkeep` como nombre y confirma; así se crea la carpeta. Entra en ella y usa «Add file → Upload files» para subir los 14 `.glb`. Repite con `anatomia3d/datos/` (dos `.json`) y con la raíz `anatomia3d/` (`index.html`, `README.md`, `LICENCIAS.md`, `.nojekyll`).

## Interfaz y zonas del cuerpo

Todos los paneles entran y salen con animaciones suaves, y la cámara se desplaza con una transición al seleccionar una estructura o cambiar de zona.

La interfaz sigue el estilo de la Biblioteca de la Natación (paleta Costa Esmeralda, fuente Sora, fondo degradado animado y paneles de cristal), con tema oscuro por defecto y botón para cambiar a claro. Tiene tres usos principales:

- **Modelo 3D:** capas, modo Simple/Avanzado, calidad Completa/Low poly y un filtro de **zona del cuerpo** (cabeza, cuello, tórax, columna y espalda, abdomen, pelvis, hombro y brazo, antebrazo y mano, cadera y muslo, pierna y pie) que aísla las estructuras de esa zona y encuadra la cámara.
- **Estudio:** lista las estructuras agrupadas por zona en acordeones, con buscador y filtro por capa. Al tocar una, se abre en el modelo 3D.
- **Quiz:** el pop up permite elegir el modo, la profundidad y la zona. Está pensado para el móvil, con botones grandes, barra de progreso y respuestas con color (verde si aciertas, rojo si fallas, ámbar si pides la respuesta).

La barra superior fija solo tiene **Modelo, Estudio, Quiz y Ajustes**; el Estudio incluye el buscador (por nombre en español, inglés o latín). Las capas, el modo, la calidad, las zonas, la profundidad, la opacidad, el tema y la información están dentro del popup **Ajustes** (un pequeño indicador al pie de la pantalla lo abre también). La ficha de una estructura es un panel compacto que se puede plegar y que desplaza el modelo hacia arriba para no taparlo.

La zona de cada estructura (décimo y undécimo valor de cada fila de `estructuras.json`: importancia y zona, esta última como índice 0 a 9 en el orden anterior) sale de las colecciones del modelo original y, si faltan, de la posición de la estructura. Las estructuras largas (nervios, vasos) figuran en una sola zona. La fuente Sora se carga desde Google Fonts, por lo que la página contacta con ese servicio al abrirse.

## Fichas de los músculos

`datos/fichas.json` guarda las fichas (qué hace, uso diario y curiosidad). Cada **grupo** de músculos tiene una ficha (clave `G:...`) y cada estructura del modelo apunta a su grupo con `{"a":"G:bic"}`; así, las dos cabezas del bíceps comparten ficha. Ejemplo:

```json
{
  "G:bic": {"n":"Bíceps braquial","m":1,"fn":"Flexiona el codo...","uso":"Subir una bolsa...","cur":"«Bíceps» significa..."},
  "Long head of biceps brachii": {"a":"G:bic"},
  "Short head of biceps brachii": {"a":"G:bic"}
}
```

(`m`:1 indica que el grupo tiene varias partes y activa el texto «Parte de…». El campo opcional `les` guarda las lesiones o problemas frecuentes, y se muestra en la ficha y en la respuesta del quiz.) También se admite una ficha como texto simple: `"Nombre en inglés de la estructura": "texto"`.

`datos/fichas-estado.csv` lista todas las estructuras de todas las capas con su estado (con ficha, pendiente u oculta).

`datos/lista-musculos.csv` es la lista de las 344 estructuras musculares por nivel de importancia (1 Imprescindible, 2 Importante, 3 Secundario, 4 De detalle, 5 Estructura asociada: fascias, bolsas, vainas y tendones), con una columna que indica si ya tiene ficha. El nivel de cada estructura está también en `estructuras.json` (décimo valor de la fila).

## Modo quiz

El botón **🎯 Quiz** abre un pop up donde eliges de qué músculos quieres que te pregunte: el modo (Simple o Avanzado) y el paso del slider de profundidad. Solo se pregunta por los músculos con ficha que siguen visibles con esa configuración. Cada pregunta resalta un músculo en naranja sobre el modelo (el resto se ve atenuado) y tú escribes su nombre. La pista usa primero la función del músculo y luego su curiosidad con el nombre tapado, y al terminar cada pregunta se muestra la ficha completa. El botón «Ajustes» del quiz reabre el pop up.

Las preguntas salen de `datos/fichas.json`: cada grupo nuevo que añadas con su `fn` entra solo en el quiz. Los sinónimos aceptados (por ejemplo «gemelos» para el gastrocnemio) están en el objeto `ALT` de `index.html`.

## Cómo se ajusta

- **Modo por defecto y profundidad inicial:** en `index.html`, busca `let dv=7,adv=true`.
- **Colores de capas:** array `LY` en `index.html`.
- **Estructuras masculinas ocultas:** conjunto `MALE`. Las 25 siguen en el modelo, pero no se muestran.
- **Qué entra en el modo Simple:** campo «modo» (`s` o `x`) de cada fila de `estructuras.json`; las capas de huesos y articulaciones se muestran completas siempre.

## Limitaciones

Es anatomía masculina de un solo individuo; no hay aparato reproductor femenino; el nivel de profundidad es un cálculo automático. Detalle completo en el panel «ℹ Info» de la página y en `LICENCIAS.md`.
