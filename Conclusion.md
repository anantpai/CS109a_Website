---
title: Conclusion
notebook: Conclusion.ipynb
nav_include: 5
---

Despite facing multiple obstacles over the course of this project, such as data road blocks and models that weren't as successful as we would have liked, we were ultimately able to gain insights as to some factors that might be related to overall success of a playlist, that translated into generation of cohesive playlists that we believe have the potential to be successful. Our goal of correctly predicting playlist followers definitely fell short of our initial hopes, however using what we learned about the data, we were able to develope alternate strategies through shifting our scope to the classification of successful playlists. Through this, we ultimately did see significant improvements.  

**Strengths**:
Our strengths as a group were in most areas except for the direct results. We were able to secure the data from the Spotify API early on, complete good EDA and the creation of new variables and a good foundational data set to build off of. Additionally, despite not having the best $R^2$ values or incredible results from our models, we were still able to generate some pretty fire playlists (if we may say so ourselves) with the information and insights that we gained from these models.

**Weaknesses**: Our main weaknesses were in the models themselves. We had very little success in absolute prediction accuracy and in nailing down any insights that weren't intuitive naturally when thinking about the playlists on a qualitative level. A lot of our test $R^2$ values were 0 or negative and at times our AUC was very close to 0.5 meaning we weren't much more accurate than a coin flip.

In terms of generalizability, our model also weaknesses. Had our classification and regression models worked perfectly, they still would not have been very generalizable to playlists in general. We defined success by Spotify’s standards, as we only used their own curated playlists to measure popularity and number of followers. If we were to make a model to predict the success of playlists in general, it would be interesting to pull data from different streaming services’ APIs, such as Apple Music and Tidal, and gather data on the success of their own curated playlists as well.

**Future Work**: If given more time (and resources) we would like to expand our dataset and remodel. There is certainly valuable information that could be indicative of playlist success that was not accessible through the spotify API. We strongly believe that Spotify playlist success is directly correlated to the amount of exposure that Spotify gives it in terms of visual space within their application. Some playlists are very niche and can only be found when searched whereas others appear everytime you open the application. There is also the issue of the methods utilized, which were limited to the regression and classification methods covered in this course. Given the complexity of predicting a playlist's success, and how unlikely it is that these will all follow any particular patter, it is likely that these methods are simply not robust enough to predict number of playlist followers. Additionally, given more time, we may have taken a wider scope and gone for a more niche approach in playlist creation (as they did for the recent 2017 'wrapped' promotion).


