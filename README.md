# I'm Going to Disney World!
Here is my second major project for my Coding Dojo Data Science program. 

# Background: 
I worked with a small dataset I retrieved from [data.world](https://data.world/lynne588/walt-disney-world-ride-data/workspace/file?filename=WDW_Ride_Data_DW.xlsx) to analyze what it is that makes a ride enjoyable, i.e., rated highly, by its guests. I hope that through this project, I can explore and spot trends that can be used for future attractions and extend to other theme parks. 

A concern regarding this project would be that it may be hard to generalize findings due to the small data set (46 rows in total). I identify some high correlations and deal with them appropriately. In addition, this dataset is from 2018, so many rides have since opened and are not present. I correct time-related changes in later steps (i.e., age of rides updated to 2021).

## Goal: 
Analyze the rides of Disney World and create a model that can predict features of a highly rated amusement park ride.

## My connection: 
As a Florida resident, rollercoaster enthusiast, and Annual Passholder at Walt Disney World, I chose to investigate a topic I hold dearly; amusement park rides.



# Data Dictionary:
The original 27 columns of the dataset

![image](https://user-images.githubusercontent.com/86759538/134617993-dd70df18-d213-456b-9891-0cc0e5b14562.png)


# Data Cleaning Summary:
* Filled missing data in TA_Stars Column
  * Ride "Slinky Dog Dash" has since been rated on Trip Advisor's Website
  * Ride "Alien Swirling Saucers" was filled by estimating the average of similar rides (spinning).
* Dropped rows with missing values in TL_Rank Column
  * These rides no longer are at the parks, which is likely why it didn't have a rank on Travel & Leisure. 
* Corrected typo in "Ride_type_all" column
  *  "thirll, small drops” to "thrill, small drops"
* Dropped 'Age_interest_kids' as every row was a "Yes"
  * This was a fixed variable and therefore could be removed
* dtype for TL_Rank was erroneously a float rather than an integer
  * changed dtype to integer
*  Changed "Yes" and "No" inputs into binary (1, 0)
*  Dropped columns which were repetitive ('Age_of_ride_days', 'Age_of_ride_total', 'Open_date')
  *  They all represent in some way the age of the ride
  *  Kept Age_of_ride_years as a column to hold this value
  *  Added 3 to the Age_of_ride_years column to update it from 2018 to 2021

# Data Visualizations:

## Heatmaps
![heatmap ages](https://user-images.githubusercontent.com/86759538/134619036-a6ef04b9-f7cf-4608-9087-b6120bfa5eef.png)

The above is a heatmap that shows the correlations between the different columns. 

![heatmap no ages](https://user-images.githubusercontent.com/86759538/134619074-ed7f8da7-4acc-4fc7-be6a-4d1e1c85892b.png)

The above is a second heatmap with the ages removed, so it is easier to focus on other columns' relationships.

### Summary of heatmaps' correlations:
In the heatmap above, the following becomes visualized:

**Positive Correlations**
* *1.0 (or visually equal) correlations* for all the age interests
  * This is because so many of the rows are one value (Yes/1)
  * Although it may be wise to drop/remove these columns, I will keep them, for now, to see how they may affect machine learning models in the future, creating a separate dataset without these types of columns as an option to compare.
  * Our dataset is smaller, so we will want to keep them, as just one "No"/0 is 1/44, or ~ 2% of that category
* The next *high*est correlations are that of Ride_type_slow & age_interest_preschoolers
  * Suggests that preschoolers like slower rides (low-thrill)
*  Height requirements & thrill rides also *higher* correlation
  * Thrill rides often require higher height requirements, as they are not appropriate for small children for safety and enjoyment reasons
* *Moderate Correlation* in Classic & age_of_ride_years 
  * makes sense; a classic ride is going to be older (higher in age)
* Appearing as a *moderately positive* correlation: 
  * Preschoolers vs. TL_rank is slightly positive

_________________________________
*The relationships between TL_rank are complex; they do not seem to act like we expect them to. It may work the opposite of how we would want. The computer may be expecting a low rank to be like low stars and vice versa, which creates an opposite correlation.*
_________________________________

**Negative Correlations**

* *Highly negative* correlation suggests inverse relationships
  * Height requirements & ride_type_slow
    * slower (low-thrill) rides have no or low height requirements
  * Height requirements and interests of preschoolers are *highly* negative
    * Preschoolers are shorter; therefore, they will only have interests in rides that have lower height requirements, as that's the only ones they can go on!
  * Related to the last point, thrill ride is *highly* negatively related to preschoolers
    * Preschoolers are too young to go on thrill rides
*  *Moderately Negative*
   * Thrill ride is negatively related to both slow rides & preschoolers
     * Slow rides are the opposite of thrill rides
     * Preschoolers are too young to go on thrill rides
    * Fast pass & TL Rank
       * Rides with a fast pass may have a certain rank (high rank)
    * Fast Pass  & ride_type slow
      * Slow rides probably don't have a fast pass


**TA_Stars**
* *Negative Relationships:* 
  * TL_Rank 
    * Stars & Rank ARE opposites
     * Lower rank is the better outcome, where higher stars are the better outcome; they are inversely proportional (expected)
  * Preschoolers
    * The kinds of rides preschoolers really like aren’t as likely to be rated high. 
  * Slow Rides
    * Suggests slow rides have a lower star rating 
* *Positive Relationships:*
  * Thrill Rides
    * Suggests thrill rides have a relationship with a higher star rating
  * Big Drops
    * Suggests big drops have a relationship with a higher star rating

## Scatterplot
![rank vs star](https://user-images.githubusercontent.com/86759538/134619331-d06ee5c4-96c8-4a5e-805e-2598eb4a9526.png)

This scatterplot compares rides' ranks against its star ratings. The ranking is just one person’s perspective since it was in a written article, vs. stars which are an average score of many users on Travel Advisor’s site. This difference is demonstrated by the downward trend yet also why the top-ranked aren’t necessarily five-starred. 

## Bar plot
![ride ratings](https://user-images.githubusercontent.com/86759538/134619491-430180b6-c18a-4387-a6c1-470212c78880.png)

The bar chart displays the total number of rides with each star rating. 

## Box plot
![thrill vs non thrill](https://user-images.githubusercontent.com/86759538/134619583-8f5df293-16bd-4a52-ad1f-7e9bd2c397f7.png)

My last visualization is a boxplot of thrill vs. non-thrill rides’ star ratings. Thrill rides generally have a higher star rating than non-thrill rides. Each category has one outlier. The 25% and 50% are the same star rating within both types: 4.5 for thrill and 4.0 for non-thrill.


# Pre-processing of Data:
## Dropping Columns
* We already have each ride type and age interests in separate columns, so Ride_type_all and age_interest_all are repetitive. I dropped them for all data frames.
* Ride_name is made into the index, rather than dropped

## Dummies
'Park_location' and 'Park_area' values are extended into individual columns. I initially keep all (don't drop first). I do this for all data frames.

## Rework Target
As it was, the target (TA_Stars) for the model has five options: 5.0, 4.5, 4, 3.5, and 3. Rather than creating a model that looks at each star rating, I turned it into a binary classification problem, which allows it to be more targeted.

I split the target into "High" AKA 1 (5.0 and 4.5) or else "Low" AKA 0 star classes; once doing this, I converted the column to integer dtype rather than float.

## Dataframes
I ended up creating three separate data frames to compare model evaluations:
1) df: The first data frame was the cleaned data.
2) no_ages_df: The second was the cleaned data but with all age_interest type columns removed 
3) df2: I made this third data frame after starting the models on the former data frames; when I recognized that many of the features had importance values of 0 (essentially held no effect on the target, stars), I created this data frame built off of the first, but without any of the zero importance columns ( 'Age_interest_tweens', 'Age_interest_teens', and 'Age_interest_adults').


# Machine Learning

## Baseline Model
The baseline model is if we just assumed the majority class was everything. It is the same for all of our data frames: 53.3%.

## Models
I experimented with a handful of models to test what worked best for the data: Random Forests, K-Nearest Neighbors (KNN), Logistic Regression, and three Gradient Boosters (eXtreme Gradient Boosting, Light GBM, Gradient Boosting Classifier). I also used VotingClassifier as well as GridSearchCV to ensure optimization and the best performers. 

### Determining Feature Importances
![image](https://user-images.githubusercontent.com/86759538/134757588-12c290c6-2c3a-4e60-ad81-6a5ae0188762.png)

I created this bar chart to evaluate the model’s features’ importance to develop the third data frame using Random Forests. It was evident there were features with no significance whatsoever, which allowed me to look into it further to determine what should stay and go. I could fully identify all of the null feature columns utilizing the first data frame, as it had all of the columns.

## Models continued

In my third/final data frame, I created the best-performing model using a “Soft” Voting method on the non-tuned models. I narrowed it down to two models, which performed the best to use in the Voting Classifier and GridSearchCV: Random Forest and eXtreme Gradient Boosting. The models' testing accuracy did not change in performance after using GridsearchCV.


![image](https://user-images.githubusercontent.com/86759538/134757601-2fe0a36e-10a8-40bb-b353-663d54f035ec.png)
![image](https://user-images.githubusercontent.com/86759538/134757615-9f685335-a6d9-4f91-ae55-6710871f8660.png)

Grid Search and Soft Vote Confusion Matrix.

![image](https://user-images.githubusercontent.com/86759538/134757386-cdf1281e-29f8-4465-b02c-de6c2e4dc22d.png)

Classification Report of best model performance.

![image](https://user-images.githubusercontent.com/86759538/134757416-4f013477-bca8-4933-bc47-e296a1c43b15.png)

The previous confusion matrices are the same as the XGB.


## Further Explanation of Recall/Precision/F1 Scores
The recall defines how many of a particular class is correctly found, so it correctly identified about 88% of what was low starred for low starred rides. For the high starred rides, the model could only identify ~57% of the high starred rides correctly.

Precision regards how many of the predictions for a particular class are right; the predictions for low starred was 70% correct, and for high star predictions, it was 80% correct.

The f1-score is akin to the mean between precision & recall, similar to a type of accuracy, but for each target. The average of the two f1 scores is the overall accuracy. Therefore, the model was 78% accurate for the low starred category and 67% for the high starred class. These two values lead to an overall accuracy of 73%.

# In Conclusion
I created a model that, with 73% accuracy, can predict whether a ride at Walt Disney World is highly rated or lowly rated based on its features. 

The model itself can confidently identify what features go into a low-starred ride. eXtreme Gradient Boosting (XGBoost) is the model that performed the best. XGBoost is more skilled at picking out the lower starred class and is only slightly better than baseline in differentiating high starred rides. By observing what features play into these sortings, theme park executives can get an idea of what new rides should have for the most enjoyment of consumers. I am confident that with more data (more rides), we could drastically improve this model.
