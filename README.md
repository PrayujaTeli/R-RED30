# Twitter Data Collection and Analysis using Tweepy

This project demonstrates how to use the Tweepy library to collect tweets from Twitter based on a specific search query, process the data into a pandas DataFrame, and save the data into a CSV file for analysis.

## Features
- Collect tweets using a search query
- Filter out retweets, replies, and links
- Store tweet attributes (username, date created, likes, source, and content)
- Save tweet data as a CSV file

## Requirements
- Python 3.x
- Pandas
- Tweepy

## Installation

To install the necessary libraries, run:

```bash
pip install git+https://github.com/tweepy/tweepy.git
pip install pandas
```

## Running the Script

### Step 1: Set up Twitter Developer Account

1. Create a Twitter Developer Account and create an app to get your **API keys**.
2. Add your `consumer_key`, `consumer_secret`, `access_token`, and `access_token_secret` in the script.

### Step 2: Run the Script

```python
import tweepy
import pandas as pd

# Replace with your credentials
consumer_key = "YOUR_CONSUMER_KEY"
consumer_secret = "YOUR_CONSUMER_SECRET"
access_token = "YOUR_ACCESS_TOKEN"
access_token_secret = "YOUR_ACCESS_TOKEN_SECRET"

# Authenticate with Twitter
auth = tweepy.OAuth1UserHandler(consumer_key, consumer_secret, access_token, access_token_secret)
api = tweepy.API(auth, wait_on_rate_limit=True)

# Define search query and tweet limit
search_query = "'ref''world cup'-filter:retweets AND -filter:replies AND -filter:links"
no_of_tweets = 100

# Retrieve tweets
tweets = api.search_tweets(q=search_query, lang="en", count=no_of_tweets, tweet_mode='extended')

# Process tweet attributes
attributes_container = [[tweet.user.name, tweet.created_at, tweet.favorite_count, tweet.source, tweet.full_text] for tweet in tweets]
columns = ["User", "Date Created", "Number of Likes", "Source of Tweet", "Tweet"]

# Create DataFrame
tweets_df = pd.DataFrame(attributes_container, columns=columns)

# Save to CSV
tweets_df.to_csv('tweets.csv', index=False)

# Download the CSV (for Colab)
from google.colab import files
files.download('tweets.csv')
```

### Example Output

| User                | Date Created           | Number of Likes | Source of Tweet       | Tweet                                                                                                                                         |
|---------------------|------------------------|-----------------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| Muswema Mukuni      | 2022-12-23 04:38:40    | 0               | Twitter for Android   | Once again, the English are criticising everyone in their world cup defeat except the guy who missed a penalty that would have taken the game...|
| Alia Liverpool       | 2022-12-22 22:07:11    | 0               | Twitter for Android   | Btw the quality of the ref in England is so bad...                                                                                            |
| United Flag Designs | 2022-12-22 21:29:44    | 0               | Twitter for iPhone    | This ref has clearly watched the World Cup and...                                                                                              |
| ...                 | ...                    | ...             | ...                   | ...                                                                                                                                           |
