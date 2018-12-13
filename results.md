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
   
  
We were able to test our model on a range of inputs and genres (for example, christmas songs and pop songs) and it made great playlists that we would enjoy listening to. The playlist generator also allows you to input just a song name, and if there are multiple songs with that name, lists the artists who have songs with that track name. 

```python
playlist_bad_romance = SR.recommend_me_some_songs(database, unique_tracks,'Bad Romance')
```

    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:48: UserWarning: More than one artist with the song name. Please specify the artist name. Your options are: ['Lady Gaga' 'Artist Vs Poet' 'Lissie' 'Halestorm'
     'Vitamin String Quartet' "Scott Bradlee's Postmodern Jukebox"
     'On The Rocks' 'The Chipmunks & The Chipettes'
     'University of Rochester YellowJackets' 'Simply Three' 'Alex Goot'
     'Outline In Color' 'Berk']
      warnings.warn(f"More than one artist with the song name. Please specify the artist name. Your options are: {unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()}")

```python
playlist_bad_romance = SR.recommend_me_some_songs(database, unique_tracks, 'Bad Romance', 'Lady Gaga')
```

    Bad Romance - Lady Gaga
    Poker Face - Lady Gaga
    Just Dance - Lady Gaga
    TiK ToK - Kesha
    Party In The U.S.A. - Miley Cyrus
    Telephone - Lady Gaga
    Umbrella - Rihanna
    Hollaback Girl - Gwen Stefani
    SexyBack - Justin Timberlake
    Paparazzi - Lady Gaga
    I Gotta Feeling - The Black Eyed Peas
    California Gurls - feat. Snoop Dogg - Katy Perry
    Yeah! - Usher
    Hot N Cold - Katy Perry
    Toxic - Britney Spears
    Dynamite - Taio Cruz
    I Kissed a Girl - Katy Perry
    Super Bass - Nicki Minaj
    Don't Stop The Music - Rihanna
    Hips Don't Lie - Shakira
    Crazy In Love - Beyoncé
    Whatcha Say - Jason Derulo
    Last Friday Night (T.G.I.F.) - Katy Perry
    Disturbia - Rihanna
    Fergalicious - Fergie
    
 When we use our tag function, it provides us with a list of songs that are tagged with a preferred item. An interesting case is the Christmas playlist, where all songs that were generated were Christmas songs, but not all were tagged 'Christmas'. This demonstrates that while the tags in the last.fm dataset are accurate, they are not wholistic and not all songs have all the tags they should have. However, the remaining playlist is still one we are very happy with. 
    
    Silent Night - Pentatonix
    Hark! The Herald Angels Sing - Pentatonix
    That's Christmas to Me - Pentatonix
    Sleigh Ride - Pentatonix
    Dance of the Sugar Plum Fairy - Pentatonix
    It's Beginning To Look A Lot Like Christmas - Michael Bublé
    All I Want for Christmas Is You - Mariah Carey
    White Christmas (Duet With Shania Twain) - Michael Bublé	
    Holly Jolly Christmas - Michael Bublé	
    I'll Be Home For Christmas	- Michael Bublé	
    Have Yourself A Merry Little Christmas - Michael Bublé	
    All I Want For Christmas Is You	- Michael Bublé	
    Hallelujah - Pentatonix	
    Santa Claus Is Coming To Town - Michael Bublé	
    Jingle Bells (feat. The Puppini Sisters) - Michael Bublé	
    
