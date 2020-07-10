---
layout: post
title: How to build a recommendation engine based on Twitter?
---

In 2020 with everything going on plus the polarization in this country, it’s nearly impossible to find someone who doesn’t have an opinion (good or bad) on the President of the United States. However, this year it’s become more clear how important local politicians and other elected officials are to our day-to-day lives. President Barack Obama said it best in his [recent Medium post](https://medium.com/@BarackObama/how-to-make-this-moment-the-turning-point-for-real-change-9fa209806067) discussing the George Floyd killing.

> ...the elected officials who matter most in reforming police departments and the criminal justice system work at the state and local levels

Arguably this is the same for most issues that we care about. For instance, if you don’t like how Covid-19 was handled, part of the blame may be on your local officials. Because of this, for my passion project at Metis I decided to build a recommendation engine that would recommend local politicians to vote for in your Bay Area county. At the very least, I wanted to help myself be a more informed local voter!

### Data Collection

For my third project, I predicted which political party people will vote for based on how they answer personal questions. Initially, I thought I would recommend local politicians based on the same dataset but we don’t know how politicians would answer these questions. In other words, the dataset I wanted didn’t really exist. These days, Twitter represents an easy way to quickly grab data on each politician's beliefs. Thus, I decided to generate my own dataset and find similarities by comparing Twitter profiles. I used the [GetOldTweets API](https://github.com/Mottl/GetOldTweets3) to scrape tweets for all of them. Below is an example of an API call I ran in Command Line:

```GetOldTweets3 --username "SpeakerPelosi" --since 2019-03-03 --until 2020-03-03 --maxtweets 1000```

So there were 108 politicians running for either State Senate, State Assembly or the U.S. House of Representatives on March 3rd, 2020. Only 61% of those politicians had a “good” Twitter profile and had more than 10 tweets in the year preceding the election. Also 83% of democrats had a “good” profile and only 30% of Republicans could say the same. Because of this, I decided not to go more “local” with politicians as the percentage of them with usable Twitter profiles would probably be even worse. So we’re already seeing the limits of using Twitter and on focusing on the Bay Area. Here the Democrats tend to dominate the elections and this model will be biased towards picking Democrats as a result. Keep this in mind for later!

### Vectorization

Next, I cleaned the data and then used TF-IDF to create a document-term matrix. This tells us how often a term appeared in each document (i.e. tweet). 

```
tfidf = TfidfVectorizer(stop_words=custom_stop_words,ngram_range=(1,3), min_df = 5, max_df=.9, binary=True)
doc_word = tfidf.fit_transform(result)
doc_word_df = pd.DataFrame(doc_word.toarray(),index=df.drop_duplicates(),columns=tfidf.get_feature_names())
```

Afterwards, I summed up the columns of the matrix to get a one row politician-term vector. Essentially, this tells you often a politician uses various words because higher values for a word will mean they used that word across many tweets. 

```
#sum up all columns
data = np.zeros((1,len(doc_word_df.columns)))
for i,column in enumerate(doc_word_df.columns):
	data[0,i] = doc_word_df[column].sum()
    
#put the sums into a new dataframe
final = pd.DataFrame(data,index=[twitter_handle], columns=doc_word_df.columns)
```

The final result is what you see below:

![pelosi_vector](/images/pelosi_vectors.png)

Once we add other politicians the matrix looks like this:

![all_vectors](/images/all_vectors.png)

### Sentiment Analysis

So now we have vectors for all politicians and can use similarity metrics like cosine similarity or euclidean distance to figure out who is the most similar. I tested the recommendation engine at this point and it did relatively ok but I did notice one worrying result. If you looked at the values for a controversial topic like guns, you’ll see something like this:

![all_politicians_guns](/images/all_politicians_guns.png)

According to this, Scott Weiner and DeAnna Lorraine used the word gun the most. If you knew nothing about these politicians you might think they were similar but from a quick glance at the tweets we see very different sentiments:

#### Scott Weiner

![scott_weiner_tweet](/images/scott_weiner_tweet.png)

#### DeAnna Lorraine

![deanne_lorraine_tweet](/images/deanna_lorraine_tweet.png)

As you can see very different beliefs on guns! There is definitely a need to add sentiment analysis here to try and find the difference between these two types of tweets. But how should we add sentiment analysis? I experimented and realized adding a general sentiment column is not good enough given that the politician term vectors have thousands of columns. So I decided to take the top words that each politician used and find the sentiment for each one and input a new column for each word_sentiment in the politician-term vector. However, since I used VaderSentiment for this the sentiment analysis output is actually 4 things: a measure of positive sentiment, negative sentiment, neutral sentiment and compound (i.e. total) sentiment. So the final result is I added 4 new sentiment columns per top word. Below you can see a visualization created using Principal Component Analysis showing where these vectors would lie in a 2D space. 

![2d_w_sentiment](/images/2d_w_sentiment.png)

### Similarity

Great, now we are ready to make some recommendations. What similarity metric should we use to provide them? This answer was pretty simple. Since tweets have a variable length, I didn’t want that to play a part so we use cosine similarity rather than euclidean distances. By experimenting with both, the results also show that this is a better method.

### Final Result

For my specific use case, I wanted to give recommendations for any inputted twitter handle and recommend the top choice in each political contest in a county. Below you can see the results of the model when I inputted Joe Biden’s twitter feed and San Francisco as my county. 

![recommendations](/images/recommendations.png)

Voila! We have built a recommendation engine for Bay Area politicians! I really hope this was useful and if you have any questions you can reach me at samirthanedar@gmail.com. In the meantime, please VOTE this November!

For access to all the code for this project you can go Github [here](https://github.com/samirthanedar/Recommending-Local-Politicians). You can also test the recommendation engine [here](https://politician-recommender.herokuapp.com/). 