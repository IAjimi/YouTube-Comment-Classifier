# YouTube Comment Classifier
This project was completed as a class requirement for Data Mining for the Social Sciences (Fall 2019).

Our objective was to automatically flag comments which violate [Youtube's community guidelines](https://www.youtube.com/about/policies/#community-guidelines). We focus on comments which contain:

* nudity or sexual content
* harmful or dangerous content
* hateful content (*"content that promotes or condones violence against individuals or groups based on race or ethnic origin, religion, disability, gender, age, nationality, veteran status, caste, sexual orientation, or gender identity, or content that incites hatred on the basis of these core characteristics"*)
* violent or graphic content
* harassment or cyberbullying
* threats
* spam
* child safety

We downloaded a dataset of YouTube comments from [Kaggle](https://www.kaggle.com/datasnaek/youtube). This dataset contains 100 web-scraped comments for every video in the top 200 U.S. trending over a number of days, as well as information about the video those comments were posted on. 

We divided our analysis into 2 steps: pre-processing messages, followed by the implementation of classification models. 

Pre-processing is done both to generally adjust to the nature of our data, which is primarily made up of comments, and to its source, the Internet. Given the latter, we clean our dataset to adjust for the presence of emojis (coded in a `<..>` format, e.g., `<e2><80><bc><ef><b8><8f><e2><80><bc><ef><b8><8f><e2><80><bc><ef><b8><8f>`) and links.

After pre-processing our dataset, we measured

* the number of characters within the message (message length)
* the number of *letters* within the message
* the number of *capital letters* within the message
* the number of *punctuation marks*
* the percentage of the three variables above
* the difference in message length before and after collapse all repetitions of characters over 3 (e.g. "looooooooool" to "lool")
* the average word length
* the number of "bad" (offensive) words, found from a [list](https://www.cs.cmu.edu/~biglou/resources/bad-words.txt) online, for every bad word
* the percentage of bad words within the message
* the sentiment score, adjusted for the presence of said insults
* the number of unique words
* the sum of tdif

Our approach is primarily *content-based* approach, relying on textual features to identify abusive messages.

We trained logistic and elastic net regressions, gradient boosted decisions trees, and KNN classifiers on our data, using 10-fold cross-validation.

Our best performing model was the logit regression (accuracy +- 96%), with variables selected using backward-selection and "bad word" columns re-included into the training and testing sets. The model's performance, when compared to the No Information Rate, is fairly low. Changes such as splitting the dataframe based on heuristics, using up/downsampling and different metrics failed to produce any results. Ultimately, the logit was the best model for our data, as it ran quickly and could use important, low-variance variables.

Our model could have been improved by labelling our observations into categories like spam/offensive/cyber-bullying/etc. It would also have been helpful to incorporate more context-related variables into the dataset, such as content tags, user history, etc. More refined variables, which take into account the target of a message, would also have been helpful. Finally, having more data, to cope with our limited number of observations, would also have improved our model's performance.

Ultimately, this was a fairly difficult task, as context makes it hard to distinguish between truly offensive and merely mean messages. 
