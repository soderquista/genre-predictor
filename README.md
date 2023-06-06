# genre-predictor
Based on text classification, we aim to build a machine learning model that predicts the genre of a song based on qualities of the song (e.g., energy, instrumentals, acoustic vs. electronic, loudness) and its lyrics.

## Building the Dataframe Training Set
To gather data on songs in 8 chosen genres (rock, pop, rap, hip hop, heavy metal, country, techno, and electronica), a Spotify playlist was constructed with 15 songs in each category (120 songs total). An API request pulled information about each of the songs on the playlist and added the information to a dataframe. Information gathered included:

* track_ids (in order to query more information about individual songs)
* track_names (song titles)
* artist_names
* acousticness - Acoustic music solely or primarily uses instruments that produce sound through acoustic means, as opposed to electric or electronic means. In a measure of a song's 'acousticness,' a score of 1.0 means the song is most likely to be an acoustic one.
* danceability - 'Danceability' describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity. A value of 0.0 is least danceable and 1.0 is most danceable.
* energy - 'Energy' represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy
* instrumentalness - This value represents the amount of vocals in the song. The closer it is to 1.0, the more instrumental the song is.
* liveness - This value describes the probability that the song was recorded with a live audience. A value above 0.8 provides strong likelihood that the track is live.
* loudness - How loud a song is, from 0.0 to 1.0.
* speechiness - Speechiness detects the presence of spoken words in a track. If the speechiness of a song is above 0.66, it is probably made of spoken words, a score between 0.33 and 0.66 is a song that may contain both music and words, and a score below 0.33 means the song does not have any speech.
* tempo - How fast a song is, from 0.0 to 1.0.
* valence - A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).

In addition to the above information, an API request also pulled song lyrics from the Genius song lyrics website for each song. These lyrics were merged with the dataframe. 

Extraneous, non-lyric words were removed using regEX and individual words were counted. The count of original words in each song was appended to the dataframe. 

All of the above information and the process to gather and clean it is contained in the `API-requests.ipynb` file.

All of the dataframe information was exported to a JSON (`distinct_lyric_df.json`). This was then uploaded to MongoDB (`distinct_lyric_df_sql.json`) to help improve the data formatting.

Initial trends were observed (visualizations in `Matplotlib_Visualizations.ipynb`). There was a positive correlation between energy and loudness, and a negative correlation between energy and acousticness in the songs sampled.

## Machine Learning Model
Scikit-learn was used to build a multiclass classification model. The data was then split into training and testing sets. A random forest classifier was used to create the predictive model. 

The machine learning model had difficulty due to the small sample size of each genre, so the 8 genres were combined into 4: rap/hip hop, pop/country, techno/electronica, and rock/metal.

The total accuracy of predicting which of the 4 genre groups a test song would be classified as was 77%.

## Data Credit
Description of Spotify song data is from the article <a href='https://towardsdatascience.com/is-my-spotify-music-boring-an-analysis-involving-music-data-and-machine-learning-47550ae931de'>'Is my Spotify music boring? An analysis involving music, data, and machine learning'</a> by Juan De Dios Santos (May 28, 2017).
