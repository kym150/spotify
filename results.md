---
title: Results
---

## Results

Our result: a recommender system that can take a song as an input and produce a list of desired length. For example, after inputting 'HUMBLE' by Kendrick Lamar, we are given the following playlist.

```python
#if there are multiple artists
SR.recommend_me_some_songs(database, unique_tracks,'HUMBLE.')
```


    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:48: UserWarning: More than one artist with the song name. Please specify the artist name. Your options are: ['Kendrick Lamar' 'Our Last Night']
      warnings.warn(f"More than one artist with the song name. Please specify the artist name. Your options are: {unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()}")




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
