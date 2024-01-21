# Network-Clean-and-analyze-social-media-usage-data-with-Python
import pandas as pd
import tweepy

consumer_key = "I72QtV3p7fsCrm3j8j00BCPtt"  # Your API/Consumer key
consumer_secret = "yoFsqHngxhudGrOylZRpuuXaGIrEZ399DzH5CHLP7bvsQfSggG"  # Your API/Consumer Secret Key
access_token = "1448227792659705857-HBjXfgOVLctL4EBo0A8CwItZXHlFI9"  # Your Access token key
access_token_secret = "46i1POTGjBpagHBAqkDMmHSCF3mVB6fuu5PKsGEYezAbz"  # Your Access token Secret key

# Pass in our Twitter API authentication key
auth = tweepy.OAuth1UserHandler(
    consumer_key, consumer_secret,
    access_token, access_token_secret
)

# Instantiate the Tweepy API
api = tweepy.API(auth, wait_on_rate_limit=True)

# Define the topics you want to search for
topics = ["food", "family", "life", "fitness", "tech", "family", "beauty"]

# Set the limit for the number of tweets to fetch
limit = 50000

# Initialize an empty list to store tweet data
tweets_data = []

# Loop through each topic and fetch tweets
for topic in topics:
    search_query = f"{topic} -filter:retweets AND -filter:replies AND -filter:links"
    
    # Fetch tweets for the current topic
    try:
        for tweet in tweepy.Cursor(api.search, q=search_query, lang="en", tweet_mode="extended").items(limit):
            tweet_data = {
                "User": tweet.user.screen_name,
                "Date Created": tweet.created_at,
                "Number of Likes": tweet.favorite_count,
                "Source of Tweet": tweet.source,
                "Tweet": tweet.full_text
            }
            tweets_data.append(tweet_data)
    
    except Exception as e:
        print(f"Error fetching tweets for '{topic}': {e}")

# Create a DataFrame from the collected tweet data
tweets_df = pd.DataFrame(tweets_data)

# Display the first few rows of the DataFrame
print(tweets_df.head())
