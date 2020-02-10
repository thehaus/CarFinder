# Car Finder
## What it is
CarFinder is a website that returns cars and MSRP prices for cars matching a set of features.
## Why it is
CarFinder was created due to my obsession with finding cars that meet weird requirements. Unlike sites like autotrader which find cars for sale, I wanted to find cars that match certain requirements. For instance, if I wanted a v8 AWD manual sedan with a minimum of 300hp, do any exist? More than that, if I wanted leather seats and a HUD, what model and trim has both of those?
## Problem approach
Starting with the problem "I want a site that allows me to select features and return cars that match", the starting point is that I need a database full of information and a UI to filter and display those results. This gives me a general idea of the approach, but the problem needs to be broken down a little further.

# Database
This is most the most important piece. The quality of the data directly correlates to the quality of the software. You could make a fairly simplistic UI and service layer, but without data being correct, the software would be pointless. My first thought was to plan out all of the necessary tables and relations and then look up the data points to fill them in, but there is something that needs to be considered first: the size of the data. 
## To manually input, or not to manually input?
In the United States, there are 47 different car manufacturers with cars on the market, and that is excluding some of the more boutique manufacturers like Bugatti or McLaren, so let's round to 50. If I took an average guess that each brand has 5 models and each of those models has 4 trims, that's 1,000 different data points. Consider that each trim might contain 10 new features: if I were to manually input every make, model, trim, and feature set for that trim, I would be inputting over 10,000 points of data. If each took me just one minute to upload, that's 20+ days, 8 hours per day, of manually inputting data. That's not my idea of a good time.

So, if I'm not manually inputting data, I felt there were three options:
1. Purchase/find an existing database
2. Use an existing API to pull in the necessary data points
3. Emulate the way I would be getting the car data manually by web scraping and pulling in the data.

I did not go with option one, because I knew that existing data might not be as clean as I was expecting, and I couldn't be sure that all the data points I was looking for would be there. Using an API is a great idea, but I was not able to find any useful ones. I originally attempted to use the Autotrader API, but the result sets did not have all the information I needed. In addition, sites like Edmunds have either removed their API access, put them behind paywalls, or don't return the necessary data points. This left web scraping as the solution of choice.
## Designing the database
Now that I had decided what method I was using to seed the data, I could take a look at what data points were available via web scraping so that I effectively could design the database. To get a quick prototype, I wrote the web scraper and inputted the data into text files as JSON. This would allow me to easily write a script reader to pull the data into a more permanent database, such as MySQL or MongoDB. The data I was able to scrape followed this pattern: a "CAR MAKE" has "CAR MODEL"s. A "CAR MODEL" has "TRIM LEVEL"s. A "TRIM LEVEL" has "FEATURE"s. A "FEATURE" is a set of attributes, such as "HORSEPOWER": "300", "HEATED SEATS": "true", "PANORAMIC SUNROOF": "true".

Despite the ease in which uploading these json files to a document database would be, I wanted the data to be consistent, reliable, and relational. Perfect use case for an RDBMS.
