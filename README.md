# Streaming Wars: Disney+, Prime Video, Hulu and Netflix
By: Team The Avocados 🥑🥑🥑🥑🥑🥑🥑 <br />
## Contributors

### Project	Lead
Zachary Chang, @zachang890

### Team
Kevin Xu, @KevinXu264 <br />
Nora Liu, @yanxiliu230 <br />
Eleanor Pae, @epae0402 <br />
Annie Li, @JL03-Yue <br />

## Project Description & Motivation (Available On MEDIUM)
The video streaming services industry is everywhere, and it’s one that consumers use daily especially in quarantine. What would we do without some light binge-watching of Disney movies, classic sitcoms like “Friends”, and new content every other week? With over 2,000 streaming businesses in the streaming industry, 4 key players, and continuously booming growth due to the variety of content produced, it can be difficult to narrow down which sites are necessary for personal leisure and entertainment. If you’ve ever had difficulty choosing which subscription to give in to, you’re not alone.

Team Avocados had the same dilemma and aimed to find the best streaming service for entertainment consumers. When checking out services like Netflix, Hulu, Disney+, and Amazon Prime, the pros and cons can be confusing and overwhelming. 

Using Kaggle datasets of TV shows and movies offered on these 4 streaming platforms as well as IMDb datasets of movie ratings, we discovered which service outshined the rest for certain metrics. By looking at the amount of content, diversity and variety in the selection offered, popular and highly-rated movies, and exclusive or original streaming content, our team hoped to alleviate some of these concerns to narrow down the best service. 



## Tech Used
- Jupyter Notebooks
- RStudio
- Matplotlib, ggplot, highcharter
- MySQL Workbench

## Data Sources
- #1: [Kaggle: Movies on Netflix, Prime Video, Hulu and Disney+](https://www.kaggle.com/ruchi798/movies-on-netflix-prime-video-hulu-and-disney)
- #2 [Kaggle: TV Shows on Netflix, Primve Video, Hulu and Disney+](https://www.kaggle.com/ruchi798/tv-shows-on-netflix-prime-video-hulu-and-disney)
- #3: [Kaggle: Movie Industry](https://www.kaggle.com/danielgrijalvas/movies)
- #4: [Kaggle: IMDb movies extensive dataset](https://www.kaggle.com/stefanoleone992/imdb-extensive-dataset)
- #5: [Kaggle: MovieLens 20M Dataset](https://www.kaggle.com/grouplens/movielens-20m-dataset?select=movie.csv)
- #6: [Kaggle: Netflix Movies and TV Shows](https://www.kaggle.com/shivamb/netflix-shows)
- #7: [Kaggle: Netflix Top TV Shows](https://www.kaggle.com/ritesh2000/trending-tv-shows-on-netflix)
- #8: [IMDB DataSet](https://www.imdb.com/interfaces/)

## Ranking Categories
### Category #1: Diversity
Datasets used: #1, #2 <br />
Objective: Quantify and rank the diversity of films available on each streaming platform, where greater diversity is considered more favorable

The challenge of ranking the streaming services by diversity is that diversity is inherently a rather abstract category. In ecology, scientists have developed a mathematical methodology for quantifying biodiversity by taking into account the *number of species* as well as the *abundance of each species*. Through further research, it was concluded that this is the best mathematical representation of diversity to be found that can be extrapolated to cover *number categories of metrics* and the *abundance of instances of those categories of metrics*. (ex. categories of metrics = [1984, 1926, 1926] abuncance = {1984: 1, 1926: 2}) 

In Python, the mathematical formula is as follows:
```python
def simpson_index(year_count):
    total_count = 0
    for key in year_count:
        total_count += year_count[key]
    denom = total_count * (total_count - 1)
    
    numer = 0
    for key in year_count:
        numer = numer + (year_count[key] * (year_count[key] - 1))
    return 1 - (numer/denom)
```
where __numer__ is ∑n(n-1) where __n__ is the number of instances of a certain metric (ex. in this example, year of production)
and where __denom__ is N(N-1) where __n__ is the number of films considered in this metric.

A similar formula/procedure was applied across the board for six unique metrics including: production year, country, genre, language, age rating, and runtime. The results for this category for diversity are as follows:
| Placing | Production Year | Country | Genre | Language | Age Rating | Runtime (difference too small to count in overall ranking) | Overall |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| *1st Place*  | Disney+  | Netflix | Hulu | Netflix | Hulu | *Netflix* | __Hulu__ 🎉 🟩|
| *2nd Place*  | Prime Video  | Hulu | Prime Video | Hulu | Netflix | *Disney+* | __Netflix__ 🟥|
| *3rd Place*  | Hulu  | Prime Video | Netflix | Prime Video | Disney+ | *Prime Video* | __Prime Video__ 🟧|
| *4th Place*  | Netflix  | Disney+ | Disney+ | Disney+ | Prime Video | *Hulu* | __Disney+__ 🟦|

### Category #2: Ratings
Datasets used: #1

Objective: What is the average and typical IMDb ratings of movies on each streaming service? What percentage do those generally recognized good movies hold among all the movies each service offers?

Customers usually hope that the streaming service they choose actually worth their money. And what we mean by worth it? One important thing we consider is the quality of movies we will get. We definitely do not want to log onto the service and just find that most of the movies don't worth us sitting their for two hours.

After some analysis of actual mean, median, as well as percentage of movies of different rating range, we discovered that Disney+ wins in all three categories. And even though we generally feel that there is a difference in terms of the genres of movies that each streaming service focus on, e.g. we go to Disney+ for musicals and comedy, the analysis actually tells us that there is not much difference. This finding comes from the word cloud of genres of movies with IMDb above 6.0 generated in R, take netflix for example:

What's more, we also analyzed the distribution of rating according to different platforms, as well as the rating of each genre of each of the four platforms. We discovered that Documentaries and News have higher ratings overall. Disney+ takes lead in Family, Adventure, and Animation, and Netflix is especially rated high for Drama and Comedy.

```r
library(wordcloud)
library("RColorBrewer")

netflix_genre <- c()

for (index in 1:nrow(netflix_six)) {
  movie_to_test <- table[index, ] # a temporary object to store the movie we are going to process
  genre <- as.character(movie_to_test$Genres) # convert Genre from factor to character object
  genre <- unlist(strsplit(genre, split = ","))
  netflix_genre <- c(netflix_genre, genre)
}

table_netflix_genre <- table(netflix_genre) # table() will give us the count of each genre
netflix_genre_freq <- data.frame(table_netflix_genre)

layout(matrix(c(1, 2), nrow=2), heights=c(1, 4))
par(mar=rep(0, 4))
plot.new()
text(x=0.5, y=0.3, "Netflix", cex = 1.5, col = "black")
wordcloud(words = netflix_genre_freq$netflix_genre, freq = netflix_genre_freq$Freq, min.freq = 1, colors=brewer.pal(8, "Set1"))
```

More detailed findings can be found in our Medium article.

### Category #3: Amount of Content
Datasets used: #1, #2

Objective: How much content is there on each streaming service? Is the selection available an important factor to consider when choosing a streaming service?

For avid movie watchers, the amount of content available is key to exploring new entertainment and stepping outside of one’s comfort zone. Looking at the selection of movies and TV shows offered on streaming services is something that users want to know before deciding on a service. 

Using the two Kaggle datasets, we were able to extract and separate data on how many movie and TV show titles were available on each streaming service as of 2020. After compiling this data, it was easy to compare the amount of content between streaming services by looking at the number of titles available across the four different services. 

We noticed that Prime Video took the lead by a large margin in terms of movie content, and it also had the most TV shows although the margins did not vary as much between Prime Video, Netflix, and Hulu. The trends for movies and TV shows are consistent for the four streaming services because the order for greatest to least amount of content is: Prime Video, Netflix, Hulu, and Disney+.

In R, we created dataframes and visualizations with the relevant data for movies and TV shows across all four streaming services:
```r
total <- merge(amtofmovies, amtofshows, by="service")
 
streaming <- c(rep("Netflix" , 2) , rep("Hulu" , 2) , rep("Disney+" , 2) , rep("Amazon Prime" , 2) )
condition <- rep(c("movies", "shows") , 4)
value <- c(3560, 1931, 903, 1754, 564, 180, 12354, 2144)
data <- data.frame(streaming,condition,value)
 
ggplot(data, aes(fill=condition, y=value, x=streaming)) + 
    geom_bar(position="dodge", stat="identity") + scale_fill_manual(values=c("#b392ac", "#e8c2ca"))+ggtitle("Movies and Shows per streaming service") +
  xlab("Streaming Service") + ylab("Number of titles")
  ```

We also examined the amount of content per age group across the four platforms. From there, we concluded that Prime Video contains comparatively more 18+ and 7+ movies than other three platforms, partially due to the amount of content it has. However, Prime Video only has a slightly advantages for 13+ and movies for all ages than platform with the second most corresponding content --- Disney+. Hulu has the least content, and Netflix has decent amount of content for 7+ and 13+, although falls short in overall content. 
...
### Category #4: Exclusive Content
Datasets used: #1,6,4,6,7,8
Objective: What differentiates their top 10 list? How popular are the top movies? And what percentage of their views go towards exclusive platform content?

It was inherently difficult to find data that was available on consumer watch data since that is not publicly available information, and thus the next best thing was to solely base exclusive content based on what shows and movies each subscription service offered and ranking them based on the number of IMDb user votes. We felt that that was a more accurate measure of audience popularity and consumption as compared to purely IMDB rating, since that is a performance of how well the movie was according to only a certain number of people, and can be biased to movies/TV shows that have low number of reviewers. 

To generate the data for this comparison, we first split and subset the datasets on Kaggle that distinguished which subscription service offered which TV shows and movies, and then combined that with IMBDb rating and user votes data to generate a combined data frame. 

While IMDb movie scores was widely available on Kaggle, we couldn't find any IMDb ratings for TV shows, so we created our own by extracting the open source IMDb data on their website, filtering for only TV shows, and then combining them using SQL queries due to the massive size of the data. In SQL, the query used was

```SQL
CREATE TABLE (
	SELECT movie_names.titleType, movie_names.primaryTitle, movie_ratings.averageRating, movie_ratings.numVotes, 
	movie_names.startYear, movie_names.runTimeMinutes, movie_names.genres
	FROM movie_names
	INNER JOIN movie_ratings
	ON movie_ratings.tconst = movie_names.tconst
)

```

After collecting all the movies and TV shows that each subscription service offers, we then sorted and collected data for the top 100 Movies and TV shows based on the num of user submitted votes on IMDb and we compared the distribution of release dates, IMDb votes, and IMDb ratings across all four subscription services. We then determined the winners in each category. It was difficult to come up with a clear overall winner, because even though Disney was outleading in the movies categories, it severely underperformed compared to every other service in the TV shows category. Below is just an example of what the data frames looked like that we used to graph. More data is included within our data and graphs section in our repo! 

#### This is an example of the top 15 Disney Movies
|    | Title                                                  | Year | IMDb | imdb_votes | service |
| -- | ------------------------------------------------------ | ---- | ---- | ---------- | ------- |
| 1  | The Avengers                                           | 2012 | 8    | 1225316    | Disney  |
| 2  | Star Wars: Episode IV - A New Hope                     | 1977 | 8.6  | 1188658    | Disney  |
| 3  | Star Wars: Episode V - The Empire Strikes Back         | 1980 | 8.7  | 1109656    | Disney  |
| 4  | Avatar                                                 | 2009 | 7.8  | 1086714    | Disney  |
| 5  | Guardians of the Galaxy                                | 2014 | 8    | 1007917    | Disney  |
| 6  | Pirates of the Caribbean: The Curse of the Black Pearl | 2003 | 8    | 992127     | Disney  |
| 7  | WALL·E                                                 | 2008 | 8.4  | 955757     | Disney  |
| 8  | Star Wars: Episode VI - Return of the Jedi             | 1983 | 8.3  | 912250     | Disney  |
| 9  | Finding Nemo                                           | 2003 | 8.1  | 911647     | Disney  |
| 10 | Iron Man                                               | 2008 | 7.9  | 910300     | Disney  |
| 11 | The Lion King                                          | 1994 | 8.5  | 901362     | Disney  |
| 12 | Up                                                     | 2009 | 8.2  | 895906     | Disney  |
| 13 | Toy Story                                              | 1995 | 8.3  | 846332     | Disney  |
| 14 | Star Wars: Episode VII - The Force Awakens             | 2015 | 7.9  | 833706     | Disney  |
| 15 | Monsters, Inc.                                         | 2001 | 8    | 779728     | Disney  |

#### This is an exameple of the top 15 Netflix TV Shows
|    | Title                                                  | Year | IMDb | imdb_votes | service |
| -- | ------------------------------------------------------ | ---- | ---- | ---------- | ------- |
| 1  | Breaking Bad                                           | 2008 | 9.5  | 1467953    | Netflix |
| 2  | The Walking Dead                                       | 2010 | 8.2  | 854372     | Netflix |
| 3  | Stranger Things                                        | 2016 | 8.8  | 824251     | Netflix |
| 4  | Sherlock                                               | 2010 | 9.1  | 808347     | Netflix |
| 5  | Dexter                                                 | 2006 | 8.6  | 646903     | Netflix |
| 6  | House of Cards                                         | 2013 | 8.7  | 467258     | Netflix |
| 7  | Black Mirror                                           | 2011 | 8.8  | 445446     | Netflix |
| 8  | Arrow                                                  | 2012 | 7.6  | 407822     | Netflix |
| 9  | Supernatural                                           | 2005 | 8.4  | 393006     | Netflix |
| 10 | Narcos                                                 | 2015 | 8.8  | 358250     | Netflix |
| 11 | Peaky Blinders                                         | 2013 | 8.8  | 350120     | Netflix |
| 12 | Better Call Saul                                       | 2015 | 8.7  | 324611     | Netflix |
| 13 | The Flash                                              | 2014 | 7.7  | 305509     | Netflix |
| 14 | Family Guy                                             | 1999 | 8.1  | 304819     | Netflix |
| 15 | The Witcher                                            | 2019 | 8.3  | 300996     | Netflix |

## Overall Results & Conclusion

So now that you’ve read through all of that, you’re probably wondering what the final ranking is for each of the streaming services. We compiled the rankings together across each of the four categories, diversity, amount of content, IMDb ratings, and exclusive content into the table below and our winner is……...

| Placing | Diversity | Rating | Amount of Content | Exclusive Content | Overall |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| *1st Place*  | Hulu | Disney+ | Prime Video | Netflix | __Netflix__ 🎉 🟥|
| *2nd Place*  | Netflix | Netflix | Netflix | Disney+ | __Prime Video__/__Hulu__/__Disney+__ 🟧🟩🟦|
| *3rd Place*  | Prime Video | Hulu | Hulu | Prime Video |-|
| *4th Place*  | Disney+  | Prime Video | Disney+ | Hulu | -|

Netflix! Based on our data analysis and visualizations, our team determined that Netflix was the best overall streaming service, ranking first in terms of exclusive content and second in diversity, ratings, and the amount of content. Surprisingly, the other three streaming services, Prime Video, Hulu, and Disney+ all tied for second place, with their respective placings found above. So the next time you’re unsure of what to watch, you can’t go wrong with picking Netflix!




