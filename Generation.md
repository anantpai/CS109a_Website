---
title: Generation
notebook: Generation.ipynb
nav_include: 4
---


#### Playlist Generation

Now that we have all of these insights on what fuels a successful playlist, we can use those to create our own 'hopefully viral' playlist. We will do so in a four step process:

1) We will ask the playlist curator for a genre of which to create a playlist from and we will subset our songs data into only songs from said genre.

2) We will give each individual song in this arsenal a 'score' for how well it matches these attributes.

3) We will collect the songs with the highest set of scores into a dataframe and have them as a pool to draw from.

4) We will randomly select songs from this pool, keeping in mind some of the genre variables we introduced and their performances.

Our scoring metric itself is affected, as mentioned, by the attributes from our final models. Those attributes are average song popularity, the top song popularity, the percentage of explicit songs, the average artists followers, the number of songs, and numerous interaction terms between genres and artists. In order to consistently make playlists for a given genre, however, only a few of these factors become significant. Since we are creating playlists for one genre at a time, the interaction terms become only relevant in their given genres. For demonstration purposes we will be creating an R&B playlist (so here the only valid interaction term is that with The Weeknd), as well as a Jazz playlist (no interactions), and a pop playlist (including an interaction with Sia). Additionally, the number of songs variable becomes less crucial here since we want to consistently generate playlists with the same number of songs. We'll use 50 since that's where the benefit tops out and is customary for Spotify curated playlists.





    /Users/Trevor/anaconda/lib/python3.6/site-packages/statsmodels/compat/pandas.py:56: FutureWarning: The pandas.core.datetools module is deprecated and will be removed in a future version. Please use the pandas.tseries module instead.
      from pandas.core import datetools









<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>playlist_id</th>
      <th>playlist_name</th>
      <th>followers</th>
      <th>song_name</th>
      <th>number_of_artists</th>
      <th>artist_name</th>
      <th>artist_id</th>
      <th>popularity</th>
      <th>track_number</th>
      <th>explicit</th>
      <th>duration_ms</th>
      <th>available_markets</th>
      <th>delete</th>
      <th>artist_popularity</th>
      <th>artist_followers</th>
      <th>artist_genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1686</th>
      <td>37i9dQZF1DXcBWIGoYBM5M</td>
      <td>Today's Top Hits</td>
      <td>18123888.0</td>
      <td>Plain Jane</td>
      <td>1</td>
      <td>A$AP Ferg</td>
      <td>5dHt1vcEm9qb8fCyLcB3HL</td>
      <td>90</td>
      <td>8</td>
      <td>1</td>
      <td>173600</td>
      <td>['AD', 'AR', 'AT', 'AU', 'BE', 'BG', 'BO', 'BR...</td>
      <td>NaN</td>
      <td>87</td>
      <td>821565</td>
      <td>['dwn trap', 'hip hop', 'indie r&amp;b', 'pop rap'...</td>
    </tr>
    <tr>
      <th>1687</th>
      <td>37i9dQZF1DX0XUsuxWHRQd</td>
      <td>RapCaviar</td>
      <td>8318573.0</td>
      <td>Plain Jane</td>
      <td>1</td>
      <td>A$AP Ferg</td>
      <td>5dHt1vcEm9qb8fCyLcB3HL</td>
      <td>90</td>
      <td>8</td>
      <td>1</td>
      <td>173600</td>
      <td>['AD', 'AR', 'AT', 'AU', 'BE', 'BG', 'BO', 'BR...</td>
      <td>NaN</td>
      <td>87</td>
      <td>821565</td>
      <td>['dwn trap', 'hip hop', 'indie r&amp;b', 'pop rap'...</td>
    </tr>
    <tr>
      <th>1688</th>
      <td>37i9dQZF1DWVstK6FYh8Nw</td>
      <td>This Is: Future</td>
      <td>267999.0</td>
      <td>New Level REMIX</td>
      <td>4</td>
      <td>A$AP Ferg</td>
      <td>5dHt1vcEm9qb8fCyLcB3HL</td>
      <td>58</td>
      <td>1</td>
      <td>1</td>
      <td>251194</td>
      <td>['AD', 'AR', 'AT', 'AU', 'BE', 'BG', 'BO', 'BR...</td>
      <td>NaN</td>
      <td>87</td>
      <td>821565</td>
      <td>['dwn trap', 'hip hop', 'indie r&amp;b', 'pop rap'...</td>
    </tr>
    <tr>
      <th>1689</th>
      <td>37i9dQZF1DX2A29LI7xHn1</td>
      <td>Signed XOXO</td>
      <td>1302273.0</td>
      <td>Plain Jane</td>
      <td>1</td>
      <td>A$AP Ferg</td>
      <td>5dHt1vcEm9qb8fCyLcB3HL</td>
      <td>90</td>
      <td>8</td>
      <td>1</td>
      <td>173600</td>
      <td>['AD', 'AR', 'AT', 'AU', 'BE', 'BG', 'BO', 'BR...</td>
      <td>NaN</td>
      <td>87</td>
      <td>821565</td>
      <td>['dwn trap', 'hip hop', 'indie r&amp;b', 'pop rap'...</td>
    </tr>
    <tr>
      <th>1690</th>
      <td>37i9dQZF1DWY4xHQp97fN6</td>
      <td>Get Turnt</td>
      <td>3263165.0</td>
      <td>Plain Jane</td>
      <td>1</td>
      <td>A$AP Ferg</td>
      <td>5dHt1vcEm9qb8fCyLcB3HL</td>
      <td>90</td>
      <td>8</td>
      <td>1</td>
      <td>173600</td>
      <td>['AD', 'AR', 'AT', 'AU', 'BE', 'BG', 'BO', 'BR...</td>
      <td>NaN</td>
      <td>87</td>
      <td>821565</td>
      <td>['dwn trap', 'hip hop', 'indie r&amp;b', 'pop rap'...</td>
    </tr>
  </tbody>
</table>
</div>



We wrote a function that would give each song a score-- we provide the code to this portion of the project only because it gives some insight into how our generator works.



```python
def song_score(artist, song_pop, explicit, artist_followers):
    score = 1
    score = score*1.2*song_pop
    score = score*.05*artist_followers
    if "The Weeknd" in artist and 'r&b' == desired_genre:
        score = score*1.02
    if "Sia" in artist and 'pop' == desired_genre:
        score = score*1.04
    if song_pop > 95:
        score = score*1.09
    if explicit == 1:
        score = score*1.007
    return(score)
```


As aforementioned, we can score our R&B songs weighted the variables accordingly (based on the weighted outputs from our random forest model) - including the bonus on songs from the Weeknd. Below you can see a small subset of what this scoring looks like.








<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>song_name</th>
      <th>artist_name</th>
      <th>score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Plain Jane</td>
      <td>A$AP Ferg</td>
      <td>4.46751e+06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>New Level REMIX</td>
      <td>A$AP Ferg</td>
      <td>2.87906e+06</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Work REMIX</td>
      <td>A$AP Ferg</td>
      <td>3.8222e+06</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rubber Band Man</td>
      <td>A$AP Ferg</td>
      <td>3.67328e+06</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Olympian</td>
      <td>A$AP Ferg</td>
      <td>3.12725e+06</td>
    </tr>
  </tbody>
</table>
</div>



Our R&B playlist then is as follows:








<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>song_name</th>
      <th>artist_name</th>
      <th>score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>38</th>
      <td>Don't Wake Me Up</td>
      <td>Chris Brown</td>
      <td>1.97947e+07</td>
    </tr>
    <tr>
      <th>1384</th>
      <td>I Be Puttin' On - feat. Wiz Khalifa, French Mo...</td>
      <td>Wale</td>
      <td>2.2093e+06</td>
    </tr>
    <tr>
      <th>552</th>
      <td>Distance And Time</td>
      <td>Alicia Keys</td>
      <td>9.51984e+06</td>
    </tr>
    <tr>
      <th>202</th>
      <td>I Luv This Shit - Remix</td>
      <td>August Alsina</td>
      <td>2.61306e+06</td>
    </tr>
    <tr>
      <th>979</th>
      <td>No Limit</td>
      <td>Usher</td>
      <td>1.69494e+07</td>
    </tr>
    <tr>
      <th>279</th>
      <td>Make Me Like You</td>
      <td>Gwen Stefani</td>
      <td>4.11886e+06</td>
    </tr>
    <tr>
      <th>602</th>
      <td>Sin City</td>
      <td>John Legend</td>
      <td>6.99978e+06</td>
    </tr>
    <tr>
      <th>615</th>
      <td>#thatPOWER</td>
      <td>will.i.am</td>
      <td>5.15373e+06</td>
    </tr>
    <tr>
      <th>1038</th>
      <td>Beautiful</td>
      <td>Christina Aguilera</td>
      <td>8.38635e+06</td>
    </tr>
    <tr>
      <th>1488</th>
      <td>The First Noel</td>
      <td>Mary J. Blige</td>
      <td>2.18267e+06</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Sure Thing</td>
      <td>Miguel</td>
      <td>5.22818e+06</td>
    </tr>
    <tr>
      <th>965</th>
      <td>A Moment Like This</td>
      <td>Kelly Clarkson</td>
      <td>4.6232e+06</td>
    </tr>
    <tr>
      <th>1375</th>
      <td>Bad - Remix feat. Rihanna</td>
      <td>Wale</td>
      <td>3.40864e+06</td>
    </tr>
    <tr>
      <th>1019</th>
      <td>Drink You Away</td>
      <td>Justin Timberlake</td>
      <td>1.69851e+07</td>
    </tr>
    <tr>
      <th>142</th>
      <td>Dead Wrong (feat. Ty Dolla $ign)</td>
      <td>Trey Songz</td>
      <td>7.6262e+06</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Arch &amp; Point</td>
      <td>Miguel</td>
      <td>2.00533e+06</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Nasty (Who Dat)</td>
      <td>A$AP Ferg</td>
      <td>3.42509e+06</td>
    </tr>
    <tr>
      <th>424</th>
      <td>Touch My Body</td>
      <td>Mariah Carey</td>
      <td>6.74932e+06</td>
    </tr>
    <tr>
      <th>1881</th>
      <td>Diggin' on You</td>
      <td>TLC</td>
      <td>2.31339e+06</td>
    </tr>
    <tr>
      <th>207</th>
      <td>Better Man (feat. Rick Ross)</td>
      <td>PARTYNEXTDOOR</td>
      <td>3.67622e+06</td>
    </tr>
    <tr>
      <th>947</th>
      <td>Love On The Brain - Don Diablo Remix</td>
      <td>Rihanna</td>
      <td>4.56485e+07</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Doe-Active</td>
      <td>A$AP Ferg</td>
      <td>2.28339e+06</td>
    </tr>
    <tr>
      <th>520</th>
      <td>Out Of My Mind (feat. Nicki Minaj) - feat. Nic...</td>
      <td>B.o.B</td>
      <td>3.89598e+06</td>
    </tr>
    <tr>
      <th>948</th>
      <td>Sex With Me</td>
      <td>Rihanna</td>
      <td>6.24174e+07</td>
    </tr>
    <tr>
      <th>1047</th>
      <td>Oh Mother</td>
      <td>Christina Aguilera</td>
      <td>4.93315e+06</td>
    </tr>
    <tr>
      <th>1269</th>
      <td>I. Pink Toes</td>
      <td>Childish Gambino</td>
      <td>8.1217e+06</td>
    </tr>
    <tr>
      <th>586</th>
      <td>So High - Single Version</td>
      <td>John Legend</td>
      <td>8.05698e+06</td>
    </tr>
    <tr>
      <th>1005</th>
      <td>Row The Body (feat. French Montana)</td>
      <td>Taio Cruz</td>
      <td>3.87516e+06</td>
    </tr>
    <tr>
      <th>1492</th>
      <td>I'm In Love</td>
      <td>Mary J. Blige</td>
      <td>2.29755e+06</td>
    </tr>
    <tr>
      <th>267</th>
      <td>All I Have</td>
      <td>Jennifer Lopez</td>
      <td>8.43617e+06</td>
    </tr>
    <tr>
      <th>81</th>
      <td>The Thrill - Spotify Sessions</td>
      <td>Miguel</td>
      <td>2.50666e+06</td>
    </tr>
    <tr>
      <th>787</th>
      <td>Return to Air</td>
      <td>Bonobo</td>
      <td>1.96448e+06</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Slow Motion</td>
      <td>Trey Songz</td>
      <td>1.02378e+07</td>
    </tr>
    <tr>
      <th>1483</th>
      <td>Not Gon' Cry</td>
      <td>Mary J. Blige</td>
      <td>2.75706e+06</td>
    </tr>
    <tr>
      <th>1411</th>
      <td>Dead And Gone (feat. Justin Timberlake)</td>
      <td>T.I.</td>
      <td>7.48906e+06</td>
    </tr>
    <tr>
      <th>1245</th>
      <td>Blow</td>
      <td>Kesha</td>
      <td>8.67446e+06</td>
    </tr>
    <tr>
      <th>91</th>
      <td>All I Want Is You</td>
      <td>Miguel</td>
      <td>4.51199e+06</td>
    </tr>
    <tr>
      <th>1000</th>
      <td>Love in This Club</td>
      <td>Usher</td>
      <td>1.7646e+07</td>
    </tr>
    <tr>
      <th>1463</th>
      <td>2 Fine</td>
      <td>T-Pain</td>
      <td>3.64477e+06</td>
    </tr>
    <tr>
      <th>1040</th>
      <td>Lady Marmalade - From "Moulin Rouge" Soundtrack</td>
      <td>Christina Aguilera</td>
      <td>7.89304e+06</td>
    </tr>
    <tr>
      <th>1915</th>
      <td>Cherish the Day</td>
      <td>Sade</td>
      <td>2.27073e+06</td>
    </tr>
    <tr>
      <th>154</th>
      <td>B.E.D.</td>
      <td>Jacquees</td>
      <td>2.23687e+06</td>
    </tr>
    <tr>
      <th>854</th>
      <td>(You Drive Me) Crazy - The Stop Remix!</td>
      <td>Britney Spears</td>
      <td>9.05175e+06</td>
    </tr>
    <tr>
      <th>662</th>
      <td>Partition</td>
      <td>BeyoncÌÄå©</td>
      <td>3.7594e+07</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Grass Ain't Greener</td>
      <td>Chris Brown</td>
      <td>2.24645e+07</td>
    </tr>
    <tr>
      <th>950</th>
      <td>Lost In Paradise</td>
      <td>Rihanna</td>
      <td>4.09905e+07</td>
    </tr>
    <tr>
      <th>438</th>
      <td>Lose Control (feat. Ciara &amp; Fat Man Scoop)</td>
      <td>Missy Elliott</td>
      <td>2.31457e+06</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Shabba</td>
      <td>A$AP Ferg</td>
      <td>3.52437e+06</td>
    </tr>
    <tr>
      <th>653</th>
      <td>Check On It</td>
      <td>BeyoncÌÄå©</td>
      <td>2.81431e+07</td>
    </tr>
    <tr>
      <th>1595</th>
      <td>You</td>
      <td>Lloyd</td>
      <td>2.12753e+06</td>
    </tr>
  </tbody>
</table>
</div>



We can repeat the same process for Jazz and Pop. Our Jazz playlist is as follows:








<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>song_name</th>
      <th>artist_name</th>
      <th>score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2327</th>
      <td>The Red One</td>
      <td>John Scofield</td>
      <td>136927</td>
    </tr>
    <tr>
      <th>2414</th>
      <td>This Land Is Your Land</td>
      <td>Sharon Jones &amp; The Dap-Kings</td>
      <td>348156</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Let It Be</td>
      <td>Aretha Franklin</td>
      <td>2.34095e+06</td>
    </tr>
    <tr>
      <th>150</th>
      <td>The Bare Necessities</td>
      <td>Tony Bennett</td>
      <td>469550</td>
    </tr>
    <tr>
      <th>234</th>
      <td>White Christmas</td>
      <td>Bing Crosby</td>
      <td>138672</td>
    </tr>
    <tr>
      <th>2613</th>
      <td>Perhaps</td>
      <td>Oscar D'LeÌÄå_n</td>
      <td>301563</td>
    </tr>
    <tr>
      <th>1987</th>
      <td>The Fool</td>
      <td>Fleshgod Apocalypse</td>
      <td>94817.2</td>
    </tr>
    <tr>
      <th>279</th>
      <td>Work Song</td>
      <td>Nina Simone</td>
      <td>1.90408e+06</td>
    </tr>
    <tr>
      <th>365</th>
      <td>Eyes Of Man</td>
      <td>Chuck Berry</td>
      <td>918435</td>
    </tr>
    <tr>
      <th>228</th>
      <td>Chatanooga Choo Choo</td>
      <td>Glenn Miller</td>
      <td>221286</td>
    </tr>
    <tr>
      <th>344</th>
      <td>Know You</td>
      <td>Bonobo</td>
      <td>1.92439e+06</td>
    </tr>
    <tr>
      <th>259</th>
      <td>Li'l Liza Jane - Live Version - Newport Jazz F...</td>
      <td>Nina Simone</td>
      <td>1.45312e+06</td>
    </tr>
    <tr>
      <th>2525</th>
      <td>Back to Nature - Edit</td>
      <td>Nightmares On Wax</td>
      <td>375818</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Love And Happiness</td>
      <td>Al Green</td>
      <td>405637</td>
    </tr>
    <tr>
      <th>346</th>
      <td>Sleepy Seven</td>
      <td>Bonobo</td>
      <td>1.60366e+06</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Ain't No Way</td>
      <td>Aretha Franklin</td>
      <td>2.29006e+06</td>
    </tr>
    <tr>
      <th>401</th>
      <td>You Can't Catch Me - Single Version</td>
      <td>Chuck Berry</td>
      <td>675320</td>
    </tr>
    <tr>
      <th>2235</th>
      <td>Maiden Voyage / Everything In Its Right Place</td>
      <td>Robert Glasper</td>
      <td>177583</td>
    </tr>
    <tr>
      <th>475</th>
      <td>The Beat Of Black Wings</td>
      <td>Joni Mitchell</td>
      <td>398950</td>
    </tr>
    <tr>
      <th>363</th>
      <td>Jamaica Moon</td>
      <td>Chuck Berry</td>
      <td>972460</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Call Me Irresponsible</td>
      <td>Tony Bennett</td>
      <td>516505</td>
    </tr>
    <tr>
      <th>112</th>
      <td>The Way You Look Tonight</td>
      <td>Tony Bennett</td>
      <td>594763</td>
    </tr>
    <tr>
      <th>3226</th>
      <td>Vem vet?</td>
      <td>Lisa Ekdahl</td>
      <td>187146</td>
    </tr>
    <tr>
      <th>471</th>
      <td>Come In From The Cold</td>
      <td>Joni Mitchell</td>
      <td>468332</td>
    </tr>
    <tr>
      <th>162</th>
      <td>My Favourite Things</td>
      <td>Tony Bennett</td>
      <td>704325</td>
    </tr>
    <tr>
      <th>268</th>
      <td>Sinnerman - Live In New York/1965</td>
      <td>Nina Simone</td>
      <td>2.30494e+06</td>
    </tr>
    <tr>
      <th>2507</th>
      <td>Black Baby</td>
      <td>Kruder &amp; Dorfmeister</td>
      <td>118841</td>
    </tr>
    <tr>
      <th>2124</th>
      <td>For Better or Worse</td>
      <td>Keb' Mo'</td>
      <td>169678</td>
    </tr>
    <tr>
      <th>278</th>
      <td>Save Me</td>
      <td>Nina Simone</td>
      <td>2.10451e+06</td>
    </tr>
    <tr>
      <th>311</th>
      <td>Samurai</td>
      <td>Bonobo</td>
      <td>2.08476e+06</td>
    </tr>
    <tr>
      <th>526</th>
      <td>Drift Away</td>
      <td>Ike &amp; Tina Turner</td>
      <td>121994</td>
    </tr>
    <tr>
      <th>489</th>
      <td>Shadows And Light</td>
      <td>Joni Mitchell</td>
      <td>520369</td>
    </tr>
    <tr>
      <th>451</th>
      <td>A Case Of You</td>
      <td>Joni Mitchell</td>
      <td>1.05808e+06</td>
    </tr>
    <tr>
      <th>2909</th>
      <td>Where We Used To Live</td>
      <td>EsbjÌÄå¦rn Svensson Trio</td>
      <td>160449</td>
    </tr>
    <tr>
      <th>292</th>
      <td>Four Women</td>
      <td>Nina Simone</td>
      <td>1.95419e+06</td>
    </tr>
    <tr>
      <th>191</th>
      <td>A Rockin' Good Way (To Mess Around And Fall In...</td>
      <td>Dinah Washington</td>
      <td>201038</td>
    </tr>
    <tr>
      <th>198</th>
      <td>April In Paris - Alternate Take</td>
      <td>Count Basie</td>
      <td>126229</td>
    </tr>
    <tr>
      <th>334</th>
      <td>Recurring</td>
      <td>Bonobo</td>
      <td>1.56357e+06</td>
    </tr>
    <tr>
      <th>595</th>
      <td>So In Love - feat. Anthony Hamilton</td>
      <td>Jill Scott</td>
      <td>1.09961e+06</td>
    </tr>
    <tr>
      <th>188</th>
      <td>Drinking Again</td>
      <td>Dinah Washington</td>
      <td>194553</td>
    </tr>
    <tr>
      <th>616</th>
      <td>How Sweet It Is (To Be Loved By You)</td>
      <td>Michael McDonald</td>
      <td>137178</td>
    </tr>
    <tr>
      <th>520</th>
      <td>I Am A Motherless Child</td>
      <td>Ike &amp; Tina Turner</td>
      <td>107207</td>
    </tr>
    <tr>
      <th>295</th>
      <td>Nobody Knows You When You're Down And Out - Li...</td>
      <td>Nina Simone</td>
      <td>1.90408e+06</td>
    </tr>
    <tr>
      <th>2485</th>
      <td>Music Takes Me Up</td>
      <td>Mr. Scruff</td>
      <td>196342</td>
    </tr>
    <tr>
      <th>483</th>
      <td>The Hissing Of Summer Lawns</td>
      <td>Joni Mitchell</td>
      <td>711171</td>
    </tr>
    <tr>
      <th>144</th>
      <td>It Amazes Me</td>
      <td>Tony Bennett</td>
      <td>453898</td>
    </tr>
    <tr>
      <th>510</th>
      <td>Fat Mama</td>
      <td>Herbie Hancock</td>
      <td>444177</td>
    </tr>
    <tr>
      <th>3161</th>
      <td>World Looking In</td>
      <td>Morcheeba</td>
      <td>742280</td>
    </tr>
    <tr>
      <th>2257</th>
      <td>Learn To Love</td>
      <td>Harry Connick, Jr.</td>
      <td>93716.2</td>
    </tr>
    <tr>
      <th>219</th>
      <td>My Little Brown Book</td>
      <td>Duke Ellington</td>
      <td>166304</td>
    </tr>
  </tbody>
</table>
</div>



Our Pop playlist is as follows:








<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>song_name</th>
      <th>artist_name</th>
      <th>score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6155</th>
      <td>Welcome to the Family</td>
      <td>Avenged Sevenfold</td>
      <td>7.08221e+06</td>
    </tr>
    <tr>
      <th>963</th>
      <td>Nothing Without You</td>
      <td>The Weeknd</td>
      <td>3.33288e+07</td>
    </tr>
    <tr>
      <th>7219</th>
      <td>Unbreakable</td>
      <td>Of Mice &amp; Men</td>
      <td>2.03897e+06</td>
    </tr>
    <tr>
      <th>5134</th>
      <td>Dirty Laundry</td>
      <td>Nickelback</td>
      <td>7.09069e+06</td>
    </tr>
    <tr>
      <th>8850</th>
      <td>U + Me (Love Lesson)</td>
      <td>Mary J. Blige</td>
      <td>3.10169e+06</td>
    </tr>
    <tr>
      <th>7117</th>
      <td>Painting Flowers</td>
      <td>All Time Low</td>
      <td>3.44235e+06</td>
    </tr>
    <tr>
      <th>3324</th>
      <td>Young, Wild &amp; Free (feat. Bruno Mars) - feat. ...</td>
      <td>Snoop Dogg</td>
      <td>1.12672e+07</td>
    </tr>
    <tr>
      <th>599</th>
      <td>New Flame</td>
      <td>Chris Brown</td>
      <td>2.18317e+07</td>
    </tr>
    <tr>
      <th>8326</th>
      <td>Ain't Nobody Takin My Baby</td>
      <td>Russ</td>
      <td>4.25502e+06</td>
    </tr>
    <tr>
      <th>3266</th>
      <td>Untitled</td>
      <td>blink-182</td>
      <td>7.58641e+06</td>
    </tr>
    <tr>
      <th>887</th>
      <td>Reconsider (Jamie xx Edit)</td>
      <td>The xx</td>
      <td>8.00504e+06</td>
    </tr>
    <tr>
      <th>7868</th>
      <td>Giving Up The Gun</td>
      <td>Vampire Weekend</td>
      <td>2.8704e+06</td>
    </tr>
    <tr>
      <th>417</th>
      <td>Back from the Edge</td>
      <td>James Arthur</td>
      <td>4.97943e+06</td>
    </tr>
    <tr>
      <th>879</th>
      <td>Dangerous</td>
      <td>The xx</td>
      <td>1.00455e+07</td>
    </tr>
    <tr>
      <th>5195</th>
      <td>Magic</td>
      <td>Coldplay</td>
      <td>3.98666e+07</td>
    </tr>
    <tr>
      <th>1015</th>
      <td>Break From Love</td>
      <td>Trey Songz</td>
      <td>8.27441e+06</td>
    </tr>
    <tr>
      <th>1494</th>
      <td>Fire and the Flood - Acoustic</td>
      <td>Vance Joy</td>
      <td>2.63853e+06</td>
    </tr>
    <tr>
      <th>1540</th>
      <td>Echoes Of Love</td>
      <td>Jesse &amp; Joy</td>
      <td>4.21527e+06</td>
    </tr>
    <tr>
      <th>9128</th>
      <td>Exhale - Shoop Shoop</td>
      <td>Whitney Houston</td>
      <td>4.86459e+06</td>
    </tr>
    <tr>
      <th>4404</th>
      <td>I Hope You Find It</td>
      <td>Miley Cyrus</td>
      <td>9.77481e+06</td>
    </tr>
    <tr>
      <th>2914</th>
      <td>Don't Panic</td>
      <td>French Montana</td>
      <td>3.24564e+06</td>
    </tr>
    <tr>
      <th>252</th>
      <td>Drowning Shadows</td>
      <td>Sam Smith</td>
      <td>2.78856e+06</td>
    </tr>
    <tr>
      <th>1798</th>
      <td>Such Great Heights</td>
      <td>Iron &amp; Wine</td>
      <td>1.87611e+06</td>
    </tr>
    <tr>
      <th>9161</th>
      <td>Cleva</td>
      <td>Erykah Badu</td>
      <td>2.03986e+06</td>
    </tr>
    <tr>
      <th>8571</th>
      <td>Family Business</td>
      <td>Kanye West</td>
      <td>1.73254e+07</td>
    </tr>
    <tr>
      <th>3314</th>
      <td>Nobody Told Me</td>
      <td>B.o.B</td>
      <td>2.43499e+06</td>
    </tr>
    <tr>
      <th>6200</th>
      <td>Beautiful War</td>
      <td>Kings of Leon</td>
      <td>8.88857e+06</td>
    </tr>
    <tr>
      <th>2590</th>
      <td>Drive</td>
      <td>R.E.M.</td>
      <td>2.35473e+06</td>
    </tr>
    <tr>
      <th>8274</th>
      <td>Terrified</td>
      <td>Childish Gambino</td>
      <td>8.70182e+06</td>
    </tr>
    <tr>
      <th>3897</th>
      <td>Scared of Lonely</td>
      <td>BeyoncÌÄå©</td>
      <td>2.92918e+07</td>
    </tr>
    <tr>
      <th>4257</th>
      <td>Simple Twist of Fate</td>
      <td>Bob Dylan</td>
      <td>5.84436e+06</td>
    </tr>
    <tr>
      <th>7021</th>
      <td>Sloom</td>
      <td>Of Monsters and Men</td>
      <td>5.43e+06</td>
    </tr>
    <tr>
      <th>304</th>
      <td>Lonely Together - (feat. Rita Ora) Dj Licious ...</td>
      <td>Avicii</td>
      <td>1.73326e+07</td>
    </tr>
    <tr>
      <th>16963</th>
      <td>Love Sosa</td>
      <td>Chief Keef</td>
      <td>2.60406e+06</td>
    </tr>
    <tr>
      <th>8564</th>
      <td>Gold Digger</td>
      <td>Kanye West</td>
      <td>2.58114e+07</td>
    </tr>
    <tr>
      <th>4593</th>
      <td>So Many Tears</td>
      <td>2Pac</td>
      <td>1.02681e+07</td>
    </tr>
    <tr>
      <th>573</th>
      <td>Three</td>
      <td>Future</td>
      <td>2.04974e+07</td>
    </tr>
    <tr>
      <th>710</th>
      <td>XXX. FEAT. U2.</td>
      <td>Kendrick Lamar</td>
      <td>2.80441e+07</td>
    </tr>
    <tr>
      <th>276</th>
      <td>Nothing Left</td>
      <td>Kygo</td>
      <td>8.39098e+06</td>
    </tr>
    <tr>
      <th>4870</th>
      <td>It Takes Two</td>
      <td>Katy Perry</td>
      <td>2.05732e+07</td>
    </tr>
    <tr>
      <th>8814</th>
      <td>Brand New</td>
      <td>Pharrell Williams</td>
      <td>5.60257e+06</td>
    </tr>
    <tr>
      <th>1274</th>
      <td>Loser</td>
      <td>Beck</td>
      <td>2.66465e+06</td>
    </tr>
    <tr>
      <th>2731</th>
      <td>Christmas Time Is In The Air Again</td>
      <td>Mariah Carey</td>
      <td>4.54045e+06</td>
    </tr>
    <tr>
      <th>5041</th>
      <td>Beautiful</td>
      <td>Christina Aguilera</td>
      <td>8.38635e+06</td>
    </tr>
    <tr>
      <th>2775</th>
      <td>iSpy (feat. Lil Yachty)</td>
      <td>KYLE</td>
      <td>1.94756e+06</td>
    </tr>
    <tr>
      <th>3328</th>
      <td>Gin And Juice (feat. Dat Nigga Daz)</td>
      <td>Snoop Dogg</td>
      <td>1.21208e+07</td>
    </tr>
    <tr>
      <th>8546</th>
      <td>Bad - Remix feat. Rihanna</td>
      <td>Wale</td>
      <td>3.40864e+06</td>
    </tr>
    <tr>
      <th>6156</th>
      <td>Beast and the Harlot</td>
      <td>Avenged Sevenfold</td>
      <td>6.44798e+06</td>
    </tr>
    <tr>
      <th>8137</th>
      <td>You And Tequila (With Grace Potter) (Live At R...</td>
      <td>Kenny Chesney</td>
      <td>3.15612e+06</td>
    </tr>
    <tr>
      <th>4703</th>
      <td>Unfaithful</td>
      <td>Rihanna</td>
      <td>6.29086e+07</td>
    </tr>
  </tbody>
</table>
</div>





```python

```

