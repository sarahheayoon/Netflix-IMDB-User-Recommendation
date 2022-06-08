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


In order to test individual user behavior, we took out one user (User_ID : 387418) from the entire dataset. We observed that this particular user had watched 10 ‘‘Drama’ movies out of the 91 movies she watched in total. 

We wanted to use a binomial distribution with beta priors because we are interested in whether or not a user will watch a movie in the drama genre as opposed to any other genre, a question that is best modeled binomially.

We used three priors. The first is a uniform prior, which assumes that we have no prior belief, but we will simply use this to compare with the following two beta priors. We took the beta prior of (1,1), and we know that the particular user watched 10 movies that are from the ‘drama’ genre out of the 91 movies she watched in total. When we calculated the posterior distribution subject to our uniform prior, we have the posterior mean of 0.1183 and the posterior standard deviation of 0.0333. A 95% Bayesian credible interval for the mean is [0.0612, 0.1908]. The results are shown below:
