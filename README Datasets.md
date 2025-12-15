# Documentación de Datasets

Esta sección describe **todos los datasets** generados a lo largo del proyecto, con un enfoque claro y sin repeticiones innecesarias.

Se divide en:

1. **Lista de datasets en orden de aparición**, con una breve descripción.
2. **Categorías de columnas** (explicadas en detalle).

---

# 1. Lista de datasets (en orden de pipeline)

---
## **`Kaggle - Billboard Hot 100 dataset.csv`**
shape: 330 087,7

Dataset original (1958–2021) descargado desde Kaggle.  
Contiene posiciones semanales del Billboard Hot 100.
- input fase 1

---
## **`billboard_weekly_ready.csv`**
shape: 330 087,9

Dataset semanal limpio preparado para integrarse con Spotify API.
- output fase 1
- input fase 2


## **`billboard_top100_annual.csv`**
shape: 6 400,9

Dataset auxiliar con el ranking anual calculado.  
NO se usa en fases posteriores.
- output fase 1

---
## **`spotify_weekly_integrated.csv`**
shape: 330 073,16

Dataset integrado con metadatos de Spotify (géneros, artista, duración, álbum, popularidad).
- output fase 2
- input fase 3

## **`spotify_weekly_errors.csv`**
shape: 0,3 (song_and_artist, date, reason)

Archivo de errores API.  
Normalmente vacío.
- output fase 2

---
## **`mismatch_artist_para_api.csv`**
shape: 49 799, 7

Registros descartados (~15%) por mismatch entre Billboard y Spotify.  
No continúa en la pipeline. 
- output fase 3

**Advertencia:** A partir de este punto, todos los datasets contienen solo ~85% de los registros originales.  
Los datos descartados NO estan reintegrados de momento.

---
## **`spotify_clean_full.csv`**
shape: 279 424, 40

Dataset completo post-limpieza, con columnas derivadas, género musical y era tecnológica.  
- output fase 3
- input fase 4
---
## **`spotify_clean_for_anova.csv`**
shape: 279 424, 8

Subset optimizado para ANOVA y análisis estadísticos.  
- output fase 3
- input fase 4

---

## 2. Categorías de columnas

A continuación se agrupan todas las columnas utilizadas en el proyecto según su origen y función.

---

## **A) Columnas originales del dataset Billboard (Kaggle)**
Estas columnas existen únicamente en el archivo original de Kaggle.

- **date** — Fecha de la semana Billboard.  
- **song** — Título de la canción.  
- **last-week** — Posición de la semana anterior.  
- **peak-rank** — Mejor posición alcanzada.  
- **weeks-on-board** — Número de semanas en lista.

- **artist** — Artista principal según Billboard.  !!Transfomada en `artist_main`  
- **rank** — Posición semanal en el Hot 100. !!Transfomada en `position` 
(rank y artist son renombrados y normalizados en la Fase 1)

---

## **B) Columnas creadas durante la limpieza inicial (Fase 1)**
Aparecen por primera vez en `billboard_weekly_ready.csv`.


- **position** — Renombrado de `rank` para consistencia con Spotify.  
- **artist_main** — Derivada de `artist`, Limpieza y estandarización del nombre del artista.
- **year** — Año extraído de la fecha.  
- **song_artist** o **song_and_artist** — Concatenación normalizada de título + artista para búsqueda en API.

---

## **C) Columnas importadas desde Spotify API (Fase 2)**
Añadidas durante la integración.

- **track_id**  
- **track_name**  
- **track_popularity** — Métrica de popularidad interna de Spotify (0–100).  
- **album_name**  
- **release_date**  
- **duration_ms**  — Métrica de duracion de canciones en milisegundos.  
- **explicit**  — Métrica de Explicit (true/false).  
- **artist_id**  
- **artist_name**  
- **spotify_genres** — Lista original de géneros según Spotify.

---

## **D) Columnas derivadas durante la limpieza avanzada (Fase 3)**
Creadas para enriquecer el análisis.

- **id** — Identificador único por fila.  
- **duration_min** — Duración de las canciones en minutos.
- **spotify_genres_list** — Lista parseada desde `spotify_genres`.  
- **macro_genres_all** — Lista de macro géneros asignados (18 categorías).  
- **primary_macro_genre** — Macro género dominante.  
- **era_tecnologica** — Clasificación por era musical (Vinilo, CD, MP3, Streaming).

---

## **E) Columnas booleanas (18 macro géneros)**
Una columna por cada macro género musical.

- **genre_pop**  
- **genre_rock**  
- **genre_jazz**  
*(+15 más, una por cada categoría asignada)*

---

## **F) Columnas estadísticas utilizadas para ANOVA**
Subset final presente en `spotify_clean_for_anova.csv`.

- **id**  
- **duration_min**
- **primary_macro_genre**  
- **era_tecnologica**  
- **track_popularity**  
- **position**  
- **explicit**  
- **year**

---


