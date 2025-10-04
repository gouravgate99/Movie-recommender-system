# Movie Recommender System (Content-Based Filtering)

## Overview
This project implements a **content-based movie recommender system**. The system analyzes the text metadata (genres, keywords, cast, crew, and plot overview) of movies to find similarities between them. When a user selects a movie, the system recommends other movies that share similar characteristics.

## Dataset
The project utilizes the **TMDB 5000 Movie Metadata** dataset, which consists of two files:
1.  `tmdb_5000_movies.csv`
2.  `tmdb_5000_credits.csv`

The final merged dataset contains approximately **4,803 movies**.

## Methodology and Algorithm

The recommendation is based on **Cosine Similarity** applied to a vectorized representation of key movie features.

### 1. Data Processing and Feature Extraction
The following features were extracted and processed to create a unified "tag" for each movie:

* **Feature Selection:** Merged the datasets on `title` and retained only `movie_id`, `title`, `overview`, `genres`, `keywords`, `cast`, and `crew`.
* **Structured Data Cleaning:** The `genres`, `keywords`, `cast`, and `crew` columns, which were stored as lists of dictionaries (JSON strings), were converted into simple lists of names (e.g., extracting "Action," "Adventure").
* **Cast and Crew Filtering:**
    * Only the **top 3 actors** from the `cast` list were retained.
    * Only the name of the **Director** was extracted from the `crew` list.
* **Text Transformation:** All extracted names (genres, keywords, cast, director) were pre-processed by removing spaces (e.g., 'Science Fiction' became 'ScienceFiction') to ensure multi-word entities are treated as single tags during vectorization.

### 2. Vectorization and Similarity Calculation
* **Tag Creation:** A single `tags` string was created for each movie by concatenating the processed `overview`, `genres`, `keywords`, `cast`, and `director` fields.
* **Text Vectorization:** A **CountVectorizer** was applied to the `tags` column, limiting the feature space to the **top 5000** most frequent words (tokens) and removing English stop words.
* **Similarity Metric:** **Cosine Similarity** was calculated between all pairs of movies based on their vectorized representations. This metric measures the angular distance, indicating how structurally similar the content of two movies is.

## 3. Recommendation Function
A `recommend(movie_title)` function was built to:
1.  Find the index of the input movie.
2.  Retrieve the similarity scores between the input movie and all other movies.
3.  Sort the movies based on these similarity scores.
4.  Return the **top 5** most similar movie titles.

## Example Recommendation
The system suggests similar movies based on the content.

| Input Movie | Top 5 Recommendations |
| :--- | :--- |
| `Gandhi` | `Gandhi, My Father`, `The Wind That Shakes the Barley`, `A Passage to India`, `Guiana 1838`, `Ramanujan` |

## Requirements
To run this project, you need the following Python libraries:
* `numpy`
* `pandas`
* `scikit-learn` (`sklearn`) - for `CountVectorizer` and calculating `cosine_similarity`.
* `ast` (Python built-in) - for safely evaluating string representations of Python literals.
* `pickle` - used to save and load the processed data and the final similarity matrix.

# Files
* `movie recommender.ipynb`: The main Jupyter Notebook containing the code.
* `tmdb_5000_movies.csv`: Movie metadata (required for data loading).
* `tmdb_5000_credits.csv`: Cast and crew data (required for data loading).
* `movie_list.pkl`: Saved list of movie data (created after processing).
* `similarity.pkl`: Saved matrix of cosine similarity scores (created after training).
