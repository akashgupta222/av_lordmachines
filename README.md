# av_lordmachines
Solution to AV Lord Of Machines competition held on AnalyticsVidhya.com

## Problem
The problem involved predicting whether any link in an email campaign will be clicked or not. 
The train file had information regarding when was the email sent, user id and campaign id and whether the email was opened and clicked. 
There was a campaign description file mapping the campaign id's to the subject, body, no of images, etc 

## Modelling
I posed it as a problem of sequence prediction where we want to find whether a user will click on an email, given his past interactions on platform.
The first thing that comes to mind when we think of sequence prediction problems is RNN or more specifically LSTM. 

## Features
I formed sequences of users' actions (in form of clicked and opened). 
4 sequences were formed: 
- Clicked (0s and 1s)
- Opened (0s and 1s)
- No of sections in each email
- No of images in each email 

These sequences acted as 4 features for sequential input. 

## Network Architecture(s)
- I started with LSTM model followed by a couple of dense layers and tuned its optimisers and no of cells in the LSTM layer. 
- I added a CNN model followed by a GlobalMaxPool followed by a couple of dense layers and tuned its architecture. 
- Finally I added a CNN layer followed by a LSTM layer followed by couple of dense layers as the third model. 

## Prediction and Cold Start
- The output of these models gave me a probability whether these users will click the next email or not. I used this probability across all emails sent to that user 
(I did not want to add prediction to sequence and predict again because that can cause errors to propagate further). 
- This allowed me to make predictions for the users for whom we have some data (previous behaviour), but in the test set, 20% of the entries were for users for whom we do not have any data (aka cold start).
- To deal with Cold Start, I grouped by campaign_id and sent_weekday and sent_quarter_of_day and filled the missing values by 90% quantile across each group. 

## Ensemble
These models were tuned independently and a added to ensemble via simple rank average.  

