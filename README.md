# Proyecto: Analisis Billboard + integracion Spotify API

## 1. Introducción

Este proyecto integra datos de canciones, a partir de un dataset de **Billboard Hot 100** por el periodo 1958-2021, con metadatos musicales de **Spotify API** para analizar tendencias historicas de generos, duraciones, popularidad y diferencias entre eras tecnologicas (vinilo, cd, mp3 y streaming).

Link to dataset: https://www.kaggle.com/datasets/dhruvildave/billboard-the-hot-100-songs?resource=download

Pipeline completa en 4 fases, con sus relativos dataframes.

---

## 2. Pipeline del proyecto

### **Fase 1 – Kaggle Cleaning (Billboard)**

**Archivo:** `Fase 1 - Kaggle cleaning - Billboard The Hot 100 Songs.ipynb`

**Input:**

* `Kaggle - Billboard Hot 100 dataset.csv`

**Procesos principales:**

* Limpieza inicial
* Conversión de fechas y creación de columna `year`
* Check fila duplicadas
* Normalización de campos
* Creación columna combinada `song_artist` para busqueda API
* Generación del ranking **Top100 anual** (no usado en fases posteriores) -> guardado en `billboard_top100_annual.csv`

**Output:**

* `billboard_weekly_ready.csv` (usado en Fase 2)
* `billboard_top100_annual.csv`

---

### **Fase 2 – Spotify API Integration**

**Archivo:** `Fase 2 - Spotify API integration.ipynb`

**Input:**

* `billboard_weekly_ready.csv`

**Procesos principales:**

* Consulta a Spotify API con cache
* Manejo de limites (429 Retry-After) y rotación de credenciales
* Parsing de metadatos (artista, genero, duraciones, popularidad)

**Output:**

* `spotify_weekly_integrated.csv`
* `spotify_weekly_errors.csv` (vacío)

---

### **Fase 3 – Preparing Final Dataframe**

**Archivo:** `Fase 3 - Preparing Final Dataframe for Analysis.ipynb`

**Input:**

* `spotify_weekly_integrated.csv`

**Procesos principales:**

* Transformaciones basica: Creación de columna id, duration_min y cambio formato year (float -> int)
* Check fila duplicadas
* Detección y ELIMINACIÓN de registros NO SONGS en **Outliers** (over 15 min y under 1.18 min)
* Detección y extracción de registros **mismatch** → creación del file `mismatch_artist_para_api.csv` para reingración futura
* ELIMINACIÓN de **mismatch** (casi 15% de dataset perdido)
* Check missing genre y propagacion por artista de los missing genre
* Check unique 'spotify_genres' (447 generos musicales diferentes)
* Mapping y Asignación por 18 macro generos msuicales `macro_genre_all`, definicción de `primary_macro_genre` 
* Check registros sospechosos NO SONG y ELIMINACÓN
* Creación de columnas para analisis: `era_tecnologica` y columnas booleana por cada macro genero
* Check final duplicados y consistencia
* Creación dataset completo y dataset reducido para ANOVA

**Output:**

* `mismatch_artist_para_api.csv`
* `spotify_clean_full.csv`
* `spotify_clean_for_anova.csv`

---

### **Fase 4 – ANOVAs y visualizaciones**

**Archivo:** `Fase 4 - ANOVAs y GRAFICOS.ipynb`

**Input:**

* `spotify_clean_full.csv`
* `spotify_clean_for_anova.csv`

**Procesos principales:**

* Transformaciones basicas de columnas (creacion de duration_sec y primary_macro_genre as str)
* Funciones ANOVA, Tuckey, Boxplot
* ANOVA y Comparaciones Tukey por: `era_tecnologica`,`primary_macro_genre`, interaccion de `era_tecnologica` y `primary_macro_genre`, `popularity`,`explicit`
* Graficos:
  * distribución de duration_min, distribución de AVG_duration_min
  * Heatmap distribución macro generos por año, tambien en version normalizada
  * Heatmaps de co-ocurrencias de generos, tambien en version normalizada
  * Grafico sunburst por combinaciones de generos musicales

**Output:**

* Sin CSV; solo visualizaciones y tablas.

---

## 3. Dataframes principales

(Ver archivo separado `README_DATAFRAMES.md`)

---

## 4. Consideraciones importantes

* ~15% de datos se clasifican como mismatch y se guardan por separado.
* La integracion Spotify mantiene metadatos completos para analisis avanzados.

---

## 5. Bibliografía
DA AGGIUNGERE DOPO TI MANDO IL MATTERIALE
* Billboard Hot 100 Dataset – Kaggle
* Spotify Developer API
* Documentación oficial de ANOVA y Tukey HSD
