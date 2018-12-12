---
title: Modeling Approach
---

## Choosing a Model

We considered a number of modeling approaches before deciding upon our final design. 

One approach we considered was scraping the lyrics wiki database to find songs that were similar based on their lyrics. However, we decided that there weren’t enough differences within lyrics to distinguish songs from one another in a useful way (too many songs are about love!). 

We also considered splitting each playlist and then using machine learning methods to train selections of songs and using how well the list of songs reflected the list of songs in other playlists. However, we didn’t feel like this was necessarily an adequate measure of ‘success’. 

Lastly, we considered identifying songs that are in many of the same playlists together, ranked by frequency. We ultimately decided this was the best approach given the thematic power of playlists. 


## Modeling Approach

**A Playlist-Driven Model**

Our final model is rooted in the idea that the purpose of playlists is to bring different songs together under a unifying theme. The act of a Spotify user putting certain songs together on the same playlist suggests that they believe these songs are similar in some way. We used the Million Playlists Dataset, which contains actual playlists that Spotify users created. If two songs appear together on many playlists, it suggests that many playlist creators independently believed them to be similar. The power of this approach is that it removes us from having to make arbitrary decisions about the qualities of songs that make them "similar": often, these qualities are varied and indefinable. Instead, we based our determination of "similarity" on actual user behavior. 

**Developing the Model**

To develop our model, we loaded the Million Playlist database and developed an algorithm that would identify all playlists that contain the inputted song title, then count the number of playlists that each song appears in with the inputted song.

```python

def load(n = np.random.randint(0,10)):
    database = load_data.load_tracks_csv(n)
    df_unique_tracks = pd.read_csv('mpd.v1/tracks_and_counts/'+str(n)+'.csv', index_col=0)
    print('Database is loaded, song recommender ready to use.')
    return database, df_unique_tracks


def is_in_database(song_name, unique_tracks):
    return song_name in unique_tracks.track_name.unique()

def get_playlists(database, song_name, artist = None):
    '''
    Get all the playlists with the specific song 
    '''
    playlists = []
    np.random.shuffle(database)
    if type(artist) == type(None):
        for playlist in database:
                if song_name in playlist.track_name.unique():
                    playlists.append(playlist)
                    if len(playlists)>500:
                        break
    else:
        for playlist in database:
                if artist in playlist[playlist.track_name == song_name].artist_name.unique():
                    playlists.append(playlist)
                    if len(playlists)>500:
                        break
    df_playlists = pd.concat(playlists)
    return df_playlists
        
def count_duplicates(df):
    df_counts = df.groupby(df.columns.tolist()).size().reset_index().rename(columns={0:'count'})
    df_counts = df_counts.sort_values(ascending=False, by='count')
    return df_counts

def recommend_me_some_songs(database, unique_tracks, song_name, artist= None, length = 25):
    if is_in_database(song_name, unique_tracks) == False:
        warnings.warn('Sorry, the song cannot be found in this database.')
    elif type(artist) == type(None) and len(unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()) >1:
        warnings.warn(f"More than one artist with the song name. Please specify the artist name. Your options are: {unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()}")
    else:
        df_playlists = get_playlists(database, song_name, artist)
        df_counts = count_duplicates(df_playlists)
        l = min(len(df_counts),length)	
        for n in range(0, l):    
            print(f'{df_counts.iloc[n].track_name} - {df_counts.iloc[n].artist_name}')
        return df_counts[:l]


```

**Song Recommender**

Our song recommender first asks users to input the name of a song they like and specify the length (in songs) of playlist they want returned. The recommender then checks to make sure that the song appears in at least one playlist and that the song is uniquely identified with a single artist. If the song title is not unique to a single artist, the user will also be prompted to input the name of the artist associated with the inputted song.

For example, when we try to input the song "Humble" by Kendrick Lamar, we first receive a warning that there are multiple artists with that song title. We are prompted to specify the unique artist name. 

```python
SR.recommend_me_some_songs(database, unique_tracks,'HUMBLE.')
```


    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:48: UserWarning: More than one artist with the song name. Please specify the artist name. Your options are: ['Kendrick Lamar' 'Our Last Night']
      warnings.warn(f"More than one artist with the song name. Please specify the artist name. Your options are: {unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()}")


If the inputted song title is not in the database, users will receive the following warning:

```python
SR.recommend_me_some_songs(database, unique_tracks,'HI.')
```


    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:46: UserWarning: Sorry, the song cannot be found in this database.
      warnings.warn('Sorry, the song cannot be found in this database.')


Next, the recommender counts the number of times that other songs "co-appear" in the same playlist as the inputted song. It then constructs a playlist of songs that "co-appear" with the inputted song in the most playlists, in descending order with the most co-appearing song as the first song in the generated playlist.

In the same example, we input the song title, "Humble", and the artist, Kendrick Lamar. The algorithm uses this inputted song to generate the following playlist of length 25.

```python
playlist_HUMBLE = SR.recommend_me_some_songs(database, unique_tracks,'HUMBLE.', 'Kendrick Lamar')
```


    HUMBLE. - Kendrick Lamar
    DNA. - Kendrick Lamar
    Congratulations - Post Malone
    Mask Off - Future
    XO TOUR Llif3 - Lil Uzi Vert
    Bounce Back - Big Sean
    iSpy (feat. Lil Yachty) - KYLE
    Tunnel Vision - Kodak Black
    goosebumps - Travis Scott
    Bad and Boujee (feat. Lil Uzi Vert) - Migos
    Slippery (feat. Gucci Mane) - Migos
    T-Shirt - Migos
    I'm the One - DJ Khaled
    Caroline - Aminé
    Drowning (feat. Kodak Black) - A Boogie Wit da Hoodie
    rockstar - Post Malone
    Unforgettable - French Montana
    Broccoli (feat. Lil Yachty) - DRAM
    Fake Love - Drake
    Bank Account - 21 Savage
    Portland - Drake
    No Problem (feat. Lil Wayne & 2 Chainz) - Chance The Rapper
    Swang - Rae Sremmurd
    Passionfruit - Drake
    Magnolia - Playboi Carti
