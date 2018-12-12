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

**Song Recommender**

Our song recommender first asks users to input the name of a song they like and specify the length (in songs) of playlist they want returned. The recommender then checks to make sure that the song appears in at least one playlist and that the song is uniquely identified with a single artist. If the song title is not unique to a single artist, the user will also be prompted to input the name of the artist associated with the inputted song.

Next, the recommender counts the number of times that other songs "co-appear" in the same playlist as the inputted song. It then constructs a playlist of songs that "co-appear" with the inputted song in the most playlists, in descending order with the most co-appearing song as the first song in the generated playlist.
