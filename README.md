# RedditIdeology-Honours2021
Files for scraping and modeling reddit users' ideologies as a function of digital footprints.

Any file in this repo that is not mentioned in the below list is obsolete and was not used in the final project.

## Data Collection

1. *user_flair_scraper_draft.py* this script goes through the Political Compass Memes subreddit and records the flair and usernames of flaired users who have commented in the top 1000 most popular posts, it gives us our list of username and associated ideolog.
2. *user_history_scraper_draft.py* this script loops through each username in the list of usernames and ideologies produced by user_flair_scraper_draft.py and logs the ammount of times each user has posted or commented in a specific subreddit.
3. *data_manipulator_complete.py* this script pivots the data file produced by user_history_scraper_draft.py so that each row represents a user, each column a subreddit and each cell how much the relevant user comments/posts in the relevant subreddit, we merge this with our list of users and flairs so that we have a complete data set that can be use for prediction.
4. *user_corpus_scraper.py* this script goes loops through each username in the list of usernames and ideologies produced by user_flair_scraper_draft.py and records the textual content of (a maximum of) 100 comments from each user (with the additional criterion that the comments not be made in the Political Compass Memes subreddit). We process this data to gain features to use in the NLP based models.
5. *text_manipulator.py* this script takes the data generated by user_corpus_scraper.py and generates three seperate data files: 1) contains a list of users, their flairs and meta data regarding their comments (min length of comment, max length of comment, average length of comment, unique words in comment, SMOG readability, etc.), 2) contains the cleaned and tokenized comments for each user (where each users comments are concated into a single cell for the user) we extract features from this via TF-IDF, 2) contains comments for each user concated into a single cell. 

## Models

**In the main folder**

1. *all_class_int_models.py* this script trains, optimizes and validates models designed to predict the ideology of users based on the user-interaction matrix produced by data_manipulator_complete.py (see the Data Collection folder)and produces a csv file that records the exact specification of each model and its accuracy and AUC on a testing set.
2. *econ_int_models.py* does the same but with the task of predicting the economic element of the user's ideology from the user-interaction matrix.
3. *social_int_models.py* does the same but with the task of predicting the social element of the user's ideology from the user-interaction matrix.
4. *binary_econ_int_models.py* does the same but omits economic centrists; predicts if users are economically left or right from the user-interaction matrix.
5. *binary_social_int_models.py* same as above but omits social centrists; predicts if users are socially authoritarian or libertarian from the user-interaction matrix.

**In the NLP subfolder**

1. *all_class_nlp_models.py* this script trains, optimizes and validates models designed to predict the ideology of users based on the textual features produced by text_manipulator.py (see the Data Collection folder)and produces a csv file that records the exact specification of each model and its accuracy and AUC on a testing set.
2. *econ_nlp_models.py* does the same but with the task of predicting the economic element of the user's ideology from textual features.
3. *social_nlp_models.py* does the same but with the task of predicting the social element of the user's ideology from textual features.
4. *binary_econ_nlp_models.py* does the same but omits economic centrists; predicts if users are economically left or right from textual features.
5. *binary_social_nlp_models.py* same as above but omits social centrists; predicts if users are socially authoritarian or libertarian from textual features.

**In the Combined subfolder**

1. *all_class_comb_models.py* this script trains, optimizes and validates models designed to predict the ideology of users based on the user interaction matrix produced by data_manipulator_complete.py and textual features produced by text_manipulator.py (see the Data Collection folder)and produces a csv file that records the exact specification of each model and its accuracy and AUC on a testing set.
2. *econ_comb_models.py* does the same but with the task of predicting the economic element of the user's ideology from the user interaction matrix and textual features.
3. *social_comb_models.py* does the same but with the task of predicting the social element of the user's ideology from the user interaction matrix and textual features.
4. *binary_econ_comb_models.py* does the same but omits economic centrists; predicts if users are economically left or right from the user interaction matrix and textual features.
5. *binary_social_comb_models.py* same as above but omits social centrists; predicts if users are socially authoritarian or libertarian from the user interaction matrix and textual features.

## Results

1. *all_int_results.csv* contains a record of each model developed in all_class_int_models.py, its accuracy and auc on training set.
2. *econ_int_results.csv* contains a record of each model developed in econ_int_models.py, its accuracy and auc on training set.
3. *social_int_results.csv* contains a record of each model developed in social_int_models.py, its accuracy and auc on training set.
4. *binary_social_int_results.csv* contains a record of each model developed in binary_social_int_models.py, its accuracy and auc on training set.
5. *binary_econ_int_results.csv* contains a record of each model developed in binary_econ_int_models.py, its accuracy and auc on training set.
6. *all_nlp_results.csv* contains a record of each model developed in all_class_nlp_models.py, its accuracy and auc on training set.
7. *econ_nlp_results.csv* contains a record of each model developed in econ_nlp_models.py, its accuracy and auc on training set.
8. *social_nlp_results.csv* contains a record of each model developed in social_nlp_models.py, its accuracy and auc on training set.
9. *binary_social_nlp_results.csv* contains a record of each model developed in binary_social_nlp_models.py, its accuracy and auc on training set.
10. *binary_econ_nlp_results.csv* contains a record of each model developed in binary_econ_nlp_models.py, its accuracy and auc on training set.
11. *all_comb_results.csv* contains a record of each model developed in all_class_comb_models.py, its accuracy and auc on training set.
12. *econ_comb_results.csv* contains a record of each model developed in econ_comb_models.py, its accuracy and auc on training set.
13. *social_comb_results.csv* contains a record of each model developed in social_comb_models.py, its accuracy and auc on training set.
14. *binary_social_comb_results.csv* contains a record of each model developed in binary_social_comb_models.py, its accuracy and auc on training set.
15. *binary_econ_comb_results.csv* contains a record of each model developed in binary_econ_comb_models.py, its accuracy and auc on training set.
16. *svd_k.csv* contains the original variables that contribute most to the first 10 SVD components, it is produced by k_analysis.py.
17. *results_summary.xlsx* overview of results from the other files that can easily be converted to LaTex tables using Excel2Latex

## EDA

1. *create_eda_data.py* this script produces 1) a version of the user-interaction matrix produced by data_manipulator_complete.py with most columns removed so that it can be easily loaded into memory and used to create some visual analyses of the relationship between ideology and subreddit interaction 2) a grouped and summarised version of the user-interaction matrix for the same purpose.
2. *interactions_eda.R* this script uses the data file produced by create_eda_data.py to create visual analyses of the relationshup between ideology and subreddit interaction.
3. *k_analysis.py* this script uses the user-interaction matrix to 1) produce a graphic of the accuracy of predictions using a varying number of SVD components and 2) produce svd_k.csv.
4. *result_int_viz.R* this script takes the all_int_results.csv, econ_int_results.csv, and social_int_results.csv files and produces graphics illustrating the predictive performance of all the models developed in all_class_int_models.py, econ_class_int_models.py and social_class_int_models.py.
5. *create_svd_data.py* this script just computes the SVD components of the training set for user-interaction matrix models and exports them to csv so that I can create plots in R showing the data in SVD space
6. *create_tf_idf_viz_data.py* this script computes the tf-idf scores for different words used by users in their comments and exports this dataframe to a csv so I can create plots showing the frequency of different words in R
7. *response_eda.py* this script creates a plot of the proportion of each ideological class in our data set
8. *svd_viz.R* this script creates plots of the data in SVD component space
9. *word_clouds.R* this script creates plots of word frequncy and various word clouds

## Write up

This folder contains notes to myself that represent intermediate progress on the final write up and is not relevant to the final thesis.

## Proposal

This folder contains some files used in the project proposal and project presentation in semester 1 and are not relevant to the final thesis.



