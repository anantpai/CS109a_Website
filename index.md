---
title: Group 12: Spotify
---

# Group 12: Spotify

<center> Welcome to our homepage! We're so excited to present the first iteration of the Spotify Regressor (TM) </center>

<center> Group Members: Anant Pai, Andy Kim, Katherine Cohen, Trevor Noon </center>

## <center> Motivation </center>
> <center> Our project centers around the analysis of Spotify curated playlists. Taking an analytical and data driven approach, we sought to understand what factors lead to playlist success in the form of followers. From that point, we sought to use this predictive understanding to create a model that generates potentially 'successful' playlists for a given user. </center> 

## <center> Problem Statement </center>
<center> More specifically, we refined this into a two stage problem statement more clearly defining the term 'successful': </center>


> <center> 1. Predicting number of followers that a playlist will have using combinations of
the above predictors that were seen to have varying degrees of association with playlist
success, and using appropriately transformed variables </center>

> <center> 2. Generating our “own” successful playlist. Our goal is now to create a playlist
that we will project to have at least 200 thousand followers. This will place our playlist
well within the top quartile of Spotify playlists, which includes any playlist over 163
thousand followers. </center>

<center> This broke our project down into three areas: modeling for prediction with regression, continuing that modeling with classification given the 200K follower cutoff, and finally the playlist generation itself. The final end goal is to create a playlist predicted to have at least 200K followers. </center>

<center> --- </center>

#### <center> Literature Review/ Citations </center>

<center> Outside work that was key to our project's success includes primarily the Spotify API and the publicly available spotipy 2.0 package. First off, using the Spotify API, we were certain to license our individual machines and abide by the developer terms of use located at https://developer.spotify.com/developer-terms-of-use/. With the spotipy package, we continued through the authentication with the Spotify API to use it in tandem. The spotipy package is freely available to the public as outlined in the following license: https://github.com/plamere/spotipy/blob/master/LICENSE.txt. </center>
 
<center> Looking into outside readings, we looked into the two references provided to us (cited below). Specifically reading into the ISMIR article, we did not delve into the specific audio-file similarities between two different songs, however we did draw inspiration for predictor variables from the section on co-occurence (as well as from talks with out TF Nick Hoernle). Specifically, this resulted in us evaluating the genre specifics and dynamics inherent within a playlist to see if they're all the same genre or varied either purposefully or incidentally. Reading into Logan's article, we similarly skipped the spectral recognition piece since we did not analyze audio files, but likewise we drew inspiration from the section on the number of songs from a given artist. So, while we did not delve into the specific technicals for either of these articles as we did not analyze audio files themselves, they were helpful and influential into crafting our prediction variables. </center>
 
1.Berenzweig, Adam, Beth Logan, Daniel P.W. Ellis and Brian Whitman. A Large-Scale Evaluation
of Acoustic and Subjective Music Similarity Measures. Proceedings of the ISMIR International
Conference on Music Information Retrieval (Baltimore, MD), 2003, pp. 99-105.
 
2.Logan, B., A Content-Based Music Similarity Function, (Report CRL 2001/02) Compaq Computer
Corporation Cambridge Research Laboratory, Technical Report Series (Jun. 2001).
