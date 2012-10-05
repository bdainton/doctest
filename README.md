---
title: Stream Meta API
class: guide
side_content: >
  <h2><a href="#ref-top">Stream Meta API</a></h2>
  <h3><a href="#ref-resource-url">Resource URL</a></h3>
  <h3><a href="#ref-params">Parameters</a></h3>
  <ul>
    <li><a href="#ref-params-standard">Standard (Counts and Activity)</a></li>
    <li><a href="#ref-params-top-retweeted-tweets">Top Retweeted Tweets</a></li>
    <li><a href="#ref-params-top-hashtags">Top Hashtags</a></li>
    <li><a href="#ref-params-top-links">Top Links</a></li>
    <li><a href="#ref-params-top-contributors">Top Contributors</a></li>
    <li><a href="#ref-params-top-topics">Top Topics</a></li>
    <li><a href="#ref-params-top-moments">Top Moments</a></li>
  </ul>
  <h3><a href="#ref-reqs">Example Requests</a></h3>
  <ul>
    <li><a href="#ref-reqs-standard">Standard (Counts and Activity)</a></li>
    <li><a href="#ref-reqs-top-retweeted-tweets">Top Retweeted Tweets</a></li>
    <li><a href="#ref-reqs-top-hashtags">Top Hashtags</a></li>
    <li><a href="#ref-reqs-top-links">Top Links</a></li>
    <li><a href="#ref-reqs-top-contributors">Top Contributors</a></li>
    <li><a href="#ref-reqs-top-topics">Top Topics</a></li>
    <li><a href="#ref-reqs-top-moments">Top Moments</a></li>
  </ul>
---

## <a href="#ref-top" id="ref-top">Stream Meta API</a>

Provides information about and derived from the entities in a stream.

All streams have, by default, a set of meta information captured about them as they live. Most notably, this information includes...

- **counts**: total number of entities in a stream, split by their moderation state (pending, approved, rejected)
- **activity**: arrays of per-minute, per-hour, and per-day counts

This [basic/standard meta information](#refs-reqs-standard) is commonly used to render such visualizations as counters, progress meters, and social poll results.

Additionally, on a stream-by-stream basis, there are many advanced features that may be turned on (by Mass Relevance administrators). These features include...

- **top retweeted tweets**: (Twitter only) the tweets in a stream that have been retweeted the most<br />
- **top hashtags**: the hashtags that have been used the most over the last hour<br />
- **top links**: the URLs that have been referenced the most over the last 18 hours<br />
- **top contributors**: (Twitter only) the Twitter users whose tweets have been retweeted or replied to the most within the stream<br />
- **top topics**: a tally of the number of times that specific keyword sets have been mentioned in the stream<br />
- **top moments**: an extension to top topics, capturing the minutes (moments) when individual topics peaked in activity<br />

This [advanced stream meta information](#ref-reqs-advances) is commonly used to render various leaderboard visualizations.

### <a href="#ref-resource-url" id="ref-resource-url">Resource URL</a>

`http://tweetriver.com/:account/:stream_name/meta.:format`

The components of this URL are:

- **account**: the stream owner's account name (e.g. `bdainton`)<br />
- **stream_name**: the name of the stream (e.g. `kindle`)<br />
- **format**: `json`, `xml`<sup>1</sup>

**Example URL:** [http://tweetriver.com/bdainton/kindle/meta.json](http://tweetriver.com/bdainton/kindle/meta.json)

### <a href="#ref-params" id="ref-params">Parameters</a>

#### <a href="#ref-params-standard" id="ref-params-standard">Standard</a>

These parameters control the information you get back in the [basic/standard meta response](#ref-reqs-standard).

<table class='params'>
  <tr>
    <td>
      num_minutes
      <div class='note optional'>optional</div>
    </td>
    <td class='desc'>
      Number of minutes of activity to include
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 60</div>
      <div class='note'><strong>Example Value</strong>: 120</div>
      <div class='note'><strong>Notes</strong>: Must be > 0</div>
    </td>
  </tr>
  <tr>
    <td>
      num_hours
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of hours of activity to include
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 24</div>
      <div class='note'><strong>Example Value</strong>: 6</div>
      <div class='note'><strong>Notes</strong>: Must be > 0</div>
    </td>
  </tr>
  <tr>
    <td>
      num_days
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of days of activity to include
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 365</div>
      <div class='note'><strong>Example Value</strong>: 30</div>
      <div class='note'><strong>Notes</strong>: Must be > 0</div>
    </td>
  </tr>
  <tr>
    <td>
      activity
      <div class='note optional'>optional</div>
    </td>
    <td>
      Include `activity` and `count` properties in response
      <div class='note'><strong>Type</strong>: bit</div>
      <div class='note'><strong>Default</strong>: 1</div>
      <div class='note'><strong>Example Value</strong>: 0</div>
      <div class='note'><strong>Notes</strong>: Included/on by default</div>
    </td>
  </tr>
  <tr>
    <td>
      finish
      <div class='note optional'>optional</div>
    </td>
    <td>
      <a href="http://en.wikipedia.org/wiki/Unix_time">Unix time</a> of
the point of which activity data should end. This will be reflected by
the value of the last item of the minute activities.
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: <em>now</em></div>
      <div class='note'><strong>Example Value</strong>: 1349278694</div>
      <div class='note'><strong>Notes</strong>: Only use the seconds portion of
unix time (JavaScript will give use the number in milliseconds.
Divide by 1000).</div>
    </td>
  </tr>
</table>

#### <a href="#ref-params-top-retweeted-tweets" id="ref-params-top-retweeted-tweets">Top Retweeted Tweets

This is an advanced feature<sup>2</sup>.

These parameters control the information you get back when requesting the [top retweeted tweets](#ref-reqs-top-retweeted-tweets) in a stream.

<table class='params'>
  <tr>
    <td>
      top_periods
      <div class='note optional'>optional</div>
    </td>
    <td>
      Include top tweets from specific hourly periods provided. "a" is
all time. "2012070412" is July 4, 2012 at 12PM UTC.
      <div class='note'><strong>Type</strong>: string(ish)</div>
      <div class='note'><strong>Default</strong>: a <em>(all-time)</em></div>
      <div class='note'><strong>Example Value</strong>: a,2012070412</div>
      <div class='note'><strong>Notes</strong>: Accepts a string list delimited
by commas. All hours specified will be in UTC and formatted as <em>yyyyMMddHH</em> (<a
  href="http://developer.android.com/reference/java/text/SimpleDateFormat.html">Java
  SimpleDateFormat</a> pattern<sup>3</sup>). Either `top_periods` or `top_periods_relative` can be used per request.</div>
    </td>
  </tr>
  <tr>
    <td>
      top_periods_relative
      <div class='note optional'>optional</div>
    </td>
    <td>
      Include top tweets from specific hourly periods ago from now. "0"
is right now. "1" is one hour ago.
      <div class='note'><strong>Type</strong>: integer(s)</div>
      <div class='note'><strong>Default</strong>: <em>N/A</em></div>
      <div class='note'><strong>Example Value</strong>: 1,2,3</div>
      <div class='note'><strong>Notes</strong>: Accepts an integer list delimited by commas. Either `top_periods` or `top_periods_relative` can be used per request.
    </td>
  </tr>
  <tr>
    <td>
      top_count
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of tweets to include per period
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 5</div>
      <div class='note'><strong>Example Value</strong>: 12</div>
    </td>
  </tr>
</table>

#### <a href="#ref-params-top-hashtags" id="ref-params-top-hashtags">Top Hashtags</a>

This is an advanced feature<sup>2</sup>.

These parameters control the information you get back when requesting the [top discovered hashtags](#ref-reqs-top-hashtags) in a stream.

<table class='params'>
  <tr>
    <td>
      num_hashtags
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of discovered hashtags to return.
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 10</div>
      <div class='note'><strong>Example Value</strong>: 5</div>
    </td>
  <tr>
</table>

#### <a href="#ref-params-top-links" id="ref-params-top-links">Top Links</a>

This is an advanced feature<sup>2</sup>.

These parameters control the information you get back when requesting the [top discovered URLs/links](#ref-reqs-top-links) in a stream.

<table class='params'>
  <tr>
    <td>
      num_links
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of discovered URLs/links to return.
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 10</div>
      <div class='note'><strong>Example Value</strong>: 5</div>
    </td>
  <tr>
</table>

#### <a href="#ref-params-top-contributors" id="ref-params-top-contributors">Top Contributors</a>

This is an advanced feature<sup>2</sup>.

(Twitter only) This feature tracks the Twitter users whose tweets have been retweeted or replied to the most within the stream.

These parameters control the information you get back when requesting the [top contributors](#ref-reqs-top-contributors) in a stream.

<table class='params'>
  <tr>
    <td>
      num_contributors
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of contributor user handles return.
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 10</div>
      <div class='note'><strong>Example Value</strong>: 5</div>
    </td>
  <tr>
</table>


#### <a href="#ref-params-top-topics" id="ref-params-top-topics">Top Topics</a>

This is an advanced feature<sup>2</sup>.

These parameters control the information you get back when requesting the top topics in a stream.

<table class='params'>
  <tr>
    <td>
      num_trends
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of trends to return in response per bucket
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 30</div>
      <div class='note'><strong>Example Value</strong>: 5</div>
      <div class='note'><strong>Notes</strong>: 5</div>
    </td>
  <tr>
  </tr>
    <td>
      disregard
      <div class='note optional'>optional</div>
    </td>
    <td>
      Exclude trends that match supplied values from buckets while trying to ensure `num_trends`
is met
      <div class='note'><strong>Type</strong>: strong</div>
      <div class='note'><strong>Default</strong>: <em>N/A</em></div>
      <div class='note'><strong>Example Value</strong>: obama,romney</div>
      <div class='note'><strong>Notes</strong>: Accepts a string list delimited by commas</div>
    </td>
  </tr>
</table>

#### <a href="#ref-params-top-moments" id="ref-params-top-moments">Top Moments</a>

This is an advanced feature<sup>2</sup>.

These parameters control the information you get back when requesting the top moments in a stream.

<table class='params'>
  <tr>
    <td>
      top_moments
      <div class='note optional'>optional</div>
    </td>
    <td>
      Include moments from specific hourly periods provided. "a" is
all time. "2012070412" is July 4, 2012 at 12PM UTC.
      <div class='note'><strong>Type</strong>: string(ish)</div>
      <div class='note'><strong>Default</strong>: a <em>(all-time)</em></div>
      <div class='note'><strong>Example Value</strong>: a,2012070412</div>
      <div class='note'><strong>Notes</strong>: Accepts a string list delimited
by commas. All hours specified will be in UTC and formatted as <em>yyyyMMddHH</em> (<a
  href="http://developer.android.com/reference/java/text/SimpleDateFormat.html">Java
  SimpleDateFormat</a> pattern<sup>3</sup>). Either `top_moments` or `top_moments_relative` can be used per request.</div>
    </td>
  </tr>
  <tr>
    <td>
      top_moments_relative
      <div class='note optional'>optional</div>
    </td>
    <td>
      Include moments from specific hourly periods ago from now. "0"
is right now. "1" is one hour ago.
      <div class='note'><strong>Type</strong>: integer(s)</div>
      <div class='note'><strong>Default</strong>: <em>N/A</em></div>
      <div class='note'><strong>Example Value</strong>: 1,2,3</div>
      <div class='note'><strong>Notes</strong>: Accepts an integer list delimited by commas. Either `top_moments` or `top_moments_relative` can be used per request.
    </td>
  </tr>
  <tr>
    <td>
      top_moments_count
      <div class='note optional'>optional</div>
    </td>
    <td>
      Number of moments to include per period
      <div class='note'><strong>Type</strong>: integer</div>
      <div class='note'><strong>Default</strong>: 1</div>
      <div class='note'><strong>Example Value</strong>: 5</div>
    </td>
  </tr>
</table>


### <a href="#ref-reqs" id="ref-reqs">Example Requests</a>

#### <a href="#ref-reqs-standard" id="ref-reqs-standard">Standard Meta Information</a>

To get a stream's basic/standard count and activity rate information...

  `$ curl http://tweetriver.com/bdainton/kindle.json`

The response:

    ```json
    {
      "name": "kindle",
      "full_name": "bdainton/kindle",
      "description": "The Twitterverse has some good things to say about the Amazon Kindle.",
      "created_at": "2010-09-21T19:18:20Z",
      "count": {
        "total": 19958828,
        "rejected": 14238898,
        "pending": 5704155,
        "approved": 15775
      },
      {
        "minute": {  // arrays of minute-by-minute counts
          "total": [],
          "rejected": [],
          "pending": [],
          "approved": []
        },
        "hourly": { // arrays of hour-by-hour counts
          "total": [],
          "rejected": [],
          "pending": [],
          "approved": []
        },
        "daily": { // arrays of day-by-day counts
          "total": [],
          "rejected": [],
          "pending": [],
          "approved": []
        }
      }
    }

#### <a href="#ref-reqs-advanced" id="ref-reqs-advanced">Advanced Meta Information</a>

A stream with other advanced features enabled will have far more information contained in its meta response.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json`

The response:

    ```json
    {
      // standard details
      "name": "things-we-do",
      "full_name": "MassRelDemo/things-we-do",
      "description": "This is what people are doing on Twitter.",
      "created_at": "2012-09-26T15:07:07Z",
      "count": {
        "total": 6210,
        "rejected": 314,
        "pending": 1260,
        "approved": 4636
      },
      "activity": {
        "daily": {},
        "hourly": {},
        "minute": {}
      },

      // top retweeted tweets
      "top": {},

      // a description of the time periods that have been returned in the above 'top' object
      "top_periods": [],

      // top hashtags
      "hashtags": [],

      // top links
      "links": [],

      // top contributors
      "contributors": [],

      // top topics
      "buckets": {},

      // top moments
      "moments": {},

      // a description of the time periods returned in the above 'moments' object
      "moments_periods": []
    }


#### <a href="#ref-reqs-top-retweeted-tweets" id="ref-reqs-top-retweeted-tweets">Advanced Meta Information: Top Retweeted Tweets</a>

Get the top retweeted tweets in this stream.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json?top_periods=a,20120926,2012092616&top_count=3`

In this example, we're asking for the top 3 retweeted tweets in each for 3 specific time periods: all-time in this stream ('a'), within a specific day (Sept 26, 2012), and within a specific hour of that day (UTC 16:00, Sept 26, 2012).

The response:

    ```json
    {
      // ... for this example, other meta details have been removed ...

      // top retweeted tweets
      "top": {
        "20120926": [
          {
            // ...top retweeted tweet 1 of the UTC Day Sept 26, 2012...
          },
          {
            // ...top retweeted tweet 2...
          },
          {
            // ...top retweeted tweet 3...
          }
        ],
        "2012092616": [
          {
            // ...top retweeted tweet 1 of the 16:00 UTC Hour Sept 26, 2012...
          },
          {
            // ...top retweeted tweet 2...
          },
          {
            // ...top retweeted tweet 3...
          }
        ],
        "a": [
          {
            // ...top retweeted tweet 1 of all time in this stream...
          },
          {
            // ...top retweeted tweet 2...
          },
          {
            // ...top retweeted tweet 3...
          }
        ]
      },

      // a description of the time periods that have been returned in the above 'top' object
      "top_periods": [
        "a",
        "20120926",
        "2012092616"
      ]
    }


#### <a href="#ref-reqs-top-hashtags" id="ref-reqs-top-hashtags">Advanced Meta Information: Top Hashtags</a>

Get the top discovered hashtags in this stream.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json?num_hashtags=5`

The <em>count</em> indicated in the response indicates the number of approved entities <em>in the last hour</em> using that hashtag.

The response:

    ```json
    {
      // ... for this example, other meta details have been removed ...

      // top hashtags
      "hashtags": [
        {
          "hashtag": "callmeoldfashioned",
          "count": 2409
        },
        {
          "hashtag": "m312",
          "count": 911
        },
        {
          "hashtag": "oomf",
          "count": 385
        },
        {
          "hashtag": "imhappiestwhen",
          "count": 385
        },
        {
          "hashtag": "lwwy",
          "count": 380
        }
      ]
    }

#### <a href="#ref-reqs-top-links" id="ref-reqs-top-links">Advanced Meta Information: Top Links</a>

Get the top discovered links in this stream.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json?num_links=5`

The <em>count</em> indicated in the response indicates the number of approved entities <em>in the last 18 hours</em> containing that link.

The response:

    ```json
    {
      // ... for this example, other meta details have been removed ...

      // top links
      "links": [
        {
          "count": 2194,
          "link": "http://youtu.be/jvHE70LZEOA"
        },
        {
          "count": 1079,
          "link": "http://viddy.it/PGptsX"
        },
        {
          "count": 360,
          "link": "http://www.massrelevance.com"
        },
        {
          "count": 300,
          "link": "http://x.co/oFxV"
        },
        {
          "count": 287,
          "link": "http://twitpic.com/ay2x06"
        }
      ]
    }

#### <a href="#ref-reqs-top-contributors" id="ref-reqs-top-contributors">Advanced Meta Information: Top Contributors</a>

Get the top contributors in this stream.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json?num_contributors=5`

The <em>count</em> indicated in the response indicates the number of approved entities <em>in the last week</em> containing that user's handle (@name).

The response:

    ```json
    {
      // ... for this example, other meta details have been removed ...

      // top links
      "contributors": [
        {
          "count": 39668,
          "name": "@techcrunch"
        },
        {
          "count": 21641,
          "name": "@mashable"
        },
        {
          "count": 8373,test
          "name": "@intel"
        },
        {
          "count": 2768,
          "name": "@wired"
        },
        {
          "count": 2518,
          "name": "@drizzled"
        }
      ]
    }

#### <a href="#ref-reqs-top-topics" id="ref-reqs-top-topics">Advanced Meta Information: Top Topics</a>

Get the top constrained topics in this stream.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json?num_trends=5`

The <em>count</em> indicated in the response indicates the number of approved entities <em>all-time and in the last 10 minutes</em> containing that topic's keywords.

The response:

    ```json
    {
      // ... for this example, other meta details have been removed ...

      // top topics
      "buckets": {
        "top": [],
        "count": []
      }
    }

#### <a href="#ref-reqs-top-moments" id="ref-reqs-top-moments">Advanced Meta Information: Top Moments</a>

Get the top moments in the stream.

  `$ curl http://tweetriver.com/MassRelDemo/things-we-do/meta.json?top_moments=a&top_moments_relative=1,2`

The <em>count</em> indicated in the response indicates the number of approved entities <em>all-time and in the last 10 minutes</em> containing that topic's keywords.

The response:

    ```json
    {
      // ... for this example, other meta details have been removed ...

      // top moments
      "moments": {
        // TODO
      }
    }

- Footnote<sup>1</sup> -- Our docs only cover the `json` API, but the `xml` endpoint supports the same query parameters as the `json` endpoint and a similar response to the `json` endpoint.
- Footnote<sup>2</sup>: -- This feature is enabled on a stream by stream basis, by a Mass Relevance administrator (upon request). By default, this feature is not enabled.
- Footnote<sup>3</sup> -- I like Android's docs better for this class than Oracle's.