# automated_ad_placements

### Some assumptions I made in selecting the model:
- Budget and start/end time have no effect on CPM.
- Clients will come in with a specific Marketing Goal, Campaign ID, and Video/Non video ad (I’ll call this MCV). The model will only optimize for the best combination of Channel ID, Customer Segment ID, and Location (I’ll call this CCL).

### Some assumptions to help validate my model:
- Similar combinations in MCV and CCL give similar CPMs.

## Methodology:
1) Train the model using Marketing Goal, Campaign ID, Video Campaign, Channel ID, Customer Segment ID, and Location as explanatory variables, and CPM, CPV, CPA, and CPV as response variables.
2) To predict the best combination of CCL for a given MCV combination (that the client comes to you with), first come up with every possible combination of Marketing Goal, Campaign ID, and Video/Non video ad. Create a fake dataset out of this.
3) Feed this fake dataset into your model and predict the CPM for each of the combinations.
4) The model’s recommended CCL is the combination that gives the lowest predicted CPM.
5) To see how much better the recommended combination performs compared to other combinations, we first need 2 things:
i) Find a row in the real dataset which has the exact same combination of MCV and CCL as the best combination that we found in the fake dataset. Get its CPM. If there isn’t an exact match, just use the predicted CPM of the best combination.
ii) Now we have to get the CPMs of suboptimal combinations, to compare to the best combination. We find all rows in the real dataset which have the same combination of MCV that the client asked for. The other columns will be different combinations of CCL. Then we average the CPMs of those combinations.
6) Then we compare the CPM of the best combination with the average CPMs of the suboptimal combinations. Calculate how many of the suboptimal combinations the best combination’s CPM is lower than as a percentage. For example - “The best combination has a CPM lower than 98% of the other combinations”. This could be used to compare models.
7) To update the model over time, just add the real results of the recommended combinations once the campaigns are finished. Retrain the model on the new, larger dataset.

### Things to note:
- Because this is just a POC, I've only looked at recommending a marketing mix to reduce CPM. But it'd be easy to add functionality to take into account CPA, CPV, etc.
- There is no definitive way to prove that my model is superior, because there is nothing to compare its performance to. You'd have to do an A/B test for that. Instead, I've used a hack - compare the ROI of a campaign (from the training set) similar to the one recommended by my model to the ROI of other campaigns. If the similar campaign has a lower CPM than the other campaigns, then you can assume my model's recommendation will also perform better than other campaigns, because they are similar.
- But there's another problem - since the dataset is just random data (each column has values pulled from a uniform distribution), the ROI of the campaign recommended by my model will usually be close to the average of the ROI of other campaigns. I can't prove my model is a good approach unless I have real data.
- Since the model was trained on random data, it can't find any real patterns in it. The Mean Squared Error of the model is ~157, which implies that on usually, its predictions are off by $12, which is too large for the predictions to be of any use. If I just get the mean of all the costs in the training data and predict that value for the test set, my MSE will be ~550, which implies that the predictions are off by $23. So my model isn't that much better than just predicting the mean of the training set values. But again, this problem will go away if the model gets real data to train on.
