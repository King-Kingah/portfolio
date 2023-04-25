---
title: "Simple Twitter Bot to Retweet & Favorite Using Python & Tweepy"
description: "How I developed a Simple Twitter Bot to Retweet & Favorite Using Python & Tweepy"
dateString: Feb 14, 2022
draft: false
tags: ["Python", "AWS", "API", "Tweepy", "Twitter"]
weight: 106
cover:
image: "/blog/aws-clf-certification/cover.jpg"
---


# Introduction

I came across the #TwitterBotChallengeKE while scrolling through Twitter's great tech communities. As a genuinely curious individual, I decided to give it a try. After a few google searches, I discovered quite a few resources and at least got a clue on where to start. This is a great way to test and flex your coding skills, while interacting with APIs and learning how they work. It's also fascinating to see what you build in action, especially if you're not very active on Twitter. There's a rich pool of resources from around the web and a supportive Twitter Community making it beginner friendly for all codenewbies.

# The Twitter Bot Challenge (#BuildWhat'sNext)

The Twitter Bot Challenge aims to support the tech ecosystem in Kenya to build using the Twitter API. We will be using the API to build for fun by creating bots. This is a summary of my experience participating, the good, the bad and the ugly, and most of all what you learned.

The goal is to create a twitter bot which tweets Dev, Linux, and other Coding content. Primarily, I decided to develop the bot using Python as it has several handy libraries and tools. As always, it is safer to code new python projects within virtual environments.

### Requirements:

- Twitter Developer Account
- Python3+
- Python virtual environment
- Hosting Server or Cloud Hosting

## Twitter Developer Account

Create a Twitter developer account. I decided to play it safe and create a separate account from my main Twitter account ([@Kingah254](https://twitter.com/Kingah254)). I’ve had a few accounts suspended in the past, I had to be cautious. This is a fairly easy processes and approval takes a few minutes.

Search for Twitter Developer Account on any search engine of your choice and choose the first option (should be). This will redirect you to a Developer Portal. Create a new application under the **Projects & Apps** section.

Request for a Twitter Developer Account and provide a few answers:

![](/blog/docker-flask/img2.png#center)

Create an application:

![](/blog/docker-flask/img2.png#center)

Request for privileged permissions under the User Authentication Settings. I decided to use *OAuth 1.0a*:

![](/blog/docker-flask/img2.png#center)

Select the Read and write and Direct message App permissions:

![](/blog/docker-flask/img2.png#center)

Generate API Key, API Key Secret, Access Token and Access Token Secret:

![](/blog/docker-flask/img2.png#center)

## Python Virtual Environment

For this project, we will use Python3.8, and primarily the Tweepy Python package to develop a simple Twitter bot that retweets, and favorites tweets with certain keywords.

Create a virtual environment to install all the necessary modules using pip, separate and without interfering with the system(global) packages. This can be done by running the command:

```terminal
python3 -m venv yourvirtualenv
```

Activate the Virtual environment to install the libraries:

```terminal
source /yourvirtualenv/bin/activate
```

Install the following packages/libraries:

- *tweepy* - this is the primary python package the bot uses to communicate or interface with the Twitter API. It has a rich module and function libraries in addition to a supportive community. This makes it the most popular package for automating Twitter.

```
pip install tweepy
```

- *configparser* - used to hide important credentials such as api key, api key secret, access token and access token secret. Therefore, it is safer when sharing the code with others as one does not have to share these credentials.

```
pip install configparser
```

- *time* - a Python module that is used to represent time in different ways including objects, numbers and strings as well as offering various functionality such as waiting during code execution.

```
pip install time
```

# Let's Code

Create a configuration file, `config.ini` to store the authentication credentials i.e. the API keys and access tokens.

```
In Config.ini

[jk_bot]
api_key=<Your API Key here>
api_key_secret=<Your API Key Secret Here>

access_token=<Your Access Token Here>
access_token_secret=<Your Access Token Secret here>
```

![](/blog/docker-flask/img2.png#center)

We’ll start by importing the dependencies:


```python3
import tweepy
import configparser
from time import sleep

#ConfigParser is used to grab the authentication credentials from a config 
#file making it safer when sharing the code.
config = configparser.ConfigParser()
config.read('config.ini')

apiKey = config['jk_bot']['apiKey']
apiKeySecret = config['jk_bot']['apiKeySecret']

accessToken = config['jk_bot']['accessToken']
accessTokenSecret = config['jk_bot']['accessTokenSecret']
```

Next, we will instantiate the authentiacation handler using the API key and API key secret and then use the access token and access token secret to complete the authentication process on the API authentication handler. Afterwards, we instantiate a new tweepy API object using the authentication handler object.

```python3
import tweepy
import configparser
from time import sleep

#ConfigParser is used to grab the authentication credentials from a config 
#file making it safer when sharing the code.
config = configparser.ConfigParser()
config.read('config.ini')

apiKey = config['jk_bot']['apiKey']
apiKeySecret = config['jk_bot']['apiKeySecret']

accessToken = config['jk_bot']['accessToken']
accessTokenSecret = config['jk_bot']['accessTokenSecret']

# authenticate API keys 
auth = tweepy.OAuthHandler(apiKey, apiKeySecret)
auth.set_access_token(accessToken, accessTokenSecret)
print('Authentication successful!')

api = tweepy.API(auth)
```

The above code should return an *Authentication Successful!* message.

Next, assign favorite and follow and create a list of the keywords to react by to tweets. After which we create a function using tweepy cursor and search to get the tweets and the usernames. The main function also defines the logic of actions for the bot. If the bot has not retweeted a certain tweet, favorite and retweet it using the `favorite()` and `retweet()` methods available to the tweet object.

```python3
import tweepy
import configparser
from time import sleep

#ConfigParser is used to grab the authentication credentials from a config 
#file making it safer when sharing the code.
config = configparser.ConfigParser()
config.read('config.ini')

apiKey = config['jk_bot']['apiKey']
apiKeySecret = config['jk_bot']['apiKeySecret']

accessToken = config['jk_bot']['accessToken']
accessTokenSecret = config['jk_bot']['accessTokenSecret']

# authenticate API keys 
auth = tweepy.OAuthHandler(apiKey, apiKeySecret)
auth.set_access_token(accessToken, accessTokenSecret)
print('Authentication successful!')

api = tweepy.API(auth)

# assign to favorite and follow
Favorite = True
Follow = True

hashtags = ['#python']

# the function with the logic on the bot actions
def main():
    for tweet in tweepy.Cursor(
    api.search_tweets, q=hashtags).items():
        try:
            print("\nTweet by: @" + tweet.user.screen_name)
            
            # check if we have retweeted and retweet if not
            if not tweet.retweeted:
                try:
                    tweet.retweet()
                    print("Tweet retweeted!")
                except Exception as e:
                    print(e)
            
            # check if we have favorited and favorite if not
            if not tweet.favorited:
                try:
                    tweet.favorite()
                    print("Tweet favorited!")
                except Exception as e:
                    print(e)
            
            # bot sleep time (seconds)
            sleep(60)
        
        except tweepy.TweepyError as e:
            print(e.reason)
        
        
        except StopIteration:
            break


while True:
    main()
    sleep(20)
```

Once completed, we can run the bot. Here is a screen capture of the bot in action:

![](/blog/docker-flask/img2.png#center)

# Conclusion

This is a simple Twitter bot and there is a lot one can do using Tweepy. It is possible to add more logic using several other methods to implement more functionalities.
Next goal is to host the bot on the cloud to ensure it runs throughout. Follow for a follow-up article.