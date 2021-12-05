# MetaCTF 2021
## Online Presence (550pts)
### Description: 
>Sources say Vedder may have used Reddit the past two weeks. It would make sense for him to post some comments on some major subreddits related to his main career interests or near his local area. Can you help find his account?
The answer format is the username followed by the post ID and comment ID in the URL to their shortest comment, underscore delimited. For example, for [this comment](https://www.reddit.com/r/AskReddit/comments/r5mmdj/comment/hmnszk4/), the answer would be MetaCTF{Specialistimran_r5mmdj_hmnszk4}.
Hint: There's a logical way to solve this challenge that doesn't require much time. There are two major (both have 500k+ subscribers) work-related subreddits.

### Step 1 - Baseline OSINT:
We need to dig up some information on Vedder himself, his interests, and his career to begin to piece together where he could be on Reddit.
By doing some quick research we can find a Twitter account and a personal website:
>https://veddercasyn.me/

>https://twitter.com/veddercasyn

Knowing he's in Hammond, IN, who's closest metro area is Chicago, and his interests in fitness and nutrition, we can begin to piece together some places he might be.

### Step 2 - Data Collection:
We built out a list of the most common subreddits using this site, sort by 'SFW':
>https://frontpagemetrics.com/top-sfw-subreddits

We figured in a challenge like this, Vedder would not be posting in NSFW subreddits and his presence would be relatively clean.
We filtered the subreddits that had more than 500k subscribers, but kept some that were very pertinent, like /r/Chicago.

With a list of the subreddits, we could now either triangulate him manually... which would take forever, or use a nifty tool called [Google Colaboratory](https://research.google.com/colaboratory/) for doing 'Big Data' queries. (Not such big data in this case)

### Step 3 - Triangulation with Google Colab
First, we need to get the Reddit API Wrapper set up with Google Colab to crawl these subreddits. Set up a Reddit account and fill in the API:
```python
pip install praw
---------------------------------------------------------------
import praw

reddit = praw.Reddit(
    user_agent="Comment Extraction (by u/AccountYouSetUpToFindVedder)",
    client_id="x",
    client_secret="y",
    username="AccountYouSetUpToFindVedder",
    password="PasswordYouSetUpToFindVedder",
    check_for_async=False
)
```
Then we have can gather submissions from subreddits and store usernames into an array. We began by collecting usernames from the last 150 fitness posts:
```python
submissions = reddit.subreddit("fitness").new(limit=150)
cusernames1 = []

i = 1
for submission in submissions:
  redditor1 = submission.author
  print("~~~~~~~~~~~~~~~Submission " + str(i) + "     ID= " + submission.id + "     Title: " + submission.title + "~~~~~~~~~~~~~~~")
  print("Posted by:" + str(redditor1.name))
  submission.comments.replace_more(limit=None)
  i=i+1
  for comment in submission.comments.list():
    cusernames1.append(comment.author)
    print("ID: " + comment.id + ";     " + str(comment.author) + ":   " + comment.body)
  print("\n")


cu1set = set(cusernames1)
set(cuset2).intersection(cu1set)
  
```
This is basically all of the code required, we just need to try different combinations of subreddits and find the users that are posting comments in each one. Store the users in different arrays and intersect those sets to filter it down.

### Step 4 - Some Hits!
When we were filtering between /r/bodybuilding, /r/fitness, and other health-related subreddits, there were tons of users in the list to go through. Who would've thought people who comment in one subreddit comment in another related one?
We decided it needed to be filtered down to the metro area, since we were getting very few users who posted in a city and a health subreddit.
Eventually, after intersecting /r/fitness and /r/Indiana, we had only two users to look at:
>{Redditor(name='Dptwin'), None, Redditor(name='dtdnumberone')}

Dptwin questionable content, but dtdnumberone was enticing. It's reddit birthday was Nov 30, 5 days before this challenge. It had very few posts, and all were within fitness, indiana, chicago, chicagosuburbs, and related subreddits.
We were confident this was Vedder, and now just had to fulfill the flag formatting.

### Step 5 - Generate the Flag
The flag was stipulated to be the username, then post ID, then comment ID of his shortest comment.
That comment would be:
<br /><br />
![image](https://user-images.githubusercontent.com/43623870/144759695-a6a5c992-75d0-45b4-abf9-0c08008a854f.png)

Going to the exact comment URL gives us the following information:
```
Username: dtdnumberone
Post ID: r4rant
Comment ID: hmqpa52
```
Therefore our flag is
<details>
  <summary>Flag Spoiler:</summary>
  
    MetaCTF{dtdnumberone_r4rant_hmqpa52}
 </details>

