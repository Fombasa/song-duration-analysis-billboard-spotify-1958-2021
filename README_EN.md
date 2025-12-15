# Project: Billboard Analysis + Spotify API Integration

![IT Academy Logo](https://www.barcelonactiva.cat/documents/20124/1625465/it_academy_logo.png)

Final project developed during the IT Academy Data Analytics Bootcamp (Barcelona Activa).

### Summary and key findings

Historical analysis of song duration in the Billboard Hot 100 (1958–2021), based on the integration and cleaning of Billboard data with Spotify API metadata.

**Average song duration changes significantly across technological eras**, confirming a structural effect of the music consumption context (ANOVA, p < 0.01, Tukey HSD).

**Significant differences in duration are observed across musical genres**, with multiple genre combinations showing statistically relevant contrasts (ANOVA, p < 0.01, Tukey HSD).

**The historical evolution of genres reveals clear stylistic transition patterns**, visible through heatmaps and temporal aggregations, reflecting cultural and technological changes in popular music.

### Historical evolution of average song duration

![AVG Songs Duration per Year](figures/avg%20duration%20per%20year%20era%20tecnologica.png)

*Average song duration increases progressively from the vinyl era, reaches its peak during the CD era, and declines clearly with the consolidation of digital consumption and streaming.*

## How to run the project

1. Clone the repository.
2. Download the required CSV files into the `/data/` folder.
3. Open the Jupyter Notebooks located in the `/notebooks/` folder.
4. Adjust local *file paths* in the notebooks according to your environment.
5. Add Spotify API credentials in the notebook corresponding to Phase 2.
6. Run the notebooks in order (Phase 1 → Phase 4).

**Disclaimer:** Spotify API credentials are not included in this repository for security reasons.  
Each user must generate their own credentials via Spotify for Developers.

---

# 1. Introduction

This project analyzes how **popular song durations** have evolved over more than six decades, using the historical **Billboard Hot 100 (1958–2021)** dataset as its foundation.

To enrich the analysis, **Spotify API metadata** were integrated, incorporating key information such as **precise duration**, **musical genres**, **popularity**, and **artist attributes**.

The combination of both sources allows for the study of how music evolves in relation to technological shifts in consumption: from vinyl to CD, from MP3 to streaming.

The main focus of the analysis is:
- the historical variation in song duration,
- the distribution of musical genres,
- and their comparison across different **technological eras**.

Link to original dataset:  
https://www.kaggle.com/datasets/dhruvildave/billboard-the-hot-100-songs?resource=download

---

# 2. Research question

To explore how **song duration**, **musical genres**, and additional variables (*popularity* and *explicit*) have changed within the Billboard Hot 100 across different **technological eras** of music consumption.

## Project objectives

1. **Integrate and unify** historical Billboard data with Spotify metadata into a structured dataset.
2. **Statistically compare** (ANOVA + Tukey HSD):
   - average duration across technological eras,  
   - differences between musical genres,  
   - the interaction **technological era × musical genre** on duration,  
   - the effect of *Popularity* and *Explicit* variables on duration.
3. **Analyze the evolution and co-occurrence** of musical genres using heatmaps and aggregations.
4. **Identify historical patterns** describing how popular music has changed between 1958 and 2021.

### Average duration by genre and technological era

![Heatmap AVG duration x genre x era](figures/heatmap%20avg%20duration%20x%20primary%20genre%20x%20era%20tecnologica.png)

*The interaction between musical genre and technological era reveals clear differences in average duration, confirming that technological change does not affect all musical styles uniformly.*

---

# 3. How to run the project

The full execution of the project follows the notebooks sequentially (Phase 1 → Phase 4), as described in the pipeline.  
Each phase generates or consumes its corresponding CSV files, available in the `/data/` folder.

---

# 4. Project pipeline

Complete pipeline structured into four phases, with their respective dataframes.

---

## **Phase 1 – Kaggle Cleaning (Billboard)**

**Notebook:** `Phase 1 - Kaggle cleaning - Billboard The Hot 100 Songs.ipynb`

**Input:**
- `Kaggle - Billboard Hot 100 dataset.csv`

**Main processes:**
- Initial cleaning  
- Date conversion and creation of the `year` column  
- Duplicate row checks  
- Field normalization  
- Creation of the combined `song_artist` column for API queries  
- Generation of the annual **Top100 ranking** (not used in later phases) → saved as `billboard_top100_annual.csv`

**Output:**
- `billboard_weekly_ready.csv` (used in Phase 2)  
- `billboard_top100_annual.csv`

---

## **Phase 2 – Spotify API Integration**

**Notebook:** `Phase 2 - Spotify API integration.ipynb`

**Input:**
- `billboard_weekly_ready.csv`

**Main processes:**
- Retrieval of Spotify metadata (track_id, track_name, popularity, etc.)  
- Caching and rate-limit handling (429 Retry-After)  
- Credential rotation  
- Parsing and cleaning of Spotify metadata  

**Output:**
- `spotify_weekly_integrated.csv`  
- `spotify_weekly_errors.csv` (empty)

---

## **Phase 3 – Preparing Final Dataframe**

**Notebook:** `Phase 3 - Preparing Final Dataframe for Analysis.ipynb`

**Main processes:**
- Creation of `id` and `duration_min` columns  
- Year format normalization  
- Removal of non-song outliers (over 15 min / under 1.18 min)  
- Extraction of mismatch records → `mismatch_artist_para_api.csv`  
- Removal of mismatches (~15%)  
- Propagation of missing genres  
- Assignment of 18 macro-genres  
- Creation of boolean columns and `era_tecnologica`  
- Duplicate and consistency checks  

**Output:**
- `mismatch_artist_para_api.csv`  
- `spotify_clean_full.csv`  
- `spotify_clean_for_anova.csv`

---

## **Phase 4 – ANOVAs and visualizations**

**Notebook:** `Phase 4 - ANOVAs and PLOTS.ipynb`

**Input:**
- `spotify_clean_full.csv`  
- `spotify_clean_for_anova.csv`

**Main processes:**
- ANOVA and Tukey HSD  
- Boxplots  
- Heatmaps (absolute and normalized)  
- Sunburst plots for genre combinations  

**Output:**  
Visualizations only (no new CSV files generated)

---

# 5. Main dataframes

(See separate file: `README_DATAFRAMES.md`)

---

# 6. Conclusions

### **Data integration**  
The combination of Billboard and Spotify data enables the construction of a robust historical dataset, suitable for descriptive and inferential analysis of popular music trends.

### **Song duration**  
ANOVA results confirm statistically significant differences in average song duration across technological eras. Tukey HSD indicates that **all eras differ from one another**.  
These results confirm that song duration is not only an artistic decision, but also a structural response to the technological context.

### **Musical genres**  
Genre-based analyses reveal significant differences in average duration across musical styles.  
Post-hoc analysis identified **55 genre combinations (out of 153)** with statistically significant differences.

### Genre relationships (co-occurrences)

![Co-occurrence genres normalized](figures/co%20occurence%20matrix%20macro%20genres%20normalized.png)

*The normalized co-occurrence matrix highlights stylistic affinities between genres that frequently coexist within the same song, revealing recurrent relationships between related musical styles.*

---

# 7. Project presentation

The full project presentation is available in the `/slides/` folder or online via Canva:

https://www.canva.com/design/DAG5hj7Qr9w/igb6U4MpRcMc99cmMIfN0A/view

---

## Acknowledgements

Project developed as part of the Data Analytics Reskilling Program at IT Academy (Barcelona Activa).  
Special thanks to Joan and all mentors and peers for their support throughout the project.

---

## Author

**Edoardo Marchionni**  
Data Analyst  

- LinkedIn: https://www.linkedin.com/in/edoardomarchionni/  
- GitHub: https://github.com/Fombasa
