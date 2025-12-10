# Proyecto: Analisis Billboard + integracion Spotify API

![IT Academy Logo](https://www.barcelonactiva.cat/documents/20124/1625465/it_academy_logo.png)

---

# 1. Introducción

Este proyecto analiza cómo han cambiado las **duraciones de las canciones populares** a lo largo de más de seis décadas, utilizando como base el dataset histórico del **Billboard Hot 100 (1958–2021)**.

Para enriquecer el análisis, se integraron metadatos de **Spotify API**, incorporando información clave como **duración precisa**, **géneros musicales**, **popularidad** y **atributos del artista**.

La combinación de ambas fuentes permite estudiar cómo evoluciona la música en relación con los cambios tecnológicos del consumo: del vinilo al CD, del MP3 al streaming.

El foco principal del análisis se centra en:
- la variación histórica de la duración de las canciones,
- la distribución de géneros musicales,
- y su comparación entre distintas **eras tecnológicas**.
<<<

Link to dataset original:  
https://www.kaggle.com/datasets/dhruvildave/billboard-the-hot-100-songs?resource=download


---

# 2. Problema de investigación

Explorar cómo han variado la **duración**, los **géneros musicales**, y variables adicionales (*popularity* y *explicit*) en el Billboard Hot 100 a lo largo de diferentes **eras tecnológicas** de consumo musical.

## Objetivos del proyecto

1. **Integrar y unificar** los datos históricos de Billboard con los metadatos de Spotify en un dataset estructurado.
2. **Comparar estadísticamente** (ANOVA + Tukey HSD):
   - la duración media entre eras tecnológicas,  
   - las diferencias entre géneros musicales,  
   - la interacción **era tecnológica × género musical** sobre la duración.
   - efecto de las variables Popularity y Explicit sobre la duración.
3. **Analizar la evolución y co-ocurrencia** de los géneros musicales mediante heatmaps y agregaciones.
4. **Detectar patrones históricos (en general)** que describan cómo cambia la música popular entre 1958 y 2021.
<<<

---
 
# 3. Cómo ejecutar el proyecto

La ejecución completa del proyecto se realiza siguiendo los notebooks en orden (Fase 1 → Fase 4), tal como se describe en la Pipeline.  
Cada fase genera o utiliza sus respectivos archivos CSV, disponibles en la carpeta `/data/`.
<<<

---

# 4. Pipeline del proyecto
Pipeline completa en 4 fases, con sus relativos dataframes.

---

## **Fase 1 – Kaggle Cleaning (Billboard)**

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

## **Fase 2 – Spotify API Integration**

**Archivo:** `Fase 2 - Spotify API integration.ipynb`

**Input:**

* `billboard_weekly_ready.csv`

**Procesos principales:**

* Recuperación de metadatos Spotify (track_id, track_name, popularity, etc.)  
* Uso de cache y manejo de límites (429 Retry-After)  
* Rotación de credenciales  
* Parsing y limpieza de metadatos Spotify  

**Output:**

* `spotify_weekly_integrated.csv`  
* `spotify_weekly_errors.csv` (vacío)

---

## **Fase 3 – Preparing Final Dataframe**

**Archivo:** `Fase 3 - Preparing Final Dataframe for Analysis.ipynb`

**Procesos principales:**

* Creación columna id, duration_min  
* Cambio formato year  
* Eliminación de outliers NO SONGS (over 15 min / under 1.18 min)  
* Extracción registros mismatch → `mismatch_artist_para_api.csv`  
* Eliminación de mismatch (~15%)  
* Propagación de géneros faltantes  
* Asignación de 18 macro géneros  
* Creación columnas booleans y `era_tecnologica`  
* Check duplicados y consistencia  

**Output:**

* `mismatch_artist_para_api.csv`  
* `spotify_clean_full.csv`  
* `spotify_clean_for_anova.csv`

---

## **Fase 4 – ANOVAs y visualizaciones**

**Archivo:** `Fase 4 - ANOVAs y GRAFICOS.ipynb`

**Input:**

* `spotify_clean_full.csv`  
* `spotify_clean_for_anova.csv`

**Procesos principales:**

* ANOVA y Tukey HSD  
* Boxplots  
* Heatmaps (absolutos y normalizados)  
* Sunburst por combinaciones de géneros  

**Output:**  
Solo visualizaciones (no se generan nuevos CSV)

---

# 5. Dataframes principales

(Ver archivo separado: `README_DATAFRAMES.md`)

---

# 6. Conclusiones

### **Sobre la duración de las canciones**  
El análisis ANOVA confirma diferencias estadísticas significativas en la duración media entre eras tecnológicas. Tukey HSD identificó qué todas las eras difieren entre sí.

### **Sobre los géneros musicales**  
Los heatmaps muestran cambios claros en la presencia relativa de los géneros a lo largo de décadas, destacando transiciones estilísticas visibles en el tiempo. el análisis ANOVA confirma diferencias estadísticas significativas en la duración media entre los varios generos musicales. Tukey HSD identificó qué 55 combinaciónes de generos sobre 153 muestran diferencias entre ellos.

### **Sobre la relación entre eras tecnológicas y características musicales**  
Aunque no implica causalidad, se observan patrones consistentes entre formatos de consumo y características musicales predominantes.

### **Sobre la integración de datos**  
La unificación Billboard + Spotify permite construir un dataset histórico robusto, adecuado para análisis descriptivos e inferenciales sobre tendencias de la música popular.
<<<

---

# 7. Presentación del proyecto

La presentación completa del proyecto está disponible en la carpeta `/slides/` o online en Canva:

https://www.canva.com/design/DAG5hj7Qr9w/igb6U4MpRcMc99cmMIfN0A/view
<<<


---

# 8. Bibliografía


