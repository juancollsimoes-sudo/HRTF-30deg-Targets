# 🎧 HRTF-30deg-Targets: Curvas Objetivo y HRTF a 30 Grados para Auriculares

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Standards: ITU-R BS.775](https://img.shields.io/badge/Acoustic_Standard-ITU--R_BS.775-green.svg)](https://www.itu.int/rec/R-REC-BS.775)
[![HATS: B&K 5128 & KEMAR](https://img.shields.io/badge/Simulators-B%26K_5128_%7C_KEMAR-orange.svg)](https://www.bksv.com)
[![Formats: CSV & TXT](https://img.shields.io/badge/Formats-CSV_%7C_TXT_(REW%2FAutoEq)-purple.svg)](#formatos-y-compatibilidad)

Repositorio de curvas objetivo (*target curves*) y funciones de transferencia relativas a la cabeza (**HRTF**) medidas, simuladas y calibradas a **30° de azimut** (el ángulo estándar de escucha estéreo en triángulo equilátero según la norma **ITU-R BS.775**).

Diseñado específicamente para audiófilos, ingenieros de mezcla y entusiastas del audio que buscan ecualizar auriculares (*over-ear* e *in-ear*) emulando la respuesta tonal natural de **altavoces de estudio en salas tratadas acústicamente**.

---

## 📑 Tabla de Contenidos

- [1. Fundamentos Acústicos y Psicoacústica](#1-fundamentos-acústicos-y-psicoacústica)
  - [¿Por qué HRTF a 30° en lugar de Campo Difuso (DF) o Campo Libre Frontal (0°)?](#por-qué-hrtf-a-30-en-lugar-de-campo-difuso-df-o-campo-libre-frontal-0)
  - [El Modelo In-Room de Chris Struck (2013)](#el-modelo-in-room-de-chris-struck-2013)
  - [La Compensación de Graves (*Bass Shelf*) y Ganancia de Oreja (*Ear Gain*)](#la-compensación-de-graves-bass-shelf-y-ganancia-de-oreja-ear-gain)
- [2. Estructura del Repositorio](#2-estructura-del-repositorio)
- [3. Catálogo Detallado de Curvas Objetivo](#3-catálogo-detallado-de-curvas-objetivo)
- [4. Comparativa Numérica de Frecuencias Clave](#4-comparativa-numérica-de-frecuencias-clave)
- [5. Guía de Uso e Integración](#5-guía-de-uso-e-integración)
  - [Room EQ Wizard (REW)](#room-eq-wizard-rew)
  - [Squiglink / CrinGraph / PublicGraphTool](#squiglink--cringraph--publicgraphtool)
  - [AutoEq (Python CLI)](#autoeq-python-cli)
  - [Equalizer APO / Peace GUI](#equalizer-apo--peace-gui)
- [6. Referencias Científicas y Bibliografía](#6-referencias-científicas-y-bibliografía)
- [7. Licencia](#7-licencia)

---

## 1. Fundamentos Acústicos y Psicoacústica

### ¿Por qué HRTF a 30° en lugar de Campo Difuso (DF) o Campo Libre Frontal (0°)?

* **Campo Difuso (*Diffuse Field - DF*):** Asume que el sonido incide con igual energía desde todas las direcciones esféricas simultáneamente (como una cámara reverberante sin tratar). Aunque fue el estándar histórico (IEC 60268-7), en auriculares genera un exceso notable de agudos y una carencia profunda de calidez, resultando en un sonido brillante, delgado y fatigante.
* **Campo Libre Frontal (0° FF):** Asume que el sonido viene directo de frente a 0°. Sin embargo, la música grabada en estéreo se mezcla y masteriza para altavoces colocados a **±30° respecto a la línea central del oyente** (triángulo equilátero ITU-R BS.775).
* **HRTF a 30°:** Refleja con precisión la difracción anatómica del torso, hombros, cabeza y pabellón auditivo (*pinna*) cuando escuchamos una pareja de monitores de estudio en la posición real de mezcla.

### El Modelo In-Room de Chris Struck (2013)

En una sala real, el tímpano no solo recibe la onda directa del altavoz ($k_{\text{FF}}$), sino también un campo sonoro reverberante e indirecto ($k_{\text{DF}}$) proveniente de las reflexiones en las paredes:

$$H_{\text{InRoom}}(f) = \sqrt{\frac{Q}{4\pi r^2} \cdot |H_{\text{FF}}(f)|^2 + \frac{25 \cdot T_{60}}{V} \cdot |H_{\text{DF}}(f)|^2}$$

Donde:
* $Q = 2$ (directividad típica de un altavoz monitor de 2 vías).
* $r = 2\text{ m}$ (distancia del oyente al altavoz).
* $T_{60} = 0.4\text{ s}$ (tiempo de reverberación de una sala crítica tratada).
* $V = 80\text{ m}^3$ (volumen estándar de un estudio de mezcla).

### La Compensación de Graves (*Bass Shelf*) y Ganancia de Oreja (*Ear Gain*)

1. **El déficit natural de la HRTF cruda:** A 1 kHz, la cabeza y la concha humana amplifican el sonido (+4.77 dB en el simulador B&K 5128). A 20 Hz, la onda es tan larga (17 m) que no genera difracción (0 dB). Al normalizar una curva a 1 kHz = 0 dB, los 20 Hz parten naturalmente de **-4.90 dB**.
2. **Preferencia psicoacústica:** En altavoces físicos sentimos el sub-grave con el cuerpo y el pecho; con auriculares esa estimulación táctil desaparece. Por ello, investigaciones de Floyd Toole y Sean Olive demostraron que los usuarios prefieren un estante de graves (*bass shelf*) entre +4 dB y +8 dB por encima del nivel de 1 kHz para percibir un balance tonal equivalente.
3. **Control del *Ear Gain* (2.5 kHz - 4 kHz):** Mientras que los maniquíes registran picos de resonancia del canal de hasta +12 a +14 dB, para ecualización de auriculares un pico tan agresivo produce fatiga auditiva (*shoutiness*). Por ello, las variantes calibradas suavizan el pico entre +7.2 dB y +9.3 dB.

---

## 2. Estructura del Repositorio

```text
Targets_HRTF_30deg/
├── 01_InRoom_Struck_30deg/         # Simulación de altavoces a ±30° en sala tratada
├── 02_FreeField_30deg_BK5128/      # Mediciones anecoicas puras en B&K Type 5128 HATS
├── 03_KEMAR_GRAS/                  # HRTF a 30° KEMAR / GRAS 45CA (IEC 60318-4)
├── 04_Shaw_1985_Standards/         # Datos históricos de referencia Shaw & Vaillancourt
├── 05_PEQdB_Diamond_Targets/       # Perfil PEQdB Diamond β (sub-grave limpio y variantes)
├── .gitignore                      # Filtros de exclusión de Git
├── LICENSE                         # Licencia MIT
└── README.md                       # Documentación técnica completa
```

---

## 3. Catálogo Detallado de Curvas Objetivo

### 📁 `01_InRoom_Struck_30deg/` (Recomendado para Ecualización General)
Simula altavoces de respuesta plana calibrados a ±30° en una sala de control acústico (modelo Chris Struck / Sean Olive):
* **`5128_Target_Harman2018_Official_5128`**: Target oficial Harman 2018 adaptado al B&K 5128 (Sean Olive & Todd Welti). Calca el nivel de sub-grave de Harman OE 2018 (+3.50 dB @ 20 Hz) con la ganancia de oreja corregida (+9.30 dB @ 3 kHz).
* **`5128_Target_30deg_InRoom_Harman2018`**: Filtro de preferencia Harman 2018 aplicado estrictamente sobre la HRTF a 30° en sala.
* **`5128_Target_30deg_InRoom_DiamondBass`**: Combina la HRTF de sala a 30° con el sub-grave ultra limpio de PEQdB Diamond y ear gain suavizado (+8.64 dB @ 3 kHz).
* **`5128_Target_30deg_InRoom_Struck_BassBoost`**: Curva Struck con estante de graves RBJ Low Shelf calibrado a nivel Harman (+3.49 dB @ 20 Hz) y medios/agudos originales intactos.
* **`5128_Target_30deg_InRoom_Tilted_BassBoost`**: Curva Struck con tilt de -1.0 dB/octava y realce de graves Harman (+4.54 dB @ 20 Hz). Gran calidez y musicalidad.
* **`5128_Target_30deg_InRoom_Struck_Tilted`**: Curva Struck con tilt de -1.0 dB/octava sin estante adicional (pivote en 1000 Hz).
* **`5128_Target_30deg_InRoom_Struck`**: Curva base plana de sala pura (sin filtros de preferencia).

### 📁 `02_FreeField_30deg_BK5128/` (Datos Anecoicos B&K 5128)
Mediciones de alta fidelidad tomadas con el simulador anatómico Brüel & Kjær Type 5128:
* **`5128_Target_30deg_FF_HRTF`**: Campo libre ipsilateral puro a 30° (0 dB @ 1 kHz).
* **`5128_Target_30deg_FF_HRTF_Tilted`**: Campo libre a 30° con tilt de -1.0 dB/octava.
* **`5128_Target_30deg_Stereo_Sum`**: Suma energética binaural (+30° y -30° simultáneos).
* **`5128_Target_30deg_Raw_Data.csv`**: Matriz completa con 121 frecuencias ISO (30°, 330°, 0°, DF y Struck en dB SPL absolutos).

### 📁 `03_KEMAR_GRAS/` (Estándar Acoplador 711 / IEC 60318-4)
* **`KEMAR_Target_30deg_FF_HRTF`**: HRTF calculada mediante FFT de la respuesta al impulso original de MIT Media Lab (Gardner & Martin, 1994) para maniquíes KEMAR y acopladores GRAS 45CA / IEC 60318-4.

### 📁 `04_Shaw_1985_Standards/`
* **`Shaw_1985_Target_30deg_FF_DTF`**: Función de transferencia direccional (DTF) tabulada en Shaw & Vaillancourt (1985, JASA), referencia fundamental en la historia de la psicoacústica.

### 📁 `05_PEQdB_Diamond_Targets/` (Perfil Sub-Grave Ultra Limpio)
Basado en el diseño de **PEQdB Diamond β** (Saurav Chala / Sharur). Se caracteriza por un sub-grave contundente que cae de forma rápida y limpia hacia los 200–300 Hz (-1.89 dB), eliminando cualquier turbidez (*mid-bass bleed*) en voces e instrumentos:
* **`PEQdB_Diamond_TamedEarGain`** *(Muy recomendada)*: Mantiene el grave 100% idéntico a Diamond (+7.96 dB @ 20 Hz, tuck en -1.89 dB @ 300 Hz), pero suaviza el *ear gain* a **+7.26 dB @ 3 kHz**, logrando un sonido sedoso y libre de estridencias.
* **`PEQdB_Diamond_WarmRelaxed`**: Variante cálida con bajo Diamond, ear gain contenido (+6.71 dB) y agudos de 6 kHz suavizados.
* **`PEQdB_Diamond_Beta_Original`**: Target original idéntico al perfil nativo de PEQdB.com.
* **`5128_Target_30deg_InRoom_DiamondBass`**: Adaptación sobre la HRTF a 30° en sala.

---

## 4. Comparativa Numérica de Frecuencias Clave

Valores normalizados a **1000 Hz = 0.00 dB**:

| Frecuencia | Harman OE 2018 | 5128 Harman Oficial | Struck BassBoost | PEQdB Diamond Orig. | **Diamond Tamed Ear Gain** | Struck Tilted + BB |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **20 Hz** | +3.52 dB | +3.50 dB | +3.49 dB | +7.96 dB | **+7.96 dB** | +4.54 dB |
| **50 Hz** | +3.34 dB | +3.37 dB | +2.99 dB | +6.80 dB | **+6.80 dB** | +3.44 dB |
| **100 Hz** | +1.06 dB | +0.79 dB | -0.46 dB | +2.93 dB | **+2.93 dB** | +0.37 dB |
| **200 Hz** | -1.96 dB | -2.33 dB | -4.02 dB | -1.14 dB | **-1.14 dB** | -2.08 dB |
| **300 Hz (Tuck)**| -2.20 dB | -2.15 dB | -3.85 dB | -1.89 dB | **-1.89 dB** | -2.10 dB |
| **500 Hz** | -0.79 dB | -1.01 dB | -2.47 dB | -1.52 dB | **-1.52 dB** | -1.48 dB |
| **1000 Hz** | **0.00 dB** | **0.00 dB** | **0.00 dB** | **0.00 dB** | **0.00 dB** | **0.00 dB** |
| **2000 Hz** | +5.51 dB | +5.04 dB | +5.87 dB | +5.26 dB | **+4.67 dB** | +4.87 dB |
| **3000 Hz** | +8.47 dB | +9.30 dB | +12.13 dB | +9.02 dB | **+7.26 dB** | +10.54 dB |
| **6000 Hz** | +5.06 dB | +6.51 dB | +9.47 dB | +7.62 dB | **+7.37 dB** | +6.89 dB |
| **10000 Hz** | -0.86 dB | +0.92 dB | +1.68 dB | +5.81 dB | **+5.81 dB** | -1.65 dB |

---

## 5. Guía de Uso e Integración

### Formatos Disponibles
* **Archivos `.csv`**: Encabezado `frequency,raw` en cuadrícula estándar de 695 frecuencias (20.00 Hz a 19955.54 Hz). Compatible con AutoEq y scripts de procesamiento por lotes.
* **Archivos `.txt`**: Formato tabulado de 2 columnas (`freq\tSPL`) sin cabecera. Compatible directamente con REW, CrinGraph, Squiglink y Equalizer APO.

---

### Room EQ Wizard (REW)
1. Abre REW y accede a la ventana **EQ** de la medición de tus auriculares.
2. En el panel derecho (*Target Settings*), selecciona:
   * **Target Type**: `Full Range Speaker` o `None`.
3. Haz clic en **Target Curve** -> **Browse** y selecciona cualquier archivo `.txt` de este repositorio (por ejemplo, `PEQdB_Diamond_TamedEarGain.txt` o `5128_Target_Harman2018_Official_5128.txt`).
4. Haz clic en **Match Target** para generar los filtros paramétricos automáticos.

---

### Squiglink / CrinGraph / PublicGraphTool
1. Accede a tu interfaz web de Squiglink o CrinGraph.
2. Arrastra y suelta directamente el archivo `.csv` o `.txt` sobre la ventana del navegador (o cópialo a la carpeta `data/targets/` si alojas tu propia instancia).
3. Selecciona la curva como **Target** en la leyenda inferior para comparar o usar el motor de auto-ecualización web.

---

### AutoEq (Python CLI)
Para ecualizar automáticamente una medición contra uno de estos targets:
```bash
python -m autoeq \
  --input_dir="measurements/my_headphones" \
  --output_dir="eq_results" \
  --target="Targets_HRTF_30deg/05_PEQdB_Diamond_Targets/PEQdB_Diamond_TamedEarGain.csv" \
  --compensation="Targets_HRTF_30deg/05_PEQdB_Diamond_Targets/PEQdB_Diamond_TamedEarGain.csv" \
  --equalize \
  --parametric_eq
```

---

### Equalizer APO / Peace GUI
1. Abre **Peace Equalizer**.
2. Haz clic en el botón de **Import** (o arrastra los filtros generados por AutoEq o REW).
3. Los archivos de este repositorio actúan como la referencia contra la cual se calculan las ganancias de cada banda paramétrica.

---

## 6. Referencias Científicas y Bibliografía

1. **ITU-R BS.775-4 (2022):** *Multichannel stereophonic sound system with and without accompanying picture*. International Telecommunication Union.
2. **Struck, C. J. (2013):** *Simulated In-Room Head-Related Transfer Functions*. CJS Labs Technical Papers & AES 135th Convention.
3. **Olive, S., Welti, T., & McMullin, E. (2013):** *Listener Preferences for In-Room Loudspeaker and Headphone Target Responses*. AES 134th Convention, Paper 8867.
4. **Olive, S., Welti, T., & Khonsaripour, O. (2018):** *The Perception and Measurement of Headphone Sound Quality: What Do Listeners Prefer?*. AES 145th Convention.
5. **Shaw, E. A. G., & Vaillancourt, M. M. (1985):** *Transformation of sound pressure level from the free field to the eardrum presented in numerical form*. Journal of the Acoustical Society of America (JASA), 78(3), 1120-1123.
6. **Gardner, B., & Martin, K. (1994):** *HRTF Measurements of a KEMAR Dummy-Head Microphone*. MIT Media Lab Perceptual Computing - Technical Report #280.
7. **Toole, F. E. (2018):** *Sound Reproduction: The Acoustics and Psychoacoustics of Loudspeakers and Rooms* (3rd Edition). Routledge / Focal Press.

---

## 7. Licencia

Este proyecto está bajo la [Licencia MIT](LICENSE) - siéntete libre de utilizar, compartir, integrar y modificar estos datos tanto en proyectos personales como comerciales citando la fuente.
