# Licencias, atribuciones y cambios

## Resumen

- Los **modelos 3D** (`modelos/`) y los **datos de nombres** (`datos/estructuras.json`) son una adaptación de **Z-Anatomy** y se comparten bajo **CC BY-SA 4.0**, con las excepciones de terceros indicadas abajo (licencia no comercial).
- Por esas excepciones, el conjunto debe usarse **solo con fines educativos y no comerciales**, o bien retirar las piezas afectadas antes de cualquier uso comercial.
- El código de `index.html` es propio de este proyecto: elige tú la licencia que quieras para él. No se ha incluido ninguna.

## Fuentes

- **Z-Anatomy**: Gauthier Kervyn (diseño, 3D y anatomía) y Marcin Zielinski (script de Blender). Modelo 3D, estructuras y nomenclatura. CC BY-SA 4.0. https://github.com/Z-Anatomy/Models-of-human-anatomy
- **BodyParts3D**: Database Center for Life Science (DBCLS), Kousaku Okubo. Modelo original en el que se basa Z-Anatomy. Licencia CC BY-SA 2.1 Japón, según la atribución de Z-Anatomy (la página de descripción de BodyParts3D la resume como «CC BY»: conviene confirmarlo en su licencia oficial). Mitsuhashi N. et al., «BodyParts3D: 3D structure database for anatomical concepts», Nucleic Acids Research 37, D782–D785 (2009). https://dbarchive.biosciencedbc.jp/en/bodyparts3d/download.html
- **Traducción al español** de las estructuras anatómicas: Carlos Torres Villar, incluida en Z-Anatomy (nomenclatura Terminologia Anatomica).
- **Materiales de terceros dentro de Z-Anatomy**:
  - «Cranial Nerves and Foramina», University of Dundee (CAHID), CC BY 4.0.
  - «Anatomy of the Inner Ear», University of Dundee School of Medicine, **CC BY-NC-SA 4.0** (no comercial). En esta página corresponde, por ejemplo, a la cóclea y el vestíbulo.
  - «Kidney», Lissie Cowley, **CC BY-NC 4.0** (no comercial). En esta página corresponde a los riñones.
- **Software**: three.js r128 (MIT); Blender 4.5 (GPL), usado para leer y convertir el modelo; meshoptimizer/gltfpack (MIT), usado para comprimirlo; fuente Sora (SIL Open Font License 1.1, Google Fonts).

## Cambios realizados sobre el modelo original

- Conversión a formato web (glTF con compresión meshopt) y separación por capas.
- Aplicación del nivel de suavizado de acabado final (el original tiene un nivel más bajo en la vista de trabajo).
- Conversión de las curvas (arterias, venas, nervios y conductos) a malla con su resolución original, sin simplificar.
- Corrección de caras invertidas en las estructuras espejadas (lado izquierdo/derecho).
- Versión «low poly» (archivos `-lp`): simplificación de las mallas originales (y tubos más gruesos para vasos y nervios) para móviles lentos. La versión completa no está simplificada.
- Ocultación de 25 estructuras genitales masculinas (testículos, epidídimo, conductos deferentes, glándulas seminales, conductos eyaculadores, próstata, pene, uretra y sus vasos).
- Cálculo automático de un «nivel de profundidad» para cada estructura (distancia al exterior del conjunto de huesos, músculos y órganos).
- No se incluyen la piel, las regiones corporales, las inserciones musculares ni las etiquetas del modelo original.

## Por qué no hay modelo de ambos sexos

Z-Anatomy y BodyParts3D representan a un varón adulto. No se ha encontrado ningún atlas anatómico femenino completo con licencia abierta; los completos son comerciales y los abiertos, parciales. No se ha inventado ni deformado anatomía para simular un cuerpo femenino. Falta el aparato reproductor femenino.

Consultados y no utilizados: BMFToolkit (Manish Sreenivasa y Daniel Gonzalez-Alvarado, Universidad de Heidelberg; huesos del tren inferior de una mujer; licencia zlib; https://github.com/manishsreenivasa/BMFToolkit), por cubrir solo el tren inferior; PelvisAtlas (I-STAR Lab, Johns Hopkins University; https://github.com/I-STAR/PelvisAtlas), por tener una licencia contradictoria y no garantizar que sea femenina; y un registro de Zenodo (21243923) que no se pudo verificar.

## Nota sobre fichas de texto

Si se añaden fichas generadas por IA en `datos/fichas.json`, son orientativas, pueden contener errores y no sustituyen a un libro de texto ni a un profesional sanitario. Uso educativo, no clínico.
