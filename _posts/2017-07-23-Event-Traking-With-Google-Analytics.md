---
title: " Event Tracking With Google Analytics"
output: github_document
---
Google Analytics provides a powerful function to track both the website performance and visitors'behaviors on the page. The general tracking can give us a brief knowledge of pageview number and some other page loading related number. However, when it comes to specific users behavior such as how many people clicked on or watched a specific video, it is hard to be tracked by default. **Event Tracking**, a function for user behavior tracking in Google Analytics can help uncover the secret.

### 1. Understand the Event 
What is event tracking? Event tracking is a feature of Google Analytics that allows users to track actions if a URL change is recorded. Some event events maybe considered during the tracking process: 
a. Banner Ads
b. Social Links
c. Email
d. Videos

How these events are tracked? It is through passing the following variable information to Google Analytics.
                  **Category, Action,Label and Value**

Then, how the event tracking works is through adding some simple pieces of code to link image, tab, video or text. The result will automatically starts tracking within the dashboard.      

### 2.Implement Event Tracking

How the code looks like? Basically, to track the event, the **_trackEvent()**method needs to be called.

```javascript
_trackEvent(CATEGORY, ACTION, OPT_LABEL, OPT_VALUE, OPT_NONINTERACTION)
```
..*CATEGORY(required) - grouping events into desired tracking groups
..*ACTION(required) - A unique string paired with category and commonly used to define the type of user interaction for web object.
..*OPT_LABEL(optional) - A string to provide additional dimensions to event data.
..*OPT_VALUE(optional) - An integer used to provide numerical data about user event
..*OPT_NONINTERACTION(optional) - A setting to indicate if the event hit will be used in bounce-rate

Let's go deeper of the code. For example, you want to track how users interact with the video(Tagging the Play, Pause and Stop button) you put. In this example, users fire data when they clicks the button. Thus, you will need to put the code, inside the onClick function, like :
```javascript
onClick=”_gaq.push([‘_trackEvent’, ‘videos’, ‘play’, ‘SEO is great’, 5, true]);”
```
or if you want to track the video duration rather than a click action, the code would like:
```javascript
_gaq.push([‘_trackEvent’, ‘videos’, ‘video duration’, ‘SEO is great’, videoDuration]);
```

For more examples, check [The Complete Google analytics Event Tracking Guide Plus 10 Amazing Examples](https://www.koozai.com/blog/analytics/the-complete-google-analytics-event-tracking-guide-plus-10-amazing-examples/)

### 3.Event Tracking Report
After setting up the code, the anlaytcis result will appear on the Google analytics **"BEHAVIOR - EVENTS - xxxx"** report.      

![p1- EventTrackingReport]({{ site.url }}/images/EventT/p1.png "Event Tracking Report ")

In these report you can click to check different reports based on the needs.

#### More Resource       
To master the event tracking process is not easy. It requires some javascript and Tag manager knowledge too. Here I attached some useful Links for people who want to go deeper into this important digital analytics skill.     

[About Event](https://support.google.com/analytics/answer/1033068#Implementation)       
[Event Tracking 101 For Google Analytics](http://searchengineland.com/event-tracking-101-for-google-analytics-93358)         
[Event Tracking for Google Analytics : 3 Step Set up](https://www.gravitatedesign.com/blog/event-tracking-google-analytics/)         
[Event Tracking - Web Tracking(ga.js)](https://developers.google.com/analytics/devguides/collection/gajs/eventTrackerGuide)
[JavaScript Tutorial](https://www.w3schools.com/js/)
[Event tracking with Google Tag Manager](https://support.google.com/analytics/answer/6164470?hl=en)


