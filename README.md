# Collaborative-Filtering-Recommendation-System-Using-Surprise-Library
RMSE Score: 0.9211

Approach
Recommender system makes use of either content-based filtering or collaborative Filtering system or both combined to predict as accurately as possible the user-item pair ratings. Content-based filtering recommends items to a particular customer similar to previous items rated highly by that particular user, while collaborative filtering finds the set of users that have similar preference as the user for which we want to predict the rating and on the basis of these set of user’s preferences we recommend the items to a particular user. Now a day the recommender system combines both the content-based and collaborative filtering system to make a hybrid collaborative filtering system that takes care of various major issues like cold start and sparsity. 
I have used collaborative filtering approach, which predicts the user-item pair rating based on the k-values of nearest neighbours preferences and using “min_support” parameter in hyper-parameter tuning of algorithm, which takes care of cold start issue by discarding those users that have rated any items less than the value specified in “min_support” parameter. 

Methodology and Associated Parameters
I have used the “Surprise Library” of Python, which was quite useful in developing a recommendation system because of its built-in algorithms for every functionalities. There are total three files used in this recommender system: Train.dat file consists of training dataset with 4 columns named “UserID”, “ItemID”, “Rating” and “Timestamp”. Test.dat file consists of test data set with two columns named “UserID” and “ItemID” and have to predict the ratings based on the user-item pair given in Test.dat file. Those estimated ratings are stored in Format.dat file by replacing the 1s in it with the equivalent number of predicted ratings. The libraries used in this recommendation system are Pandas, Numpy, Surprise, Matplotlib and Seaborn. The five steps implemented to recommend the ratings using surprise library are as below:
1)	Import dataset: Downloading the dataset into Pandas data frame and making it accessible to Surprise by below line of code. Where the rating_scale is defined on the scale of 1 to 5, 5 being the best and 1 being the worst rating given by the user for a particular item.
reader = Reader(rating_scale=(1, 5))
data = Dataset.load_from_df(train_df[['UserID', 'ItemID', 'Rating']], reader)

2)	Cross – Validation: Cross-validation assist in improving the quality of recommendation. Then we measure RMSE value for each algorithm, which gives the average error, where big errors are heavily penalized (squared). This process is repeated many times with different subsets, this is called as cross-validation. The below is the dictionary of all the algorithms along with the RMSE, fit time and test time value. As per the observed results I am using KNNBaseline model for my recommendation system as it has the 2nd lowest rmse score and not using SVDpp because of its very high fit time. I did not used 

Algorithm	test_rmse	fit_time	test_time
SVDpp	0.933328	112.691523	3.524218
KNNBaseline	0.940745	0.468465	5.050569
BaselineOnly	0.950452	0.226177	0.271529
SVD	0.952169	3.742409	0.403681
SlopeOne	0.954150	0.414259	2.713668
KNNWithMeans	0.962369	0.365869	4.644054
KNNWithZScore	0.962999	0.411534	4.942302
NMF	0.984970	4.443949	0.286411
CoClustering	0.985725	2.077911	0.275899
KNNBasic	0.995991	0.317620	4.098837
NormalPredictor	1.513971	0.104124	0.241279

3)	Built a model: I am using the KNNBaseline model and doing some hyper parameter tuning on it using Grid-Search CV and  this hyper parameter tuning will take various parameters and will built a model based on the best possible combination of those parameters. It will find the metrics that we want to evaluate and will give the best estimator by using gs.best_estimator[‘rmse’]. 
•	Below are the parameters that I am passing in Grid-Search CV
•	The K-value in the range of 10 to 100, to detect the best set of nearest neighbours
•	Computing all the similarities like cosine, pearson, pearson_baseline and msd similarity to check the similarity between all the pairs of users (or items) 
•	min_support as 5, which will discard all the users who has rated less than 5 times

4)	Fitting the model: Once the model is built with the best estimator using Gird-Search CV, I am fitting the model using .fit() method of Surprise library and training the model on the training dataset. data.build_full_trainset() will build the model on whole trainset. 

5)	Predicting the ratings: Once the model is trained on the trainset, I am predicting the ratings by providing UserID-ItemID pair from Test.dat file to the .predict() method of the Surprise library and fetching the estimated value of ratings by using pred.est command. Finally, replacing the 1s in the format.dat file by the estimated value of the ratings. The RMSE score obtained is of 0.9211.
