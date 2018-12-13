---
title: Results
---

## Results

Our result: a recommender system that can take a song as an input and produce a list of desired length. For example, after inputting the song title 'Rain is a Good Thing' by Luke Bryan, we are given the following playlist.

```python
playlist_country = SR.recommend_me_some_songs(database, unique_tracks, 'Rain Is a Good Thing', 'Luke Bryan')
```


    Rain Is a Good Thing - Luke Bryan
    Country Girl (Shake It For Me) - Luke Bryan
    Play It Again - Luke Bryan
    Chicken Fried - Zac Brown Band
    That's My Kind Of Night - Luke Bryan
    Drunk On You - Luke Bryan
    Knee Deep (feat. Jimmy Buffett) - Zac Brown Band
    Cruise - Florida Georgia Line
    I Don't Want This Night to End - Luke Bryan
    House Party - Sam Hunt
    Barefoot Blue Jean Night - Jake Owen
    Wagon Wheel - Darius Rucker
    Springsteen - Eric Church
    Toes - Zac Brown Band
    Crash My Party - Luke Bryan
    Boys 'Round Here (feat. Pistol Annies & Friends) - Blake Shelton
    Drunk On A Plane - Dierks Bentley
    This Is How We Roll - Florida Georgia Line
    Round Here - Florida Georgia Line
    Leave The Night On - Sam Hunt
    Honey Bee - Blake Shelton
    Get Your Shine On - Florida Georgia Line
    Drink In My Hand - Eric Church
    Dirt Road Anthem - Jason Aldean
    Die A Happy Man - Thomas Rhett
   
   '''
We were able to test our model on a range of inputs and genres (for example, christmas songs and pop songs) and it made great playlists that we would enjoy listening to. 
