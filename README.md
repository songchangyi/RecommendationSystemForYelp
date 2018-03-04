# How to Make Use of Friendship Relations in Recommender Systems?

By Danilo de Oliveira, Paul Riverain, Changyi Song, Data Science students at IMT Atlantique

<div align=center><img width="500" src="/img/rec01.PNG"/></div>

“To connect people with great local businesses”: That’s the purpose of Yelp, an American multinational company founded in 2004, which allows its users to find local businesses such as restaurants, dentists, hairstylists and so on. Users can access Yelp via its website and also through its mobile app. Business’ recommendations are based on the reviews of the community of users of the service (the so-called Yelpers), filtered by a recommender software that looks at various measures of quality, reliability, and activity on Yelp.

Yelp’s database of users is substantial: By the end of Q3 2017, the system had a monthly average of 74 million unique visitors via mobile web and 30 million via the Yelp app, and a total of more than 142 million reviews written by the Yelpers. Yelp also allows its users to add friends and in this way follow their activities and reviews. However, the links between users aren’t used for the main recommendations, which seemed to be from our point of view a missed opportunity.

Therefore, placing ourselves in the shoes of Yelp employees, we have studied and developed a prototype of a new feature to be added to Yelp’s homepage: a “Popular among your friends” tab that makes use of friendships in order to present customized and more reliable recommendations.

<div align=center><img width="500" src="/img/rec02.PNG"/></div>

This project followed the CRISP-DM (Cross-Industry Standard Process for Data Mining) methodology. It provides a structured approach to planning a data mining project, breaking it into six major phases: business understanding, data understanding, data preparation, modeling, evaluation and deployment.

<div align=center><img width="500" src="/img/rec03.PNG"/></div>

The work of this project was conducted on the Yelp Dataset, a subset of the website’s businesses, reviews and user data made available for use in personal, educational and academic purposes. Yelp frequently hosts challenges aimed at students in order to use their data in innovative research projects.

“Lately we’ve been seeing a rise of recommendation systems built on the trust we place in our friends”

## Why a Social Recommender?
First of all, let’s take a look at what motivated this project: lately we’ve been seeing a rise of recommendation systems built on the trust we place in our friends. Facebook, currently the most popular social network in the world, implemented a feature which allows its users to ask for recommendations from their friends, who then provide their suggestions directly on the post. Another good example of the use of social networks for recommender systems is Vermouth, a mobile friend-based review app launched in 2017, focusing on opinions of people that the user knows.

Yelp itself already shows a tendency of becoming a social network, prompting users to search for their Facebook friends on Yelp, as well as sending invitations to their email contacts. It is also possible to see the recent activity of friends and people nearby. We wish to take this trend one step higher and turn this already implemented friend infrastructure into a fully fledged recommender system. Such a feature can stimulate users to leave reviews more often, increase user trust concerning the recommendations, giving advice that’s more suitable for the user, and even avoid shilling attacks (manipulation of reviews by the businesses).

<div align=center><img width="500" src="/img/rec04.PNG"/></div>
<div align=center><img width="500" src="/img/rec05.PNG"/></div>

What is a friendship on Yelp after all, and is it widespread enough to justify such an idea? A Yelp friendship is a two-way relationship. Once a person accepts an invitation, each one is added to the other’s friends list. An analysis over the users database showed an average of 14 friends for people with more than a friend. Over a total of 550 000 users, 65 000 have more than 5 friends and have published 10 reviews or more, which supports our interest in a friend-based recommender system for the service.

Within the dataset, we can distinguish four main concepts: businesses; users; reviews, which link users to businesses; and tips, which do the same, not by star classification, but through pieces of text.

“Traditional recommender systems ignore social interactions and connections between users, hindering the trust placed in the recommendations.”

## Traditional vs. Social Recommenders

In order to understand the technical part, a little bit of theory is necessary (worry not! We’ll start from scratch, and try to keep it simple). Recommendation systems attempt to suggest items that are likely to be of interest to the users. They can be based on two strategies: content filtering or collaborative filtering. The first one creates a profile for each user or item to describe its nature, allowing for the development association between users and items. However, it requires gathering external information. On the other hand, collaborative filtering  relies only on past user behavior to create its associations. Though unable to act on new users and items (the cold start issue), it is generally more accurate than its content-based counterpart, more widely used, and it’s the approach we chose for this project.

Let’s get a bit more into detail about collaborative filtering: its two primary approaches are memory-based and model-based. Within the memory-based category, we can mostly find the neighborhood algorithm, which computes relationships between users or items so that it can make rating predictions. Alternatively, looking at the model-based approach, the highlight is the algorithm of latent factors, which tries to infer factors from the rating patterns, like female/male oriented or comedy/drama, for example.
	
Digging deeper into the latent factor approach, we have the matrix factorization method. It describes users and items by vectors of the aforementioned latent factors, and a high correspondence between these factors means it would be wise to make a recommendation. A great strength of this method is that upon incorporating this inferred additional information, it allows us to overcome the fact that usually very few of the user-item combinations have explicit feedback.

Finally, we get to SoRec (short for Social Recommendation), our algorithm of choice. Traditional recommender systems ignore social interactions and connections between users, hindering the trust placed in the recommendations. In order to overcome this issue, SoRec combines a user social network graph with the user-item rating matrix in order to make more personalized recommendations.

## How Does It Work?

With the theory part out of the way, we can now describe the different steps in the execution of our prototype: First of all, we feed the recommender system with the trust and rating matrices, that is, which users are connected and what score users gave to businesses on their reviews. This stage represents the greatest share of data volume and computational runtime of the whole process. It also allows us to make performance measures on the predictions.

Afterwards, we create a group of users and businesses for which we want to generate recommendations. We select a city, business category (restaurants, for example), maximum distance and, of course, we check if the target user hasn’t already been there. The software then estimates the scores users are likely to give to these businesses, sorts them, and selects the top rated to be suggested to these Yelpers.

## How Can We Compare Recommender Systems?

A subject that generates a bit of confusion when we talk about the performance of recommender systems is “how do we measure it?” First of all, we need to define the metrics for the scoring. The main concern here is the accuracy of the system, that is, how well it predicts the real score an user gives to an item. The metrics we used to measure the differences between predicted and real score were the mean absolute error (MAE) and the root mean square error (RMSE). The first one measures the average magnitude of the errors in a set of predictions, while the other is the square root of the average of squared differences between prediction and actual observation. This means that large errors have a higher weight in RMSE, making it more useful when large errors are particularly unwanted.

But how do we test our model? Well, the most adequate way would be to do some A//B testing. That means creating two versions of the Yelp webpage or mobile app, one untouched, and the other with the addition of our prototype friend-based recommendation tab. Half of the service’s traffic gets to see the original version (control) and the other half is shown the modified product (variation). We then analyse the difference in performance between the two of them so as to conclude if the experiment had a positive impact.

However, since we don’t have the rights or infrastructure to perform this kind of testing, we kept to tests on the dataset, performing the so-called cross validation evaluation; more specifically, the K-fold method. It consists in dividing the dataset into k subsets, using k-1 subsets to fit our model and then testing it on the remaining subset. This procedure is performed k times, until all of the subsets have been tested on once, and then we get the average error over all of the trials. Thus, the impact of the way we split the data is reduced. With a 3-fold cross validation, we achieved an MAE of 0,246 and an RMSE of 0,355. That’s an improvement of about 8% over the RMSE of BaseMF, a model-based type of collaborative filtering that does not take social relations into consideration.

## To Big Data or Not to Big Data, That’s the Question
One of the big questions during the development of this project was whether we would use Big Data tools or not. Our preliminary tests showed that by filtering the data and as such reducing the volume to be processed (100 users in Las Vegas) when creating the recommendations, we could execute the code with satisfactory runtime in our machines, with our usual programming tools. The most demanding task is the preparation of the rating and trust matrices, which deals with the greatest volume of data and therefore takes about 2 hours to process. Then comes data preprocessing, modeling and prediction and recommendation generation, which require 1, 15 and 1 minute, respectively. Nonetheless, if we wish to extend our recommender system to a wider public and perform more tests, in spite of the time required to properly learn how to use the new tools, it would be greatly beneficial to switch to the PySpark API and the TeraLab platform.

## What’s in It for Yelp? 
An analysis of our prototype’s results and performance makes it fairly safe to say that it succeeds as a Proof of Concept and Yelp would consequently benefit from a more extensive study into the addition of a social recommender system to its services. Further testing with different parameters or even different social algorithms would certainly help refine the model, resulting in an effective large scale deployment.
