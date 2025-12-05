# Documentación de Dataframes

Esta sección describe **todos los dataframes** generados a lo largo del proyecto, con un enfoque claro y sin repeticiones innecesarias.

Se divide en:

1. **Lista de dataframes en orden de aparición**, con una breve descripción.
2. **Categorías de columnas** (explicadas en detalle).
3. **Notas clave sobre la transformación de datos**.

---

# 🔹 1. Lista de Dataframes (en orden de pipeline)

## ** Dataset original**

### `Kaggle - Billboard Hot 100 dataset.csv`

Dataset original (1958–2021) descargado desde Kaggle.
link: https://www.kaggle.com/datasets/dhruvildave/billboard-the-hot-100-songs?resource=download
Contiene posiciones semanales Billboard Hot 100.

---

## ** Fase 1 — Limpieza inicial**

### `billboard_weekly_ready.csv`

Dataset semanal limpio preparado para integrarse con Spotify API.

### `billboard_top100_annual.csv`

Dataset auxiliar con el ranking anual calculado.
NO se usa en fases posteriores.

---

## ** Fase 2 — Integración Spotify API**

### `spotify_weekly_integrated.csv`

Dataset integrado con metadatos de Spotify ( géneros, duración...).

### `spotify_weekly_errors.csv`

Archivo de errores API.
Normalmente vacío.

---

## ** Fase 3 — Limpieza avanzada y dataset final**

### `mismatch_artist_para_api.csv`

Registros descartados (~15%) por mismatch entre Billboard y Spotify.
No continúa en la pipeline.
!!!! Advertencia: A partir de esta fase, todos los dataframes contienen solo el 85% de los registros originales.
Los datos descartados no se reintegran en ninguna etapa posterior, de momento.

### `spotify_clean_full.csv`

Dataset completo post-limpieza, con columnas derivadas, género musical y era tecnológica.

---

## ** Fase 4 — Dataset para análisis estadístico**

### `spotify_clean_for_anova.csv`

Subset optimizado para ANOVA y análisis estadísticos. Contiene solo 8 columnas clave.

---

# 🔹 2. Categorías de columnas

## **A) Columnas originales de KAGGLE (Billboard Hot 100)**
da verificare assieme il contenuto esatto dei vari dataframe e l' orginie delle colonne
aggikungere breve descrizione epr le colonne necessarie (trovarei gia qualche appunto)
* date
* rank / position
* song
* artist / artist_main
* last-week
* peak-rank
* weeks-on-board
* year
* song_artist

---

## **B) Columnas importadas desde Spotify API**

* track_id
* track_name
* track_popularity (breve descrizione della statistica popularity secondo spotify)
* album_name
* release_date
* duration_ms
* explicit
* artist_id
* artist_name
* spotify_genres

---

## **C) Columnas derivadas durante la limpieza** (forse il modo corretto  `e colonne per analisi?)

* duration_min (mins)
* primary_macro_genre
* macro_genres_all
* spotify_genres_list
* era_tecnologica

---

## **D) Columnas booleanas (18 macro géneros)**

Ejemplos:

* genre_pop
* genre_rock
* genre_jazz
  *(y las demás 15 categorías equivalentes)*

---

## **E) Columnas estadísticas para ANOVA**

(Subset reducido)

* id
* duration_min (vedo ripetizioni che vanno corrette)
* primary_macro_genre
* era_tecnologica
* track_popularity
* position
* explicit
* year

---

# 🔹 3. Notas clave sobre la transformación

* ~15% de registros se clasifican como mismatch y se almacenan aparte.
* Se eliminan outliers de duración ( >15 min o <1.18 min ).
* Se propagan géneros faltantes basándose en artistas.
* Se asignan 18 macro géneros musicales.
* Se crea una clasificación de eras tecnológicas.

---


