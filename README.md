# ETL_Project
Group project
Group project
Team: Jacob Concha, Tina Ren, Veerpal Sanga, Allen Virtusio

Project Proposal:
Extract data for the top 100 ranked movies from imdb and rottentomatoes and scrape ratings from the other to get the complete data for the aggregated top ranked movies from both imdb and rottentomatoes. 


# EXTRACT/TRANSFORM: 


In Jupyter Notebook: 
1) We imported pandas, beautiful soup, and chromedriver manager to help establish browser paths to rottentomatoes url to extract the top 100 movies ranked by rottentomatoes. 
After browsing the webpages html tags, we saw that the tag was a table for the movies list and with pandas ability to read html tables, we used pandas to extract the table. 
The table produced the rank, tamotometer ratings, title, and number of reviews of the movies. This result is outputted to a csv file. 


2) To extact the top 100 movies from imdb, we created the imdb_parse function to scrape the imdb browser and append the info to a list. 
Created a function that uses the browser path and beautifulsoup to find the tag and class item that will pull the title and ratings information in the browser and appends this information to the imdb list. 
We declare an empty list as the imdb_list to add the elements when calling the function to visit the browser at the imdb url and look for the title and ratings of the movies in that url over the two webpages. 
This produces a nested list with the movie and ratings. We loop through this to obtain just the movie titles and append this to a new list to be used for further scraping listed below. 

After an initial comparison to see how much overlap there was between the two lists, we found there was very little overlap, only about 13 movies were on both lists.

3) Next step, we scraped the ratings information of the imdb movies list for the rottentomatoes ratings and scraped the information of the rottentomatoes movies list for the imdb ratings.
Created a function to visit google chrome with the movies as a variable in the google url to loop through each of the top 100 imdb movies. 
The function would then visit that url and click the rottentomatoes link by finding the partial text of the link to be "https://www.rottentomatoes.com/" and then look for the html element that would provide the ratings information. 
Once we call the function, the ratings along with other information from rottentomatoes appends to a new list. 
We further transform this function call list to show the data more cleanly. Using the .split('\n') and looping through each of the results, we nest the results for each movie to a list. 
We then extract each of the results into distinct lists, a list for tomatometer ratings, audience scores, and others. We then take each of our unique lists thus far and transform them into dataframes with pandas. 
From each dataframe we take the relevant columns to create a comprehensive dataframe with the imdb top 100 movies, tomatometer, audience score, and imdb ratings information. This is outputted to a csv file. 


4) To retrieve the imdb ratings for the top 100 rottentomato movies, we first read in the output file from step 1 with pandas. Using a similar set up as step 3 above, however, instead of a function we created a for loop to loop through the movie column from rottentomato csv file. 
The for loop then visits google chrome with the movies as a variable in the google url to loop through each of the movies. The for loop then visits that url and clicks the imdb link by finding the partial text of the link to be "https://www.imdb.com/title/" and then look for the html element that would provide the ratings information. 
The ratings and number of reviews information from imdb appends to a new list. 
We transform this list with a for loop and the .split function to only grab the ratings information and append to a new list. We transform this new list to a dataframe and then concatenate this with the original rottentomato dataframe read in initially, to output a csv file for rottentomato top 100 movies, tomatometer, and imdb ratings. 
to later merge with our csv file outputted in step 3. 

5) To merge the files retrieved in steps 3 and 4, we read in the csv files with pandas. Clean the movie titles in the imdb movies dataframe to match the other data file to contain a space between the movie name and the year with the .replace function. 
This then replaces the original movie name column with the clean titles and we pull in only the relevant column headers. 
Also, pull in relevant column headers in the other dataframe as well. Finally, merge the 2 dataframes by outer join to pull in all unique movies in both dataframes and remove any duplicates. This was outputted to a csv file.


# LOAD

Reading in our final merged dataframe with pandas, we loaded this final dataframe to a sqlite database using sqlalchemy to create an engine and connect our dataframe to sqlite. 
 
Also, created a mongo database with pymongo to connect python to MongoDB. We use a default port to make a connection with mongoclient. We define the database in Mongo and iterate through each row and insert the documents into each movie collection.


SOURCES: 

Top 100 movies from imdb: 
https://www.imdb.com/search/title/?groups=top_100&sort=user_rating,desc&ref_=adv_prv

Top 100 movies from rottentomatoes: 
https://www.rottentomatoes.com/top/bestofrt/