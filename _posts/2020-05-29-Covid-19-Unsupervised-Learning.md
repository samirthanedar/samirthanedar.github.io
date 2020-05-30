---
layout: post
title: How is the public coping emotionally with Covid-19?
---

Covid-19 needs no introduction. As a country, we are over two months into a scary pandemic. But this has been silently [potentially spreading worldwide since December, 2019. Luckily my family and I have been spared from the worst of the disease and we’re all healthy. However, as the infectious disease progressed, I've felt strong emotions and worried about the future. Once the government shutdown started across the country, I felt a lack of control and complete uncertainty for what would happen next. Although this was anecdotal, I wondered how others were coping mentally and emotionally with Covid-19. 

To complete this analysis, I scraped Twitter using the GetOldTweets API and got 75,000 tweets related to the search query “coronavirus”. From there, I spent time doing various preprocessing like removing URLs, removing punctuation, putting everything in lower case, lemmatization and removing stop words. Then I used TF-IDF (term frequency–inverse document frequency) to vectorize the words and finally NMF (non-negative matrix factorization) to do topic modeling.

Below you can see the top 10 topics and the top words in each of them in no particular order:

**Topics** | **Words**
------------ | -------------
Initial Chinese Outbreak | china, outside, outside china, wuhan china, travel, sars, hubei, flu, pneumonia
Trump’s response | trump, president, response, american, trump administration, trump response
Italy’s Covid-19 Outbreak | report, italy, china report, bring, italy report, report bring, break, hubei
Stopping the spread | spread, stop, stop spread, prevent, prevent spread, country, slow, slow spread, cdc
Anger | A whole lot of curse words
Working from home | work, employee, worker, hard, school, work together, company, office
Cruise and Quarantines | quarantine, cruise, ship, lockdown, self, cruise ship, city, quarantinelife, princess
Wearing a mask | mask, face, wear, face mask, wear mask, protect, wear face mask, hand, use
White House Briefings | house, white, white house, bill, relief, force, package, stimulus, task, democrat
Second Wave Warnings | bad, good, flu, cdc, second, wave, second wave, warns, winter, cure

These all look like pretty normal topics and based on the top words you can probably guess what each one is about. The one interesting but probably not surprising topic is one that I’m calling “Anger”. This was essentially just a collection of tweets that used a lot of curse words. I will not share them here as they are rather inappropropriate for a blog post such as this but when you read through some tweets you can surmise that folks are angry at the situation. 

Ok so now we have our topics. But to get a better idea of how people were coping with Covid-19, we should look at how the topics changed over time. To do that I first took a look the document topic matrix that my NMF model spat out. The document topic matrix is essentially a table where each document (e.g. tweet) is a row and each column is a topic. The values represent how much of the topic was represented in each document. So to get an idea of how much of this topic was covered in the whole corpus of tweets I summed up each topic column. Next, I compared each topic’s value to understand the relative importance of each topic. Finally, I split up the data by month and plotted it to show how the topic “importance” change on a month-by-month basis.

![topics_over_time](/images/topics_over_time.png)

And voila! You can see here the main discussion on Twitter in December and January was about the initial outbreak in Wuhan, China which checks out. After that though, there are several topics near the top and things get a bit murkier. To get a better picture, let’s dive into each month to understand what changes and what that means. 

### December/January

In December there were only 300 tweets with the search term “coronavirus”. Near the beginning of January you start to see tweets about a “mystery illness” or “pneumonia” popping up. Also, you see some tweets quoting the WHO which said Covid-19 was not currently spreading. Obviously, at this time we now know that this disease spreads very quickly and through potential asymptomatic people but in January that wasn’t clear.

### February

![feb_topics](/images/jan_feb_mar_topics.png)

In February, at this point the Covid-19 outbreak was pretty bad in China. So the “Initial Chinese Outbreak” topic is still the biggest topic. However, in February the disease had spread to Italy and you see this topic peaking. In addition at this point, it’s becoming clear that this virus is spreading very fast so as a result you see topics about stopping the spread peaking. This is also when global events start to get cancelled to help to reduce the spread. 

### March

![march_topics](/images/feb_mar_apri_topics.png)

In March, the anger topic peaked. For context, I’m only looking at tweets in English and this is the month when Covid-19 started in large english speaking countries like the U.S. and UK. In the United States, this is when life started to get affected for almost everyone so it makes sense that this is when the anger would rise up. Also, since the U.S. now starts to have to deal with Covid-19 you also start to see tweets discussing President Trump’s response to the contagion. 

### April/May

![apr_topics](/images/mar_apri_may_topics.png)

In April and May, the anger topic goes way down in relative importance. From a quick glance, it seems like people took their anger and depending on their political affiliation focused it either on criticizing or defending President Trump’s response to the disease. Also, topics related to the new normal like wearing a mask and working from home were popular in these months. 

### Are people grieving the loss of a normal life?

So what did we learn from all of that? First of all, there were no topics in the top 15 about solutions to this massive problem we’re all facing right now. By that I mean, there is little discussion about vaccines and epidemiology. This makes sense given most of the general public (especially me) knows next to nothing about these topics. However, more importantly this data shows that people were dealing with something very difficult beyond just the physical effects of the disease.

Lots of people had a lot of anger in March but the “anger” topic is slowly going away. When I think about what that reminds me of, I think about the 5 stages of grief. For those who don’t know, the 5 stages of grief is a framework to describe the 5 stages someone goes through to get over the loss of a loved one. The stages are Denial, Anger, Bargaining, Depression, and Acceptance. So perhaps people are grieving losing their normal lifes and this is showing on Twitter. Right now we can no longer go to restaurants and bars. We don’t get to see friends and loved ones in person. Also, Covid-19 has introduced a lot of uncertainty in life. Perhaps you got laid off and don't know when the next paycheck is coming in. Perhaps you’re worried you may get laid off if things don’t improve. Perhaps you were planning on attend school in the fall and now this is in jeopardy. And on top of this you had no control over any of it. The director of this program is an invisible enemy called “coronavirus”. All of this can take an incredible emotional toll on us and some of us may need help to reach the final stage: acceptance. So the question then becomes how can we help people recover emotionally from the Covid-19 outbreak especially given the likelihood that this continues for a while or there is a siginificant second wave? 
