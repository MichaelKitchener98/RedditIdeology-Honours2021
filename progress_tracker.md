# Progress Tracker 

## Tuesday March 30

* Created user_flair_scraper_draft.py, a script which goes through the most recent posts on the PCM subreddit and looks at each comment. For each comment it records the username and user flair of the comment's author (if the comment has a flair). It saves these to a csv. 

* Created user_flair_scraper_draft.py, a script that contains a function that can record the entire post and comment history of a specified reddit user. We can loop through a list of users to create a csv file that contains the entire post/comment history of multiple users. Each row of the output csv has the following columns:
* username | interaction_type (post or comment) | title (if it is a post) | body (text) | score (of the post) | time (of creation) | subreddit


## Thursday April 1 

* Github repo created, user_flair_scraper_draft.py and user_history_scraper_draft.py uploaded to repo

* To get a sense of how long it takes to scrape user names and flairs from the comments on the posts in the PCM subreddit, I ran user_flair_scraper_draft.py  several times, each time specifying that it stop after collecting usernames and flairs for a certain number of users.
* The results are as follows: 
    * 1000 users | 71.79 seconds
    * 2000 users | 114.6 seconds
    * 3000 users | 215.34 seconds
    * 4000 users | 293.65 seconds
    * 5000 users | 368.24 seconds
    * 6000 users | 558.32 seconds

* *update:* the time is definitely non-linear since the further we go, the more likely that a comment's author is already in our list. Hence there are dimishing returns to time.
    
* Updated user_flair_scraper_draft so that it scrapes 1000 user/flair pairs at a time and then adds each user/pair as a row to an existing .csv file. This way, if the computer doing the scraping is turned off while the script is running or something else terminates the script, most of the scraped data will still be saved and available.

## Friday April 2

* Started running the updated 'user_flair_scraper_draft.py' scraper. The limit of user observations to record is 500,000. I think it is unlikely that this number will be reached (there may not even be this number of flaired users who have made comments). Fortunately, the script saves every 1000 observations to a .csv so it will be no huge loss if some limiations cause an error and the script to stop running. I plan to keep this running over the break. In the meantime, I will start to get stuck into the literature review. 

* I have had some issues with the scraper. It turns out that PRAW has a limit of 1000 requsts. as such the 'posts in pcm.new(limit=None)' that we are looping through in order to view the comments (and by extension, the author of the comments and the author's flair). I have a few ideas that may allow us to work around this problem:
   1. ~~Nest the 'for post in pcm.new(limit=None)' loop (the loop that goes through most recent posts) within a 'while len(users_unique) < 500000' loop (a loop that starts the for loop again if we have less than the desired ammount of users). One issue here is that there are likely not many new posts with lots of comment (from unique authors) being created inbetween running the initial for loop and running it again. As such this could be very slow as each iteration of the foor loop may yeild a few new comments from flaired users who we have not already recorded.~~ As predicted, this is pretty intensely slow. 
   2. ~~At the moment the script only loops through first level comments (comments that are direct replies to the post). Obviously there are many more comments that branch through these comments that may be authored by flaired users who are not currently in our .csv file. As such the script should be ammended to go through ALL the comments of a post in search of unrecorded user/flair combinations. This approach is applicable no matter what list of posts we end up searching through. I am not sure how this will impact the time given the various request limits.~~ Already done 
   3. Instead of going through the most recent posts, we can loop through the top (most popular) posts of all time (or of the week, month, year, etc.) These will presumably have a greater number of comments on them. As such the unique user/flair pairs yeilded from 1000 of the best posts of all time is likely to be much higher than through looping the 1000 most recent comments.
   4. We can loop through new comment, the top comments (of the week, year, etc.) seperately and then merge the files together. There will undoubtably be duplicates but it will be easy to remove duplicates from the merged data using R or Python.

* I will also look into alternative technologies, i.e. pushshift.io if this doesn't work.

* I am currently pursuing option 3. This seems to be doing reasonably well. So far I have collected 60,000+ user/flair pairs. I will try option 4 if this doesn't yield sufficient results by the end of the loop. 

* I have updated the 'user_flair_scraper_draft.py' script in the repo. Now it loops through the top 1000 posts of all time (prior, it looked through new posts). However, since we are limited to 1000 posts it makes sense to look through the top posts since they likely have more comments. For each of these posts it looks through the maximum ammount of comments that PRAW will allow  (1000 I think) and check the author of each comment. If the author is flaired and isn't already in our data set it records the author's username and flair. I also changed the script so that it provides updates on how many user/flair combinations have been saved every 100 user/flairs - this is not a material difference, just cosmetic. Finally, I got rid of the loopbreaker since it's unecessary - the loop will end once the  script has gone through 1000 comments in each of the top 1000 posts of all time. 
* *update:* accidentally uploaded this file with a different name - working on merging them and getting rid of the duplicate now. 

* I have updated the 'user_history_scraper_draft.py' file. Now it works like the flair scraper in that for every user we pass into the scraping UserData function, it collects this users info, updates the .csv and then deletes that users info before moving onto the next user. This is more effecient in terms of working memory. Further, this means that the process doesn't have to be all done at once. We can simply do it for a set number of users at a time and it will continue to update the file. 

* I have created an .R file called 'user_footprint_matrix_script.R'. This script takes the output of the user_history_scraper and outputs a dataframe where each row represents a user and each column represents a subreddit. There are different code chunks.
   1. In the first one, each cell represents how many comments/posts were found in the relevant subreddit for that user.
   2. each cell representst whether or not the user has commented/posted in that post at all (binary: 0/1).
   3. each cell represents the total karma (score) of the users' posts/comments in that subreddit.
   4. each cell represents the average karma (score) of the users' posts/comments in that subreddit.

* I might also create code chunks to have seperate columns for posts and comments. 

## Saturday April 3

* The user_flair_draft_scraper.py finished going through the top 1000 posts of all time. In total, we scraped 91,000 username and ideology (flair) combinations.

## Tue May 4

* Last few days spent creating the reserach proposal and accompanying slides.
* See project proposal for an outline of the project's intended contribution
* Have been continuing with the literature review.
* Created script to clean up the flair data (merge duplicate flair types for the same ideology) and report descriptive stats for ideology + viz

## Tue Jul 7

* I have started writting up a draft of some sections of the final decision paper, the introduction, the literature review, an explanation of the data, etc.
* I have also created a script to transform the full record of user interactions into a 'user interaction matrix' (non-trivial as just trying to pivot the data is impossible due to limitations with working memory) - uploaded as 'data_manipulator_complete.py'
* I am playing around with different models on a small subset of the data ~3.5k observations, nothing amazing so far
* Next, I will go through the script to scrape user interactions and check whether or not has 'censored' any data.
* I will also investigate ways to scrape the full list of user comments 
