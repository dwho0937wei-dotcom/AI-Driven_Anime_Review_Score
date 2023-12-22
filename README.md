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
For now, I only used [animes.csv](https://github.com/dwho0937wei-dotcom/My_Anime_Capstone/blob/main/animes.csv) as data for predicting some anime's popularity, but I may use the other two CSVs in this repo later on as I continue this project. The three CSVs came from the [Anime Dataset with Reviews](https://www.kaggle.com/datasets/marlesson/myanimelist-dataset-animes-profiles-reviews) Kaggle website.                                    
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
1. Column **synopsis** was first cleaned of any potential null values and then lemmatized. Due to the very long run time it would take to train and test all 15,583 synopses, I only randomly chose 1,500 of them. The Bag-of-Words (BoW) with Random Forest Regression was then done for predicting the number of members. Other slightly different methodologies was also done but this methodology in particular gave the highest accuracy.
2. Column **genre** consists of tuples of genres which were then transformed into a string of genres, each genre separated by an empty space. BoW was then used to get binary columns of distinct genres, i.e. they represent if their corresponding genre is assigned or not in the anime. Random Forest Regression was used again for predicting the number of members.
3. Other numeric features **episodes, ranked, and score** were used for training the Decision Tree model for it to predict the number of members.

All three have been GridSearched in an attempt to get higher accuracy.

#### Results
Unfortunately, despite using GridSearch, all three methodologies have very low score. The 1st one having the highest score but only slightly higher than a quarter. The 2nd one's score is significantly lower, and the 3rd one is the lowest in which I was surprised on how extremely low it is.
This is the barplot that visualizes it all:
![alt-text](https://github.com/dwho0937wei-dotcom/My_Anime_Capstone/blob/main/images/1st_Capstone_Barplot.PNG)

#### Conclusion & Next Steps
Thinking about it rationally, it would make sense on why the accuracies are so low.
After all, the anime's synopsis and assigned genres shouldn't matter on its quality.
I was rather surprised that the anime's number of episodes, ranking, and score are even more useless on predicting the number of members it would gain.
In the end, what matters significantly more is how it's written, played, and expressed towards the audiences as well as what impact it can leave to the audiences. Visual quality may also matter.
Unfortunately, the initial DataFrame I've used doesn't have such information.

Hence, I am left with two options:

1. Continue researching on the same research question using different models, fine-tune more parameters, and/or implement other unused CSVs that I also have.
2. Change my research question into something more feasible to answer/predict with the current data I possess like instead of predicting the anime's popularity, perhaps I should predict its score as the score is also worth noting on the anime's quality.

For now, I am leaning a little towards the 2nd option.

### Part 2
Based on the first **Conclusion**, I've decided to follow through the second option, i.e. switch from popularity to scores.

For Part 2, **Research Question** remain pretty much the same except that popularity is replaced with score this time which can determine the anime's quality. I believe predicting the score is beneficial for the same **Rationale** as in popularity. **Data Sources** remain the same. The **Methadology**, **Results**, and **Conclusion & Next Steps** will definitely differ and hence, I will post them further down.

#### Methdology 2
I came up with three different methodologies for predicting the new targeted feature "score", each focused on either Synopsis, Genre, or the other Numeric Features.
1. Like in **Part 1**, column **synopsis** is cleaned of any null values and then lemmatized. However, instead of using the Natural Language Processing (NLP), I'll be using the following Keras Sequential Model I've created using the Embedding, GRU, and Dense layers compiled with Adam algorithm and measured with mean squared and mean absolute errors. This led the synopses to be further processed with keras.preprocessing.text.Tokenizer and keras.preprocessing.sequence.pad_sequences in order fit into the model.
2. Like in **Part 1**, column **genre** transformed from a tuple to a string of genres and then separated by empty spaces. Because I'll be using Keras Sequential Model with GRU, and two Dense layers, I'll need to further process the **genre** into numpy arrays for binarily fitting it into the model. The model is compiled with Adam algorithm and measured with 'binary_crossentropy' and with mean squared error.
3. Because of the results from the previous 2 methodologies that I'll later disclose, I've decided that instead of using the Keras Sequential Model, I'll simply use the XGBoostRegressor for predicting the score using the numeric columns episodes, members, and ranked. 

#### Results 2
I believe the first two methodologies are worse than the ones in **Part 1**. The first one focusing on the synopsis didn't work at all with Invalid Argument Error that I'm uncertain how to fix. The second one focusing on the genre has a high mean squared error of 13.6070 considering that the score only ranges from 0 to 10.

On the other hand, the third one is way better than the one in **Part 1** because I correctly used a regression model XGBoostRegressor to predict the continuous score compared to when I used a classifier model DecisionTreeClassifier to predict the number of members rather than the regression model DecisionTreeRegressor. The XGBoostRegressor received an accuracy score of approximately 81% with its test mean squared error approximately 0.192 for predicting the anime's score.

#### Conclusion & Next Steps 2
The Keras Sequential Model is currently not suitable for Synopsis nor Genre on my part. This may further acknowledge that synopsis and assigned genres are not strongly related, if related at all, to the anime's quality. Or, this is most likely because I am still inexperience and unfamiliar with efficiently utilizing this model for texts compared to Natural Processing Language (NLP).

Thus, my probable next step for Synopsis and Genre is either:
1. continue researching on the Keras Sequential Model on Synopsis & Genre,
2. or revert back to models in **Part 1** to use for predicting the scores.

As for the numeric columns (episodes, ranked, and score) back in **Part 1**, I've noticed or been reminded that I mistakenly use the DecisionTreeClassifier in **Part 1** for predicting number of members when it should have been DecisionTreeRegressor instead.

I've already moved on to scores as the target feature, and using the XGBoostRegressor with the three other numeric columns (episodes, members, ranked) is pretty effective on predicting the score. It has a high accuracy of 81% and thus, shows that the anime's number of episodes and members, and its ranking may strongly correlate with its score. 

For my next step, I may either:
1. fine-tune XGBoostRegressor,
2. or try other regression models with their default parameters to see if they can more accurately predict the score based on the given numeric features.

#### Outline of project

- [Anime_JupyterNotebook](https://github.com/dwho0937wei-dotcom/My_Anime_Capstone/blob/main/Anime_JupyterNotebook.ipynb)





##### Further Information
Note that the [.gitattribute](https://github.com/dwho0937wei-dotcom/My_Anime_Capstone/blob/main/.gitattributes) in the repo is for committing the three large CSVs into this repo since some CSVs' size exceeds the size limit that this repo can commit the CSVs into.
