# Update 3 - (4/29/22)
******

## RECAP



## PROGRESS

### PREPROCESSING

With regards to preprocessing, we:
* Attempted to drop the six lowest class labels that all had less than 100 observations from our training and validation. 
  * This would allow us to train our models without these small classes and then use the models to see what these small classes would be predicted as. After dropping the smallest classes, we trained both a logistic regression and a support vector machine with the default hyperparameters and used both models to predict over the text for the dropped classes. As a result, both models predicted most of the smaller classes to be N/A: No relevant content, which makes sense as that specific class has the most observations by far. 
  * We presented these findings to our sponsor who told us to just drop the six smallest classes from the dataset and to state in our final report that these classes exist in the real world, but are not worth keeping in our models as their frequencies are so low. 
* Decided to solely use lemmatization as opposed to creating models for both the lemmatized and stemmed forms of the text.
* Decided to use a TFIDF-vectorizer as opposed to both a TF-IDF and a CountVectorizer just for simplicity sake.
* Decided to add in bigrams within our preprocessing pipeline
  * Instead of using solely using unigrams when vectorizing the text column, we decided to create models with only unigrams as well as models with both unigrams and bigrams.
  * According to our sponsors and mentor, in most NLP related models, the addition of bigrams creates a significant improvement in terms of accuracy.
  * This also created a challenge for us as the number of columns when using both unigrams and bigrams increases from about 400,000 to about 5 million. One solution that our sponsors provided us with is to use different forms of dimensionality reduction.
* Attempted to reduce the dimensions of the vectorized dataframe through different methods.
  * NMF: Non-Negative Matrix Factorization: Finds two non-negative matrices (W, H) whose product approximates the non-negative matrix X.
    * Worked fine for logistic regression, naive bayes, and the support vector machine.
  * Truncated SVD: This transformer performs linear dimensionality reduction by means of truncated singular value decomposition (SVD).
    * Produces negative values which does not work for the naive bayes model but worked fine for the logistic regression and the SVM.
  * We compared the use of each dimensionality reduction to the models where we do not use dimensionality reduction and while it reduces computation time, the reduction does not produce better results in terms of precision, recall or F1 score.
* Removed uncommon words that appear in less than 5% of the documents.
  * This allows us to speed up computation time by removing words that will have little effect on classifying the document into one of the types of fraud, solely due to the infrequency of the word.

### MODELING

Since our last progress report, we:
* Decided upon a modeling metric to look at in order to determine which models we prefer.
  * The modeling metric we decided to use is Weighted Average Precision. We chose a weighted average over a macro average due to the imbalance in size between classes.
  * Comparing this metric for all the models allows us to determine which models are “better” when doing cross validation to choose hyperparameters. 
* Opted to not use the balanced class weights parameter in our models.
  * In our last progress update, we mentioned the possible use of the balanced class weights parameter which would allow the model to better capture smaller classes. However we showed our results to the sponsors who said that the decrease in total accuracy is not worth the increase in accuracy for the smaller classes.
* Still have not moved on to other models as we are still working on cross-validating in order to tune hyperparameters for our existing models

### VALIDATION

Since our last progress report, we implemented a grid search cross-validation for all of our models.
* For naive bayes, we consider the following hyperparameters:
  * A smoothing parameter for additive smoothing or no smoothing
  * A parameter whether to learn class prior probabilities or to assume a uniform prior
* For linear regression, we consider the following hyperparameters:
  * Regularization parameter
  * ElasticNet ratio for Lasso regression, Ridge regression or a combination of both penalties
* For our SVM classifier, we consider the following hyperparameters:
  * Regularization parameter
  * Kernel type
