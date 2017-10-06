# automated_ad_placements

Things to note:
- Because this is just a POC, I've only looked at recommending a marketing mix to reduce CPM. But it'd be easy to add functionality to take into account CPA, CPV, etc.
- There is no definitive way to prove that my model is superior, because there is nothing to compare its performance to. You'd have to do an A/B test for that. Instead, I've used a hack - compare the ROI of a campaign (from the training set) similar to the one recommended by my model to the ROI of other campaigns. If the similar campaign has a lower CPM than the other campaigns, then you can assume my model's recommendation will also perform better than other campaigns, because they are similar.
- But there's another problem - since the dataset is just random data (each column has values pulled from a uniform distribution), the ROI of the campaign recommended by my model will usually be close to the average of the ROI of other campaigns. I can't prove my model is a good approach unless I have real data.
- Since the model was trained on random data, it can't find any real patterns in it. The Mean Squared Error of the model is ~157, which implies that on usually, its predictions are off by $12, which is too large for the predictions to be of any use. If I just get the mean of all the costs in the training data and predict that value for the test set, my MSE will be ~550, which implies that the predictions are off by $23. So my model isn't that much better than just predicting the mean of the training set values. But again, this problem will go away if the model gets real data to train on.

Assumptions:
- Time and budget have no effect on CPM