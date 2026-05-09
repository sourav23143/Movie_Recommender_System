# Movie Recommender System

A content-based movie recommendation system built with Python that uses machine learning to suggest movies similar to a user's selection. The system features a web interface powered by Streamlit and displays movie posters from The Movie Database (TMDB).

## 📋 Project Overview

This project implements a **content-based filtering** recommendation algorithm that analyzes movie metadata (genres, keywords, cast, crew, and descriptions) to find and recommend similar movies. Users can select any movie from a dropdown menu and receive 5 personalized movie recommendations with posters.

### Key Features
- **Content-Based Recommendations**: Uses movie metadata to find similar films
- **Interactive Web Interface**: Built with Streamlit for easy user interaction
- **Movie Posters**: Fetches and displays poster images from TMDB API
- **Scalable**: Handles thousands of movies efficiently using cosine similarity
- **Real-time Search**: Dropdown menu allows quick movie selection and search

---

## 🏗️ Project Architecture

```
Movie_Recommender_System/
├── app.py              # Streamlit web application
├── model.ipynb         # Jupyter notebook for model training
├── requirements.txt    # Python dependencies
└── README.md           # This file
```

### How It Works

#### 1. **Model Training (model.ipynb)**

The Jupyter notebook performs the following steps:

1. **Data Loading**: Loads TMDB movie dataset containing:
   - Movie metadata (titles, overviews, genres, etc.)
   - Credit information (cast, crew, directors)

2. **Data Processing**:
   - Merges movies and credits datasets
   - Extracts relevant features: genres, keywords, cast (top 3), and director
   - Removes spaces from names to improve text matching
   - Combines all features into a "tags" column

3. **Feature Engineering**:
   - Uses `CountVectorizer` to convert text to numerical features
   - Creates a sparse matrix with max 5,000 features
   - Removes English stop words for better feature quality

4. **Similarity Calculation**:
   - Computes **cosine similarity** between all movies
   - Produces a similarity matrix where each value represents how similar two movies are (0 to 1)

5. **Model Serialization**:
   - Saves the movie list to `movie_list.pkl`
   - Saves the similarity matrix to `similarity.pkl`

#### 2. **Web Application (app.py)**

The Streamlit app provides a user-friendly interface:

1. **Model Loading**: Loads pre-trained model files from the `model/` directory
2. **User Input**: Displays a dropdown menu with all available movies
3. **Recommendation Engine**: 
   - When a movie is selected and "Show Recommendation" button is clicked
   - Retrieves the 5 most similar movies using the pre-computed similarity matrix
   - Fetches movie posters from TMDB API
4. **Display**: Shows 5 recommended movies with their posters in a grid layout

---

## 🔧 Installation & Setup

### Prerequisites
- Python 3.7 or higher
- pip (Python package manager)
- Internet connection (for TMDB API calls)

### Step 1: Clone/Download the Project
```bash
cd Movie_Recommender_System
```

### Step 2: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 3: Train the Model
Run the Jupyter notebook to generate the required model files:
```bash
jupyter notebook model.ipynb
```

Or use VS Code Jupyter extension:
- Open `model.ipynb` in VS Code
- Run all cells to generate `movie_list.pkl` and `similarity.pkl`
- These files should be saved in a `model/` folder in the project directory

**Note**: The training notebook expects data files in a specific location. You may need to:
- Download TMDB movie dataset from Kaggle
- Adjust file paths in the notebook to match your local setup

### Step 4: Run the Web Application
```bash
streamlit run app.py
```

The app will open in your default browser at `http://localhost:8501`

---

## 📦 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| streamlit | 1.51.0 | Web application framework |
| scikit-learn | 1.7.2 | Machine learning library (CountVectorizer, cosine_similarity) |
| pandas | 2.3.3 | Data manipulation and analysis |
| numpy | 2.3.5 | Numerical computations |
| requests | 2.32.5 | HTTP requests for TMDB API |
| pickle | Built-in | Model serialization |

See `requirements.txt` for the complete list of dependencies.

---

## 🎬 Usage Guide

### Running the Application

1. **Start the app**:
   ```bash
   streamlit run app.py
   ```

2. **Select a movie**:
   - Type or click on a movie title in the dropdown menu
   - The menu is searchable - just type to filter movies

3. **Get recommendations**:
   - Click the "Show Recommendation" button
   - The system displays 5 similar movies with posters

4. **Try another movie**:
   - Select a different movie and click the button again
   - No need to restart the app

### Example
```
Input: "The Avengers"
Output: 
  - Iron Man 2
  - Captain America: The First Avenger
  - Thor
  - The Avengers: Age of Ultron
  - Guardians of the Galaxy
```

---

## 🤖 Machine Learning Algorithm

### Content-Based Filtering
This system uses **content-based filtering**, which works by:
1. Analyzing features of items (movies) the user likes
2. Finding items with similar features
3. Recommending those similar items

### Similarity Metric
- **Algorithm**: Cosine Similarity
- **Formula**: $\cos(\theta) = \frac{A \cdot B}{||A|| \times ||B||}$
- **Range**: 0 to 1 (1 means identical, 0 means completely different)

### Features Used
- **Genres**: Movie categories (Action, Drama, Comedy, etc.)
- **Keywords**: Key themes and concepts from the movie plot
- **Cast**: Main actors in the movie (top 3)
- **Director**: The film's director
- **Overview**: Brief movie description/synopsis

---

## Dataset Information

The project uses the **TMDB 5000 Movie Dataset** containing:
- **5,000+ movies** with complete metadata
- **Movie information**: Title, overview, genres, release date, popularity, budget
- **Credit information**: Cast members and crew (directors, producers, etc.)

**Data Source**: Available on Kaggle at https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata

---

## 🔌 API Integration

### TMDB API
The application uses The Movie Database (TMDB) API to fetch movie posters:

```python
# Endpoint format
https://api.themoviedb.org/3/movie/{movie_id}?api_key={API_KEY}&language=en-US

# Poster URL format
https://image.tmdb.org/t/p/w500/{poster_path}
```

**API Key**: Embedded in the code (hardcoded API key - in production, use environment variables)

**Rate Limiting**: TMDB API has rate limits; the app respects these by making on-demand requests

---

## ⚠️ Important Notes

### Model Files
- The app requires `model/movie_list.pkl` and `model/similarity.pkl`
- These are generated by running the `model.ipynb` notebook
- Without these files, the app will show an error message
- The pickle files contain pre-computed similarity scores for fast recommendations

### API Key
- The TMDB API key is currently hardcoded in `app.py`
- For production deployment, move this to environment variables
- Never commit API keys to version control

### Performance
- First recommendation may take a moment (API call to fetch poster)
- Subsequent recommendations are fast (data already cached)
- The similarity matrix is pre-computed, so recommendations are instant

### Limitations
- Recommendations are based only on content metadata, not user ratings
- No personalization based on individual user preferences
- Cold start problem: New users get generic recommendations
- Limited to movies in the TMDB dataset

---

## 🚀 Future Enhancements

### Possible Improvements
1. **Collaborative Filtering**: Add user-based recommendations using ratings
2. **Hybrid Approach**: Combine content-based and collaborative filtering
3. **User Ratings**: Let users rate movies to improve recommendations
4. **Search & Filter**: Add genre, year, and rating filters
5. **Favorites List**: Allow users to bookmark recommended movies
6. **Statistics**: Show why movies are recommended (shared genres, cast, etc.)
7. **Mobile App**: Develop mobile version using React Native or Flutter
8. **Database Integration**: Replace pickle files with proper database (PostgreSQL, MongoDB)
9. **Caching**: Cache API responses to reduce TMDB API calls
10. **A/B Testing**: Compare different recommendation algorithms

---

## 🐛 Troubleshooting

### Issue: "Model files not found"
**Solution**: Run the `model.ipynb` notebook to generate the pickle files. Make sure they're saved in a `model/` folder.

### Issue: Movie posters not displaying
**Solution**: 
- Check internet connection
- Verify TMDB API key is valid
- Check if the API has rate limit exceeded (wait a moment and try again)

### Issue: Streamlit app won't start
**Solution**:
- Ensure all dependencies are installed: `pip install -r requirements.txt`
- Check Python version is 3.7+: `python --version`
- Try running with verbose mode: `streamlit run app.py --logger.level=debug`

### Issue: Slow recommendations
**Solution**: 
- First run includes poster fetching from API (normal delay)
- Subsequent recommendations are cached and should be instant
- If consistently slow, check internet connection

---

## 📄 File Descriptions

### `app.py`
The Streamlit web application that serves as the user interface.

**Key Functions**:
- `fetch_poster()`: Fetches movie poster URL from TMDB API
- `recommend()`: Generates 5 recommendations based on cosine similarity
- UI components: Header, selectbox, button, columns for layout

**Dependencies**: streamlit, requests, pickle, pandas

### `model.ipynb`
Jupyter notebook for training the recommendation model.

**Key Steps**:
1. Data loading and exploration
2. Data merging and cleaning
3. Feature extraction
4. Text vectorization with CountVectorizer
5. Similarity matrix computation
6. Model persistence with pickle

**Output Files**: `movie_list.pkl`, `similarity.pkl`

### `requirements.txt`
Lists all Python package dependencies and their versions.

---

## 👤 Author & License

This is an educational project demonstrating content-based recommendation systems.

**License**: Open source (feel free to use and modify)

**Data Source**: TMDB dataset via Kaggle

---

## 📚 Learning Resources

### Topics Covered
- **Content-Based Filtering**: https://en.wikipedia.org/wiki/Recommender_system#Content-based_filtering
- **Cosine Similarity**: https://en.wikipedia.org/wiki/Cosine_similarity
- **CountVectorizer**: https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html
- **Streamlit Documentation**: https://docs.streamlit.io/

### Related Topics
- Collaborative Filtering
- Matrix Factorization (SVD)
- Deep Learning for Recommendations
- Natural Language Processing (NLP)
- Feature Engineering

---

## 🔗 Useful Links

- **TMDB API**: https://www.themoviedb.org/settings/api
- **TMDB Dataset**: https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata
- **Streamlit Docs**: https://docs.streamlit.io/
- **Scikit-learn**: https://scikit-learn.org/
- **Python Documentation**: https://docs.python.org/3/

---

## Support

For issues, questions, or improvements:
1. Check the Troubleshooting section above
2. Review the inline code comments
3. Consult documentation of individual libraries
4. Run cells step-by-step in the notebook to debug

---

**Last Updated**: May 9, 2026  
**Version**: 2.0
