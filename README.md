# Predicting the Outcomes of NBA Games



## Project Overview


The focus of this project is to provide insight for NBA analysts towards understanding the most critical aspects that contribute to an NBA game's resulting final score. This project will be able to accurately predict how an NBA game will turn out given a set list of team attributes. Data regarding NBA games played in the 2019-2022 span (3 seasons) will be utilized to compose a final classifier model. The results of this modeling process will change the perspective on how analysts look at resulting game outcomes, and the factors that most affect them. 


## Business Problem 


The National Basketball Association (NBA) has changed to a great degree, especially in the past 3 years. While this is in large part due to the effects of the COVID-19 pandemic, the player base that has grown to prominence undoubtedly has the greatest effect. Players like Nikola Jokic, Stephen Curry, Kevin Durant, and Lebron James have changed the game to a great degree. With each passing year in the NBA, they set a new standard for future generations to live up to/surpass. The quality of an NBA player/team is not only defined by how well they shoot the ball or block shots; there is a myriad of other atrributes that emphasize overall contribution. Even from a holistic perspective, new coaches bring in new strategies into the mix that prioritize around certain players, methods of scoring, or managing game pace. 

In an ever-fluctuating game state, NBA team general managers (or GMs) may have a hard time figuring out how to create a winning NBA franchise from the ground up. Key players departing due to either retirement, trade demands, or career-ending injuries, can leave a team in a state of disarray. Investments to acquire players or optimal NBA draft slots may prove to be unprofitable in the selected players' resulting performances. Lack of sound decision making could cost GMs their overall credibility, which may lead to ridicule or job loss, in extreme cases. 

Data from the NBA 2019-2022 seasons (3 total seasons) will be aggregated/summarized in order to determine the most impactful characteristics that contribute to NBA game wins, and develop a machine learning model that can accurately predict the outcome of any game. Understanding the effect of every possible contributing game stat/contributor would help analysts make more informed predictions. The output of this project would help build an analyst's reputation while also  educating the fanbase on the numbers that truly matter. 

## Data Understanding


The data is comprised of 32 individual fields. 
There are 4 columns which represent cumulative game metrics across both teams,  __CLEANED NAME, GAME RESULTS, POINT DIFFERENCE, and PACE.__ 

The __CLEANED NAME__ is the unique identifier across the data set. Within the nomenclature, the field contains the date the game was played, the name of the home team, and the name of the away team.  This field will not be relevant for the model build, but it will be useful in the future steps of the analysis to understand which teams won which game. 

The __GAME RESULT__ field will be the classifier that the model will eventually try to predict. This is a binary outcome classifier, represented as 1if the home team wins, and 0 if the away team wins. 

The __POINT DIFFERENCE__ is derived from taking the absolute value of the difference between the Home and Away team's final scores.  For example, if the Home team scored 130 points, and the Away team scored 110 points, the point difference value would be 20. That being said, since the absolute value was taken within the calculation, every value in this column is an integer greater than zero.

The __PACE__ of the game is represented by how many individual possessions take place across both teams. It is a metric that defines the speed at which the game is played, And is calculated by the formula below:

PACE= 48 * ((Home team Possessions + Away team Possessions) / (2 * (Minutes played / 5)))

48 is an arbitrary number, used to represent the total minutes in a standard 4 quarter NBA game. 

The other 28 columns are split amongst the home and away teams,  resulting in 14 fields represented for each team.

__OFFENSIVE RATING__ represents each team’s performance as a result of the following formula: 
Offensive Team Rating = (Points*Total FG%) + Point Difference= 1/5 of possessions - Times Fouled+ FTM* FT% * OAPOW (Official Adjusted Players Offensive Withstand).
 
__DEFENSIVE RATING__ was represented in the initial data set on a player by player basis. The metric was averaged for any player that had minutes logged in the game, to get a cumulative team rating. The  column was calculated by the following formula for each player:
Defensive Player Rating = (Players Steals*Blocks) + Opponents Differential= 1/5 of possessions - Times blown by + Deflections * OAPDW( Official Adjusted Players Defensive Withstand

The respective field goal percentages, __2PT%__, __3PT%__, __FT%__  are represented by  the amount of made team field goals/field goal attempts for each shot type (2 point - shot within the 3 point line, 3 point - shot outside the 3 point line, free throw - at the line, after a shooting foul has occurred). 

__OFFENSIVE REBOUNDS__ represent rebounds made by the scoring team, keeping possession of the ball, while __DEFENSIVE REBOUNDS__ represent those where the defending team takes possession after a scoring team's shot attempt.

An __ASSIST__ represents the event where a player passes the ball to a teammate in a way that directly leads to a field goal score.

A __BLOCK__ represents the event where a defending player legally deflects a field goal attempt by the scoring player.

A __STEAL__ represents the event where a defensive player legally makes a turnover, causing a change in possession. 

A __TURNOVER__ represents a change in possession that occurs before a player on the offensive team attempts a shot.

__PLUS/MINUS__ is an aggregated metric, averaged amongst the players in the original dataset with played minutes. This metric reflects how the team did while an individual player is on the court. For example, if a player has a +5, it means the team outscored the opposing team by 5 points while they were on the court.

A __DOUBLE-DOUBLE__ represents the event where an individual player reaches double figures (10 or more) in 2 of the 5 main stat categories (points, rebounds, assists, steals, and blocks). A running count is aggregated for each home/away team. 

The final metric, __TOP 50 CONTRACT PLAYERS__, is a derived metric based on ESPN’s player salary database. A list of the players with the top 50 salary contracts was compiled for the 3 seasons, then a distinct list was found through deduplication. When aggregating the games by player, a running count was summed for each home/away team. 


## Data Preparation


Due to all relevant fields already being in numerical form, no additional preprocessing is necessary before the modeling process is started. The initial, unprocessed dataset already had all of the information in numerical form to begin with, the aggregation was done in Excel to avoid redundancy/keep simplicity. If the data were to be preprocessed in Python, the groupby function would be used to summarize each numerical column, either summing or averaging relevant data certain columns, then grouping by the __CLEANED NAME__ field as an identifier, while dropping the player name field. Players that had a __MINUTES PLAYED__ value were the only ones aggregated in the overall metrics. Players listed in a game with no minutes played would inherently be dropped, as their inclusion would throw off the average metrics.  

For this project, the plan is to use a classification model in order to assign a classifier to any given game, each game represented by a row in the dataset. The classification model will predict a binary outcome variable, represented by 0 or 1. 1 represents a win by the home team, and 0 represents a win by the away team.

It should be noted that the scale of the variables is drastically different, with some being represented as solely positive integers, while some variables like average __PLUS/MINUS__ may also contain negative values. That being said, a scaler will have to be applied in order to improve validity and performance of our model. 



## Data Modeling

Now that the relevant columns are cleaned, modeling with a focus on 'GAME RESULT' as the target outcome variable can begin. A machine learning model is desired for this analysis, due to the necessity of a system that can predict wins given a set number of fields. The model's win predictions will be compared to the actual win results from the 2019-2022 seasons; an accurate model will reveal what the most impactful columns were on win rate. 

For the most part, it seems that most of our variables have little correlation with each other, seen by the abundance of  combination correlations being below .4. There are some variable pairs that stand out as being highly correlated with each other. 
Due to the abundance of variables, the standout correlated pairs are difficult to pinpoint. The following code displays the top correlated variables within our dataset.

In order to avoid the issue of multicollinearity, aome of the attributes as defined above will need to be removed from the model. Multicollinearity is inherently a problem as performing a predictive model assumes that unit changes in each individual column has no effect on the others. That being said, a variable from each pairing with a correlation Factor greater than .75 will be removed from the model to mitigate this issue. 
 
Since __OFFENSIVE RATING__ and __DEFENSIVE RATING__ are scored on a game by game basis, it is natural that a game with a high calculated Home team offensive rating would consequently have a low Away team defensive rating. The outcome variable is based on point metrics,__OFFENSIVE/DEFENSIVE RATINGS__ will be removed for model validity.

Since __PLUS/MINUS__ is an attribute calculated based on when either a home or away team player is on the floor,  home and away team metrics will inherently have an inverse relationship.  The more points an Away team member scores than a Home team member, the higher the away team's overall plus/minus would be and the lower the home team's overall plus/minus will be.  Upon combing through the data, since the plus/minus metric is directly based on aggregate points scored, the comparison of home/away team plus/minus is associated with points scored by each team. The outcome variable is based on point metrics, we will remove the __PLUS/MINUS RATINGS__ for model validity.

Similarly, __TURNOVERS__ and __STEALS__ are correlated, as a steal from a Home team player inherently counts as a legal turnover counted for the Away team. 

__POINT DIFFERENCE__ was removed, as that metric is directly based on points, which is directly correlated to a win designation. 

Below, the highly correlated variables in the dataset are dropped. 




![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/HEATMAP.png)




### First Model

The first model will be created with simple classifier binary logistic regression, done to establish baseline performance of what a predictive model would look like. The following process depicts parsing out the data into training and test groups, done to perceive efficacy of a classifier across 2 different groups. The classifier will assign 1 as a prediction if the home team ends up winning, and 0 as a prediction if the away team ends up winning

The initial model performs relatively well on the test data, seen from the predicted values. F1 score is relatively high for both home team losses & wins classifications, implying succesful precision and recall values as well. Looking at the overall accuracy, across all observations, our model performed exceptionally well too.

The corresponding confusion matrices, seen below, re-interprets the information above in clean visuals, showing how the model classifies the test/train data using the logistic regression classifier trhough counts of true/false positives/negatives.


![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/CM1.png)

Seen from both plotted matrices and the calculated residuals (showing the same information summarized), the amount of accurately predicted counts is relatively proportional between the train and test data. The model appears to be slightly overfit on the training data, seen by the slightly higher % of residual value counts within the training data (90.7%). This implies that further application of parameters would help maintain overall model performance between train/test data while reducing overfitting.


### Second Model

A Decision Tree classifier is applied to the dataset, in order to get the perspective of a different model. A Decision Tree is comprised of different decisions that originate from the overall sample space. These decisions will be used to parse the data out, eventually getting to a granularity where the final nodes/groups of data can be classified. With metrics like 3PT% and rebounds being critical to success, decisions done based on these would be interesting to see. Below, the DecisionTreeClassifier is instantiated for use, and the game result is predicted based on the separated test data.


The second model has dropped to ~.74 accuracy in predicting game outcomes. This may seem like a drastic change which invalidates the model, but as explained in later sections, an accuracy close to .75 is expected when predicting NBA games. The training data scores clearly show the model is perfectly fit on the training data, with an accuracy score of 1. Parameter application in later model iterations will help reduce the overfitting.


![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/CM2.png)


The matrices/residuals show a slightly less accurate model overall on the test data compared to the first model, when using a decision tree classifier.

As seen by the above classifier residuals and confusion matrix results, our prediction quality went down within the testing data. This is still a high performing model, but the fit will be optimized with the addition of hyperparameters.
Due to the lack of applied hyperparameters, the Decision Tree fit on our training data had a perfect residual score of 1,  quantifying it as overfit. 



### Third Model


The application of hyperparameters will be observed in order to improve our initial Decision Tree model and mitigate overfitting on the training data. Below, the following charts are generated amongst the training/test data using the instantiated DecisionTreeClassifier in order to identify optimal hyperparameter values that maximize both training and testing AUC. The criterion 'entropy' is selected in order to prioritize minimizing messy data, while keeping predictive power.

The application of all hyperparameters specified maintained the accuracy within the testing data relatively well, while also greatly reducing the overfitting seen within the training data.

For the test data, recall was far higher than precision for win predictions, which implies the model was better at correctly identifying true game wins, rather than predicting whether a win happened. On the contrary, recall for loss predictions was far lower. This means the model tends to favor assigning false positive home team wins.

For the training data, similar trends can be seen within precision, recall, and f1-score for win/loss predictions, which makes the model performance relatively similar between train/test data. This means that the overfitting issue seen in the prior decision tree model has been fully mitigated.


![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/CM3.png)

![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/FI1.png)



Seen above, the most impactful features in the model involve playmaking on the offensive end, and overall metrics that contribute to a team's final score. Since the 3 point shot offers the highest potential for scoring (4 points, if fouled on the made shot attempt) it is unsurprising that 3PT% is directly equated to win percentage. Similarly, 2 PT field goals (2PT%) are the standard/easiest route of scoring for most teams in the NBA. It seems that assists and rebounds are highlighted to a degree as well. 


### Fourth Model


Next, the RandomForestClassifier will be tested for efficacy comparison amongst the other models. This model will be interesting to evaluate against, as it contains multiple Decision Trees in order to eliminate bias amongst individual trees. Each individual decision tree is based on a different subset of data so the resulting output will be a composite of several decisions. The nature of having multiple trees assists with reducing overfitting within the eventual output. The plan is to use now use GridSearchCV to identify optimal hyperparameters within the forest, for a more optimized method in replicating the same overfitting-reduction results in the prior model.


![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/CM4.png)


![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/FI2.png)

Similarly to the initial Decision Tree model, the Random Forest model highlights the same features of importance within determining game wins: 3PT%, 2PT%, assists, and rebounds. 

The __TOP 50 CONTRACT PLAYERS__ being recognized as a feature of importance within the forest shows the effect a player with a max contract can have on game win %. Typically, NBA management will only give these kinds of contracts to players who they can build teams around. The max contract players will typically stay with the team for at least 2 years, and more often than not, are their main scorers.



### Fifth Model


Finally, the XGBoost model will be used as a final comparison to our prior models. XGBoost is the top gradient boosting algorithm, used on trees in order to strengthen poor performing ones. A GridSearchCV parameter grid search will be used to tune the algorithm and identify ideal hyperparameters.



![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/CM5.png)


![view](https://github.com/n0vah917/dsc-phase-3-project-v2-3/blob/main/images/FI3.png)



Feature importance seems to take all possible metrics into account to a degree, but the prior metrics identified, 3PT%, 2PT%, and rebounds still appear to be the most prominent.


Seen from the above results, the model overfit on the training data, despite having a high accuracy when applying the model to the testing set. That being said, the Decision Tree (with identified hyperparameters) will be used as our final model, as it mitigated overfitting in the test/training data split while keeping overall accuracy high.


#  Classifier Results Evaluation

There are several factors that go into determining a typical NBA game win. In order to find the ideal factors that are critical to the determination of game wins, the performed analysis/model creation using the 2019-2022 NBA dataset revealed the scale/importance of several of these key metrics. The final model created, a Random Forest Classifier with applied hyperparameters, was able to predict NBA game wins with an accuracy of 77% for test data and 80% for training data. 


For the test data, recall was far higher than precision for win predictions, which implies the model was better at correctly identifying true game wins, rather than predicting whether a win happened. On the contrary, recall for loss predictions was far lower. This means the model tends to favor assigning false positive home team wins. The tendency to designate false positive wins can be overlooked since the recall % of overall home team wins was high, which is the main target metric of success within the model. 

For the training data, similar trends can be seen within precision, recall, and f1-score for win/loss predictions, which makes the model performance relatively similar between train/test data. Both test/training predictions having ~80% accuracy implies that the overfitting issue seen in all prior models on the training data have been mostly mitigated. While an 80% accuracy across both test/training data may be seen as a poorly performing model, NBA games have a typical upset (where the non-favored team ends up winning) rate of 32%. This is understandable when looking at examples in recent years, such as the Toronto Raptors' 2019 NBA Finals win against the favored Golden State Warriors, and the Boston Celtics 4-0 playoff sweep against the favored Brooklyn Nets. That being said, the current accuracy rate with this model is to be expected. 

## 3PT and 2PT Percentages

Seen above, the most impactful features in the model have to do with playmaking on the offensive end. Since the 3 point shot offers the highest potential for scoring (4 points, if fouled on the made shot attempt) it is unsurprising that this percentage is most equated to win rate. Similarly, 2 PT field goals are the standard/easiest route of scoring for most teams in the NBA. Nearly all of NBA players have a higher average 2 PT % compared to their 3 PT %, so the importance is less impactful. It is far rarer to have a consistent and efficient 3 point shooter on most teams, so scoring ability in that regard is highly valued. Players that can take advantage of both field goal percentages inherently have more scoring potential, and are offensive assets. More often than not, these scorers typically have the highest value contracts, are all-star selections/candidates, and are starters for their team. Some of the most prolific scorers/shooters active in the NBA are Stephen Curry, Kevin Durant, Giannis Antentokounmpo, Joel Embiid, and Nikola Jokic. All of the named players have been on teams that have made deep NBA playoff runs in recent years. 

Stephen Curry in particular is widely regarded as one of the most efficient 3PT shooters of all time, holding the current all-time NBA made 3PT shot record at 3,117. He has won NBA championships with the Warriors in 2015, 2017, and 2018.

Kevin Durant's play style, athleticism, and build have made him one of the most difficult NBA players to guard; his ability to create open shot opportunities from the 3PT and 2PT range is fundamental to his role as a scorer.  He has played a pivotal role in the success of the Oklahoma City Thunder, Golden State Warriors, and Brooklyn Nets franchises.

Giannis Antentokounpo, Nikola Jokic, and Joel Embiid were all MVP (most valuable player) candidates for the 2021-2022 season; Jokic was awarded the title in 2021 and 2022, and Giannis was awarded the title in 2020 and 2019. The imposing builds of all 3 of these "big men" make them nearly impossible to guard when in the "paint"/driving to the rim. 


## Rebounds

Total team rebounds are seen as an important factor as well. This makes sense, as the more that a defending team is able to regain possession during a game, the more opportunities that creates for that team to score on a fast break, before the other team has a chance to reset their defense. Similarly, the more that a team can rebound on the offensive end, the more they can prevent the other team from taking possession and scoring points. Current rebound leaders in the NBA include Rudy Gobert, Nikola Jokic, Giannis Antentokounmpo, and Joel Embiid. 

Rudy Gobert has played a pivotal role for the Utah Jazz on the defensive end, having a career average of almost 12 rebounds per game. His high rebound/block rates have earned him the DPOY (Defensive Player of the Year) award 3 times in the past 4 years. 

As previously mentioned, Giannis, Jokic, and Embiid were all MVP candidates for the 2021-2022 NBA season, with Jokic winning the title. It is natural that players who can be assets on both offensive and defensive ends of the court are highly valued. 


## Assists

Assists are high in importance as well. Players that don't have as strong of an affinity for scoring can still be valued if they can create plays for their teammates through their passing ability/leadership. This leads to an increase in a team's resulting scores. Assists as a whole can be used to identify how well a team works together in order to score. The more assists a team has by the end of the game either highlights a team's consistent offense, or an opposing team's lackluster defense. Current assist leaders in the NBA include Chris Paul, Lebron James, and Luka Doncic.

Chris Paul has been in the top 20 season APG (assists per game) ranking list for the past 10 years. His ability to create shot opportunites for his teammates has led the Phoenix Suns to the NBA finals in 2021, and the playoffs in 2022.

Lebron James has been widely regarded as the GOAT (Greatest of all Time) by many NBA fans, and for good reason. His ability as a scorer/team leader has led him to 4 NBA championships with the Cleveland Cavaliers, the Miami Heat, and the Los Angeles Lakers.


## Recommendations/Conclusion


NBA General Managers(GMs) have the difficult job of determining ideal roster candidates in a game so highly sensitive as basketball. They are undoubtedly placed under a lot of scrutiny, as their job title inherently implies a strong understanding of the current game state. Several general managers, such as Elgin Baylor, have terrible draft picking making ability, resulting in very little success in the regular season and no appearances in the playoffs. Bad player investments can hinder a team's performance for years, or even decades. An GM’s teambuilding ability is directly correlated to their baseline understanding of NBA metrics and current/past NBA knowledge. Knowledge of game metrics and the effect they have on a win/loss result is critical to their success. 

The created model to help analysts identify crucial metrics is able to predict game wins/losses for the home team given a total set of 19 metrics, with ~80% accuracy. The state of the model is optimal, as the typical NBA upset rate is ~32%, which means that 32% of the time, the predicted favored team (based on pre-existing metrics) will end up losing. Model performance is about the same between test/training sets which means that the model’s success isn’t heavily reliant on the data used to train it, and will perform relatively well in the face of new data. 

The model identified the following stats as critical in determining game wins, in order of importance: 3 Point %, 2 Point %, Rebounds, and Assists. Although the feature importance diagram specifically lists home/away team designations, these designations should be ignored when evaluating overall model performance; it can be assumed that assists/rebounds generally both contribute to win rates. 

Translating these findings to prioritizing player picks/trades players overall higher scoring percentages, particularly when it comes to 3PT shots should be idealized, especially in today's 3-point driven game state. If these metrics are similar/inconsistent across players, players with higher rebound rates should be prioritize. Rebounds are a clear indicator of how well a player is able to maintain ball possession, on both the offensive/defensive ends of the court. Finally, if scoring percentages/rebound rates are similar, players with higher average cumulative assists/game should be favored, as they can be seen as more cohesive/better at creating scoring opportunities. 

Similarly, young NBA players that have aptitude for one/many of these metrics should be highlighted as key players to look out for in the future. For example, Zion Williamson, a young superstar on the New Orleans Pelicans, averaged 9 rebounds and 23 points during his time at Duke University. Similarly, Ja Morant, a current key offensive presence on the Memphis Grizzlies, averaged 25 points and 10 assists during his last year at Murray State.


### Next Steps

Due to the nature of the success metric, accuracy, next steps in improving this model would largely involve the addition of variables that help better represent/determine game wins. Upon finishing the analysis side of the project, it was found that there are four main metrics that are actually used to best determine win percentage: effective field-goal percentage, turnover percentage, offensive rebound percentage, and free throw rate.

Although two of those metrics are covered within our model (turnover percentage and free throw rate), the potential effect of effective field goal percentage and offensive rebound percentage on model performance/resulting accuracy would be interesting to see.

Currently, the __TOTAL TEAM REBOUNDS__ within the model is a cumulative metric that takes both defensive/offensive rebounds into account. Offensive rebounds should be looked at in particular, as they reveals a team's tendency to keep possession of the ball, allowing them to better utilize remaining time on a shot clock, slow down game pace, and create different opportunities for scoring. In contrast, defensive rebounds involve regaining possession after a shot attempt has been made, but this metric doesn't inherently lead to scoring on the other end of the court.

Effective field-goal percentage is technically present in the current model state, and is calculated by evaluating 3-point percentage and 2-point percentage within one metric. The metric gives more weight to the 3-point field goal made, since it is valued higher than a normal 2-point shot. Using this aggregated metric would be interesting to see, and may potentially improve/decrease resulting accuracy.

Injuries should be looked at as well, as a team missing their key offensive/defensive pieces has less player options to cycle through the duration a game. Considering the effect of the pandemic, several players were added to rosters on 10-day contracts. This sense of impermanence within a team could definitely throw off chemistry and game performance.

Finally, outside of the realm of general basketball statistics, a way to quantify external factors that involve NBA players would be of interest, whether they be interactions with media, other team players, or the city that a player is playing in. Some of these factors could have significant effects on a player's performance. 

For example, Kyrie Irving on the Brooklyn Nets is widely regarded as one of the best point guards of all time. Controversy involving his departure from the Boston Celtics in 2019 caused him to be booed whenever he held the ball in TD Garden (Celtics home court). In an ideal scenario, a professional basketball player would not be affected by these factors, as they are expected to play under pressure at all times. However, as seen from Game 2 in the first round of the 2022 playoffs, Kyrie was held to an abysmal 31% FG percentage, with 4 out of 13 made attempts. While this could be a factor of relentless Celtics defense, the crowd’s energy undoubtedly had an effect on his lackluster performance.

Russell Westbrook is a player who has been under a tremendous amount of scrutiny during the season since he joined the Lakers in 2021. The fans and media have started calling him "Westbrick" due to his recent trend of bad shot selection/playmaking. While this could be seen as a factor of Westbrook not yet getting adjusted to his role on the Lakers, the constant media/fan attention to the former MVP can be seen a contributing factor to his poor performance. This season, the Lakers noticeably struggled and failed to make the playoffs, despite being favored to win the 2021-2022 NBA championship by some fans/analysts.

Rivalries, whether on a player-by-player level or a team-team level undoubtedly have a factor on resulting game metrics. Long-standing rivalries like the Lakers and Celtics, or recently formed rivalries like the Atlanta Hawks/NY Knicks & Brooklyn Nets/Milwaukee Bucks, are inescapable factors that affect the energy of players and fanbases. 
On a player-by-player basis, the tension between James Harden and Giannis Antentokounmpo is a notable example of a recently formed “rivalry”. When Harden was still an active player on the Rockets, he was noted as saying, “I wish I was 7-feet, and could run and just dunk. That takes no skill at all. I have to actually learn how to play basketball and how to have skill. I’ll take that every day.”
The slight against Giannis undoubtedly fueled his 44-point performance against Harden in a Nets/Bucks matchup on March 30, 2021. Giannis was seen to address Harden’s comment in that post-game interview saying, “It's good because I'm changing the narrative. I don't want to be the guy only that dunks and runs. I can make a three.”. 
...
├── images #folder containing images used for readme
├── README.md
├── Predicting NBA Outcomes.pdf    #pdf of presentation
├── 20192022NBAGAMES3.csv #data used for analysis
├── jupyter notebook.pdf #pdf of jupyter notebook
└── Phase 3 Project - Predicting NBA Game Outcomes.ipynb #jupyter notebook containing all relevant summaries/analyses





