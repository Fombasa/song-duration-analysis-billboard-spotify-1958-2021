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

Dataset

Dhar, D. (2021). Billboard The Hot 100 Songs (1958–2021) [Dataset]. Kaggle.
https://www.kaggle.com/datasets/dhruvildave/billboard-the-hot-100-songs

APIs y herramientas

Spotify. (2024). Spotify for Developers — Web API Documentation.
https://developer.spotify.com/dashboard

Spotipy. (2024). Spotipy: A lightweight Python library for the Spotify Web API.
https://spotipy.readthedocs.io/en/2.25.2/

Navodas, A. (2023). Using Spotify API for data collection. Medium.
https://medium.com/@navodas88/using-spotify-api-for-data-collection-537ea2b4c0ab

Lopez, E. (2023). Data analysis with Spotify API. Medium.
https://medium.com/@eelopez088/data-analysis-with-spotify-api-a1507f48e9b0

Análisis estadístico

Seabold, S., & Perktold, J. (2010). Statsmodels: Econometric and statistical modeling with Python.
https://www.statsmodels.org/stable/anova.html

Statsmodels. (2024). Tukey HSD function documentation.
https://www.statsmodels.org/stable/generated/statsmodels.stats.multicomp.pairwise_tukeyhsd.html

Scikit-Posthocs. (2023). User Guide.
https://scikit-posthocs.readthedocs.io/en/latest/

Artículos y análisis sobre duración musical

Chen, Y.-W., & Jan, S.-S. (2011). Song duration: Implications for popular music and listener engagement.
Journal of New Music Research, 40(3), 239–248.
https://doi.org/10.1080/09298215.2011.603685

Chartmetric. (2023). Why songs are getting shorter: A streaming-era analysis.
https://hmc.chartmetric.com/shorter-songs-trend-streaming-history/

BBC News. (2019). Are pop songs really getting shorter?
https://www.bbc.co.uk/news/resources/idt-052ab668-403d-416f-b5a6-c5692313b9b4

RadioFidelity. (2022). The science of song length: Is 3 minutes really the best?
https://radiofidelity.com/the-science-of-song-length-is-3-minutes-really-the-best/

