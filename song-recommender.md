


```python
import song_recommender as SR
```




```python
import pandas as pd
import numpy as np
import load_data
import warnings

#Song Recommendation system 

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




```python
database, unique_tracks = SR.load()
```


    Loading folder 3...
    Folder 3 is finished.
    Database is loaded, song recommender ready to use.




```python
#If song is not in the database
SR.recommend_me_some_songs(database, unique_tracks,'HI.')
```


    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:46: UserWarning: Sorry, the song cannot be found in this database.
      warnings.warn('Sorry, the song cannot be found in this database.')




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




```python
playlist_christmas = SR.recommend_me_some_songs(database, unique_tracks, 'Silent Night')
```


    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:48: UserWarning: More than one artist with the song name. Please specify the artist name. Your options are: ['Michael Bublé' 'Pentatonix' 'Mariah Carey' 'Justin Bieber' 'Bing Crosby'
     'Josh Groban' 'Kelly Clarkson' 'The Temptations' 'Boyz II Men'
     'Andrea Bocelli' 'Carpenters' 'The Piano Guys' 'Johnny Cash'
     'Sarah McLachlan' 'Casting Crowns' 'Elvis Presley' 'Amy Grant'
     'Stevie Nicks' 'Sufjan Stevens' 'Idina Menzel' 'Lauren Daigle'
     'Phil Wickham' 'Straight No Chaser' 'MercyMe' 'Martina McBride'
     "Destiny's Child" 'CeeLo Green' 'Home Free' 'Harry Connick, Jr.'
     'David Archuleta' 'Shane & Shane' 'Brett Eldredge' 'Celtic Woman'
     "Sinéad O'Connor" 'Gloriana' 'Kenny G' 'Priscilla Ahn'
     'David Crowder Band' 'Dean Martin' 'George Strait'
     'Drew Holcomb & The Neighbors' 'Kenny Chesney' 'Zach Gill' 'Third Day'
     'Chris Young' 'John Denver' 'Aaron Neville' 'Lennon & Maisy'
     'Citizens & Saints' 'Brad Paisley' 'Sidewalk Prophets' 'Jordan Smith'
     'Walk Off the Earth' 'Dave Brubeck' 'Sleeping At Last' 'Burl Ives'
     'Nat King Cole' 'Michael W. Smith' 'Sugarland' "Seamus O' Malley"
     'Leona Lewis' 'Big Daddy Weave' 'BYU Vocal Point' 'Low' 'Brandon Heath'
     'Weezer' 'Wilson Phillips' 'Sharon Jones & The Dap-Kings'
     'The Christmas Cello' 'Franz Xaver Gruber' 'JJ Heller' 'Aly & AJ' 'Jewel'
     'Dinah Washington' 'Clay Aiken' '98º' 'Rascal Flatts' 'Taylor Davis'
     "International Children's Choir" 'Reba McEntire' 'Tracy Silverman'
     'Natalie Cole' 'Holiday Jazz' 'Lincoln Brewster' 'Josh Garrels'
     'Sixpence None The Richer' 'Mindy Gledhill' 'Seabird' 'Forevermore'
     'John Travolta' 'Wynton Marsalis' 'Hillsong Worship' 'Rod Stewart'
     'Joshua Hyslop' 'Sara Groves' 'Trace Bundy' 'Gungor' 'Hawk Nelson'
     'The Platters' 'The Robertsons' 'Taylor Swift' 'Sugar & The Hi Lows'
     'Anne Murray' 'DJ Okawari' 'House of Heroes' 'Phil Spector' 'Everfound'
     'The Flaming Lips' 'Kelly Price' 'Branches' 'Jadon Lavik' 'Kris Baines'
     'Steve Erquiaga' 'Andrew Ripp'
     'Christmas Hits,Christmas Songs & Christmas' 'Alexi Murdoch'
     'Marie Miller' 'Jimmy Sommers' 'Jon Schmidt' 'Pink Martini' 'Ben Rector'
     'Jim Brickman' 'Jacques Stotzem' 'Fluttershy' 'Jason Wright'
     'Lindsey Stirling' 'Cloverton' 'Matthew West' 'Elizabeth Mitchell'
     'Sabine Moncler' 'Various Artists' 'Canadian Brass & Eric Robertson'
     'India.Arie' 'Gloria Estefan' 'Boney James' 'Steven C' 'Bodeans'
     'Billy Gilman' 'Mandisa' 'Christmas Hits' 'Kenny Burrell'
     'Vanessa Williams' 'Crossroads' 'Phil Driscoll'
     'Tony Bennett with The London Symphony Orchestra and Chorus' 'Il Divo'
     'Neil Diamond' 'Big Maybelle' 'Chuck Billy' 'Aretha Franklin' 'Selah'
     'Margo Rey' 'Eclipse' 'Andy Williams' 'Toby Keith' 'Michael Bolton'
     'OMP Allstars' 'Ambrosian Singers' 'David Ian' 'Violin Collections'
     'Andy Zipf' 'Brenda Lee' 'Pablo Black' 'Brad Mehldau' 'Steve Petrunak'
     'Perry Como' 'Peter Mayer' 'Nelson Eddy' 'Rescue' 'Sarah Darling'
     'Hayley Westenra' 'Tamar Braxton' 'The Blind Boys Of Alabama'
     'Stopford A. Brooke' 'The Seldom Scene' 'Tyrone Wells' 'Frank Sinatra'
     'Tonic Sol-Fa' 'Bros' 'Tennessee Ernie Ford' 'Lifehouse' 'Edwin McCain'
     'Dana Cunningham' 'Keyshia Cole' 'Pamela Stone' 'Beta Radio'
     'The Digital Age' 'Svrcina' 'Sara Ramirez' 'Ryan Innes' 'Peter Hollens'
     'William Basinski' 'Richard Westenburg' 'Mark Kozelek' 'Marc Martel'
     'Alabama' 'Mike Crawford and His Secret Siblings' 'Kris Allen'
     'Samuel Ljungblahd' "Don't Sigh Daisy" 'Maggie Sansone' 'Trace Adkins'
     'Kim Walker-Smith' 'Mindy Smith' 'Erasure' 'Krucifix Klan' 'Sofia Carson'
     'Catherine Lucille' 'Jim Reeves' 'Linda Eder' 'Penny and Sparrow'
     'Folk Angel' 'New York Jazz Trio' 'Takénobu' 'Babyface' 'Jason Manns'
     'Rebecca Roubion' 'Kidzone' '6 Feet Deep' 'David Osborne' 'Adam Levine'
     'Musica Sacra' 'Jessica Sanchez' 'Michael Bubble' 'Rosemary Clooney'
     'Bluegrass Jamboree' 'Hope' 'Anna Gilbert' 'Sam Tsui' 'David Lanz'
     'Kirk Franklin' 'London Philharmonic Orchestra' 'Julian Ovenden'
     'Stephanie Mills' 'Plus One' 'Justin Rizzo' 'George Jones' 'Les Paul'
     "Oxford St. Peter's Choir" 'Olivia Newton-John' 'Breath of Aire'
     'The Drifters' 'Michelle Beauchesne' 'Irish Orchestral Players & Chorus'
     'Chicago' 'Philip Wesley' 'Al Green' 'Finding Favour' 'The Clark Sisters'
     'Acappella' 'The Impressions' 'David Grisman' 'Future Of Forestry'
     'Page CXVI' 'Daves Highway' 'All-4-One' 'Conrad Askland' 'Herb Ohta, Jr.'
     'Oscar Peterson' 'VeggieTales' 'Vinnie Zummo' 'Warren Hill'
     'Sarah Brightman' 'Barbara Higbie' 'Richard Elliot' 'Bon Jovi'
     'Dirk Powell' 'The Laurie Berkner Band' 'Bossa Nova Nouveau'
     'Joan Jett & The Blackhearts' 'Bright Eyes' 'Marston Smith'
     'William Joseph' 'Annie Lennox' 'Carolee Mayne' 'Tony Sandate'
     'Massed Choir' 'Great White' 'Martin Sexton' 'Il Volo' 'Renato Rascel'
     'Atlantic Five Jazz Band' 'Sara Evans' 'Asleep At The Wheel'
     'Cyndi Lauper' 'Merle Haggard' 'New York Jazz Lounge' 'Elenowen'
     'Joe Diffie' 'Roger Miller' 'Al Jarreau' 'Phil Keaggy' 'Mahalia Jackson'
     'Rocky Votolato' 'David Phelps' 'Mandy Joy Miller' 'Freak Nasty'
     'Kevin Max' 'The Polyphonic Spree' 'The Traditional' 'Greg Howlett'
     'Bosque Brown' 'Downhere' 'Susan Boyle' 'Peter Pupping' 'Blake Shelton'
     'Charley Pride' 'Kate Nash' 'Kermit Ruffins' 'Take 6' 'Harp'
     'Eddie Wakes' 'John Sheehan' 'Ledisi' 'Cascada' 'The Irish Tenors'
     'Meaghan Smith' 'Stephen Petrunak' "Booker T. & the M.G.'s" 'Steve Rosen'
     'Béla Fleck and the Flecktones']
      warnings.warn(f"More than one artist with the song name. Please specify the artist name. Your options are: {unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()}")




```python
playlist_christmas = SR.recommend_me_some_songs(database, unique_tracks, 'Silent Night', 'Pentatonix')
```


    Mary, Did You Know? - Pentatonix
    Silent Night - Pentatonix
    Winter Wonderland / Don't Worry Be Happy - Pentatonix
    Hark! The Herald Angels Sing - Pentatonix
    It's the Most Wonderful Time of the Year - Pentatonix
    That's Christmas to Me - Pentatonix
    Sleigh Ride - Pentatonix
    Dance of the Sugar Plum Fairy - Pentatonix
    Santa Claus is Coming to Town - Pentatonix
    White Winter Hymnal - Pentatonix
    It's Beginning To Look A Lot Like Christmas - Michael Bublé
    All I Want for Christmas Is You - Mariah Carey
    Let It Go - Bonus Track - Pentatonix
    Joy to the World - Pentatonix
    White Christmas (Duet With Shania Twain) - Michael Bublé
    Holly Jolly Christmas - Michael Bublé
    I'll Be Home For Christmas - Michael Bublé
    The First Noel - Pentatonix
    Have Yourself A Merry Little Christmas - Michael Bublé
    Have Yourself a Merry Little Christmas - Pentatonix
    All I Want For Christmas Is You - Michael Bublé
    Hallelujah - Pentatonix
    Santa Claus Is Coming To Town - Michael Bublé
    Jingle Bells (feat. The Puppini Sisters) - Michael Bublé
    Carol of the Bells - Pentatonix




```python
playlist_country = SR.recommend_me_some_songs(database, unique_tracks, 'Rain Is a Good Thing')
```


    /Users/yang_helen/Documents/GitHub/spotify/song_recommender.py:48: UserWarning: More than one artist with the song name. Please specify the artist name. Your options are: ['Luke Bryan' 'Boogie Boots']
      warnings.warn(f"More than one artist with the song name. Please specify the artist name. Your options are: {unique_tracks[unique_tracks.track_name == song_name].artist_name.unique()}")




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




```python

```

