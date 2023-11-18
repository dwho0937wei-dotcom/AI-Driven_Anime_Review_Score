## My Anime Capstone

**Daniel Ho**

### Executive summary

#### Research Question
Can we accurately predict if the upcoming anime will become popular or not based on its
genres, ranking, and other features? What are the factors that can make more audiences value or like
a particular anime?

#### Rationale
Correctly predicting which anime will be popular will help guide resource allocation for
marketing, production, and distribution because studios can mainly focus their time and effort on
anime predicted to be popular.
Anime predicted to be popular can then, business-wise, be taken advantage of by opening up
more collaboration opportunities. By collaborating with merchandise manufacturers, gaming
companies, and/or streaming platforms, studios and other related businesses can greatly
capitalize on this anime because of its supposedly large audience.
Also, anime predicted to be popular can be potential subjects to plan early for content creating
more seasons of these anime. If a lot of people like these supposedly popular anime so much,
then why not create more seasons based on them so that people can enjoy watching more of
these anime?

#### Data Sources
For now, I only used [animes.csv](https://github.com/dwho0937wei-dotcom/My_Anime_Capstone/blob/main/animes.csv) as data for predicting some anime's popularity, but I may use the other two CSVs in this repo later on as I continue this project.
The animes.csv contains the following features:
1. uid - The ID of the anime/manga
2. title - The title of the anime/manga
3. synopsis - A brief intro to what the anime/manga is about
4. genre - The list of genres the anime/manga is assigned to
5. aired - When it started airing and when it ended
6. episodes - The number of episodes
7. members - The number of users that have that entry in their anime/manga list
8. popularity - The overall position of the entry by how many users have that in their anime/manga list
9. ranked - The overall position of the entry sorted by how users score the work, from highest to lowest.
10. score - The overall rating score on a scale from 1 to 10, from worse to best.
11. img_url - Basically the image url of the corresponding anime/manga
12. link - The MyAnimeList link of the corresponding anime/manga

Since we are trying to predict the popularity of anime and to clarify, we are trying to predict the number of people who will involve themselves to this anime, we are predicting the column **members**, not column popularity.

I believe the features uid, title, aired, img_url, and link are not valuable for this prediction so we'll be leaving them out. 
The feature popularity will also be left out because if we need to assume that we don't know how many members will be part of this anime yet, then we shouldn't know the anime's popularity rank.

#### Methodology
There are three distinct methodologies done for different features in an attempt to predict the anime's member size.
1. Column **synopsis** was first cleaned of any potential null values and then stemmed. The Bag-of-Words (BoW) with Random Forest Regression was then done for predicting the number of members. Other slightly different methodologies was also done but this methodology in particular gave the highest accuracy.
2. Column **genre** consists of tuples of genres which were then transformed into a string of genres, each genre separated by an empty space. BoW was then used to get binary columns of distinct genres, i.e. they represent if their corresponding genre is assigned or not in the anime. Random Forest Regression was used again for predicting the number of members.
3. Other numeric features **episodes, ranked, and score** were used for training the Decision Tree model for it to predict the number of members.

All three have been GridSearched in an attempt to get higher accuracy.

#### Results
Unfortunately, despite using GridSearch, all three methodologies have very low score. The 1st one having the highest score but only slightly higher than a quarter. The 2nd one's score is significantly lower, and the 3rd one is the lowest in which I was surprised on how extremely low it is.
This is the barplot that visualizes it all:


#### Next steps
What suggestions do you have for next steps?

#### Outline of project

- [Link to notebook 1]()
- [Link to notebook 2]()
- [Link to notebook 3]()


##### Contact and Further Information
Note that the .gitattribute in the repo is for committing the three large CSVs into this repo since some CSVs' size exceeds the size limit that this repo can commit the CSVs into.
