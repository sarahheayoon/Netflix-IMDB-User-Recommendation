# Netflix-IMDB-User-Recommendation

## Statement of the problem: 
You are the CEO of Netflix, and you want as many people to watch movies on your platform as possible. You’re interested in two questions: 
Which movies should you show to individual users so that they will watch them? 
What movie should you commission next that would be popular among your users?
In this report, we focus on individual user preferences by genre, as well as general user base preferences by genre, runtime, user ratings, and critic ratings.

## Data: 
We utilized two datasets from Kaggle: one dataset containing a list of Netflix movies and users’ ratings of those movies ([source](https://www.kaggle.com/datasets/rishitjavia/netflix-movie-rating-dataset?select=Netflix_Dataset_Movie.csv)) and another containing a list of films and ratings from IMDB ([source](https://www.kaggle.com/datasets/harshitshankhdhar/imdb-dataset-of-top-1000-movies-and-tv-shows)). The Netflix dataset was split into two .CSV files. One contained a list of movie names, their affiliated IDs, and the years when each was released; the other .CSV file contained a set of user IDs, the names of the movies each user rated, and the user’s ratings for each movie on a scale of 1-5. We joined the two datasets using R. 

The IMDB dataset had a list of movies along with each movie’s aggregate rating by IMDB users, Meta score (critic rating), genre, runtime, and number of votes by users (the number of users who provided a rating).

We took the common movie names in the Netflix and IMDB datasets and created a new dataset via inner join that consolidated all the different variables from each of the original datasets. This dataset, which we will call the Consolidated dataset, will later help us in our Binomial analysis.

## Analyses and Results: 
We took three separate Bayesian approaches in our analyses. The first approach is the binomial-beta analysis using Netflix movie genres to drill into one user’s watch history. We wanted to predict the probability of a user watching a certain genre in his or her next Netflix session based on their movie preference. We estimated the user preference by viewership and their ratings. 

### Approach 1. Binomial Analysis of Viewership based on Genre
To do the preliminary data analysis, we sorted the consolidated dataset of movies in terms of genre. We wanted to find out which genre people watch the most. There are a total of 60 subdivided genres, of which ‘Drama’ took a significant portion in the pie chart (as shown below). It is also interesting to note the second most watched genre is ‘Crime, Drama ,Thriller.’ Perhaps, in general, people do enjoy watching the general drama genre. For our project, we restricted our scope to 
‘Drama’ movies for both the prior and posterior. We wanted to see what is the probability of an individual user watching a movie from the genre ‘Drama’ given the proportion of movies they have watched that are marked as ‘Drama.’ 

<img width="1107" alt="1" src="https://user-images.githubusercontent.com/89557209/172509467-5eb20d5e-5b13-4423-a48d-6a26d60b6858.png">

In order to test individual user behavior, we took out one user (User_ID : 387418) from the entire dataset. We observed that this particular user had watched 10 ‘‘Drama’ movies out of the 91 movies she watched in total. 

We wanted to use a binomial distribution with beta priors because we are interested in whether or not a user will watch a movie in the drama genre as opposed to any other genre, a question that is best modeled binomially.

We used three priors. The first is a uniform prior, which assumes that we have no prior belief, but we will simply use this to compare with the following two beta priors. We took the beta prior of (1,1), and we know that the particular user watched 10 movies that are from the ‘drama’ genre out of the 91 movies she watched in total. When we calculated the posterior distribution subject to our uniform prior, we have the posterior mean of 0.1183 and the posterior standard deviation of 0.0333. A 95% Bayesian credible interval for the mean is [0.0612, 0.1908]. The results are shown below:

<img width="723" alt="2" src="https://user-images.githubusercontent.com/89557209/172509628-245d89eb-7911-46a6-87bb-f42570af7b42.png">

For the second prior, we computed the beta parameters with the given mean and standard deviation from our Consolidated dataset. We have 0.1248 as the mean and standard deviation of 0.0219 for the whole population. The beta parameters are alpha equals 28.3 and beta equals 198.53, which are calculated computationally. The results are shown below: 

<img width="723" alt="3" src="https://user-images.githubusercontent.com/89557209/172509661-0c901b1a-0b8f-4c52-a3b2-bf3c29452870.png">

Finally, we graphed the percentage of users’ viewership that occupied the drama genre in a histogram across all users.

<img width="631" alt="4" src="https://user-images.githubusercontent.com/89557209/172509683-142aeafe-2da4-4bff-b715-8b6608052a4e.png">

We then found a beta prior that looks the most similar to our histogram displaying this watch history and found a beta prior that reflects our belief. The beta prior we took was (2.5, 10). The posterior mean is 0.1208 and the posterior standard deviation is 0.0319. The 95% Bayesian credible interval for the mean is [0.0656, 0.1898]. 

<img width="721" alt="5" src="https://user-images.githubusercontent.com/89557209/172509732-223f31a0-8a7a-4f3d-ab67-d3c945553971.png">

We conclude that our posterior beliefs, regardless of which prior we use, are around 12%. In other words, there is a 12% chance that this particular user will watch movies in ‘Drama’ as opposed to other movies in other genres! 

<img width="711" alt="6" src="https://user-images.githubusercontent.com/89557209/172509772-6839920b-45ee-49ee-914d-0c2cd27ec722.png">

According to the Netflix 2022 Q2 report, it does seem that users do prefer drama as a genre for their choice of movies as opposed to other movies. Our data is also from 2005-2016, rather than more recent data. Hence, for further research it would be of interest to see how either the number of movies in the ‘Drama’ genre has grown quantitatively and/or how users’ preference for ‘Drama’ subsequently has increased.

<img width="222" alt="7" src="https://user-images.githubusercontent.com/89557209/172509851-dadbeab4-251b-4772-afd9-22c95ef899c0.png">

### Approach 2. Binomial Analysis of Users’ Film Preference
While our first approach set priors based on the percentage of specific genre based on a general trend of watching that genre from a given population, we also wanted to also look at the user ratings to estimate their preference for different genres. For example, we assumed that a user would prefer watching a certain genre more if they rated those movies higher as opposed to other movies. Hence, we wrangled our Consolidated dataset once again by each user and sorted them by the accumulated rating scores for each genre. Our new Consolidated data has rows of individual users and their accumulated ratings for 60 sub genres.

We then graphed the accumulated score distribution for each genre by user in histogram:

<img width="839" alt="8" src="https://user-images.githubusercontent.com/89557209/172509888-f750588f-0c6e-4942-a224-fcdc61237165.png">

We noticed again that ratings for ‘Drama’ and ‘Crime, Drama, Thriller’ were particularly high. And the amount of times these genres were rated were more frequent than the others. Hence, we wanted to take these two genres again and set our priors based on their ratings. Here is what our dataset looks like after wrangling. The scores under each genre is an individual user’s accumulated scores. 

<img width="1106" alt="9" src="https://user-images.githubusercontent.com/89557209/172509914-eb37f96a-92b9-42a3-9134-123ba6176a92.png">

We took the percentage of their scores by dividing each total score for genre by the total ratings they have done. We interpreted this percentage of scoring by genre as the likelihood of rating the genre out of their total watch history. We then calculated the mean and standard deviation for the percentage across 143,410 users.

Similar to our first approach, we took the binomial prior since we wanted to determine what is the probability of a given user watching a specific genre as opposed to all other genres given their rating history.

Here is our posterior distribution for the particular user (User_ID : 387418) for both genres:

For ‘Drama’ :

<img width="712" alt="drama" src="https://user-images.githubusercontent.com/89557209/172509959-77093cc9-4d3c-4a08-b8f9-b58047997db1.png">

For 'Crime, Drama, Thrille':

<img width="675" alt="cdt" src="https://user-images.githubusercontent.com/89557209/172509989-f1b70426-d8d1-443e-8c80-9b97847f904d.png">

Again, we see that the probability of the given user watching ‘Drama’ is around 11%. The probability of watching ‘Crime, Drama, Thriller’ is around 7%. This is particularly an interesting conclusion. Regardless of the way we measure preference–either quantitatively by the percentage of a particular genre from a user’s watch history or by user’s rating history–the distribution of posterior for the probability of watching ‘Drama’ stays around 11%.

### Approach 3. Bayesian Linear Regression of Viewership v. User Ratings
Our final approach was to conduct a Bayesian linear regression. Initially, we were interested in running a multiple linear regression in order to investigate a range of factors bearing on film viewership, employing the IMDB and Netflix datasets for two rounds of updates. We discovered, however, that only the IMDB dataset contained all the necessary variables; the Netflix dataset lacked data about films’ genre, runtime, or critic ratings. Ergo, if we combined the Netflix dataset with the IMDB dataset via outer join, we could not update our priors with the Netflix dataset because the Netflix films did not did not have genre or runtime assigned. If we combined the Netflix dataset with the IMDB dataset via inner join (i.e., if we extracted the union of the two sets), we could not update our beliefs about runtime or genre because these variables would already be used in the prior. Either path precluded a complete Bayesian multiple regression. The pairwise graphs below, which depict data from the IMDB dataset, offer a glimpse at what we might have worked with.

<img width="768" alt="01" src="https://user-images.githubusercontent.com/89557209/172510552-e53103c0-955c-45e8-86d6-d7ea3100e770.png">

Recognizing these limitations, we opted instead to study the relationship between user ratings and movie viewership via a Bayesian simple linear regression. We chose these variables both because they are shared by the Netflix and IMDB datasets, and because—as the figure above illustrates—there appears to exist an evident linear relationship between the number of votes and user rating. To what extent is it true that films that receive higher user ratings attract a larger audience? Employing the total number of users who rated each film on the Netflix and IMDB websites as estimates of the total viewership that films received by the sites’ users, we regressed this estimate of film viewership on the aggregate user rating each film received on a 1-5 scale. (We divided the IMDB rating, standardly rated on a scale of 1-10, by two to ensure estimates of the beta coefficient are scaled correctly between the two rounds of updating.)

Although our group possessed the prior belief that user ratings positively correlate with film viewership, we chose to use a flat prior as our starting point because we had no means of estimating the magnitudes of the beta coefficient or intercept for our prior belief. The figures below graph the number of ratings for a film on the y-intercept and the same film’s mean user rating on the x-intercept.

<img width="733" alt="02" src="https://user-images.githubusercontent.com/89557209/172510573-4bc40ec3-8124-4eb2-935f-b3d65ea6708b.png">

After updating on our flat prior using the Netflix data (round one), we find the following: 
<img width="713" alt="03" src="https://user-images.githubusercontent.com/89557209/172510587-3193d197-6768-4d0d-9ca8-4b4fb4f3f2cc.png">

The beta coefficient for the posterior is estimated at 7505 additional raters per 1 rating point (top left figure). With the endpoints of the 95% credible interval at 5280.4 and 9729.6 (no interlap with 0), we can be confident that the correlation between user ratings and viewership is a positive one. 

The second update further affirms our belief. Employing the results above as the new prior inputs, we find the following as our final posterior:

<img width="608" alt="04" src="https://user-images.githubusercontent.com/89557209/172510616-541efe58-f9db-4744-ae15-1711d7cf08ed.png">

We land on a final estimate of the beta coefficient as 7857 additional raters for every rating point added, slightly higher than the prior estimate. The endpoints of the 95% credible interval are 5,632.792 and 10,081.21.

It should be recognized, however, that does in no way grant certainty about a one-way causal relation by which higher ratings beget larger viewership. Because we use the “number of voters/raters” as a proxy for a measure of viewership, it is feasible that in fact a positive feedback loop is at play; the better a film is rated, the more people will view the film and the more people will rate the film well, further raising the film’s rating. 

## Conclusions: 
We conclude that there is a 12% chance that a particular user will watch a movie in ‘Drama’ as opposed to other movies in other genres. Further, we estimate that for every additional point on a 1-5 scale for a film’s aggregate online rating, the film will receive approximately 7,857 additional raters.

## Discussion: 
Our results allow us to determine what movie should be shown to a particular user based on the genres the user has watched previously, as well as their ratings of movies. 

Because our data was taken from several years ago, and media consumption has drastically and rapidly changed since then, it would be of interest to conduct further analyses with more recent data. For instance, movie watchers’ preferences may have evolved so that drama is not as prevalent as a genre nowadays, or has become even more relevant today. Particularly, with the COVID-19 pandemic, watchtime has skyrocketed, and new patterns of viewership may have emerged.

These results would be particularly interesting for film producers, who would want to analyze users’ watch history and take advantage of trends in viewership, or Netflix, who seeks to commission movies. Ultimately, we obtained interesting results about the relevant media and entertainment industry, allowing us to understand more about the world we live in. 


