# Tracking across Id-Borders using Granary

As each event send to Granary needs a correlation id, so that an associated profile can be identified or created, problems arrise when the user behaviour spans accross multiple tracking domains \(i.e. when the user starts their journey on a web browser on their desktop machine and later switches to a mobile app, whithout having to log in\).  
We will show you hands on how such a switch can be tracked and the respective profiles linked using the Snowplow Tracker in its JS and Android implementation. 

## Example Web to App Switch Tracking Case 

To track the user accross technological boundaries we need to do a few things:

* tracking needs to be embeded on both sides of the divide
* create a flow that the user is likely to follow \(i.e. providing a QR-code\)
* create a Link-ID that is part of the flow from one side to the other
* create two\(+\) belts that perform the linking of three profiles \(i.e. the Web Profile, the App profile and the Link-ID Profile\)

#### Tracking Example using Snowplow

Here is an example [page](http://grnry.pages.alvary.io/demo-app-sprung-tracking/) that uses the Snowplow tracker and provides links as a means to download and install an app. In this example the event flow happens in the following stages:

1. A Pageview Event is generated \(simulating normal tracking, without relevancy of the link\)
2. A random number is generated as a Link-ID and shown on the webpage.
3. Two links are generated, one download link using the [Snowplow redirect](https://github.com/snowplow/snowplow/wiki/Pixel-tracker) and one [deeplink into the andoid app](https://developer.android.com/training/app-links/deep-linking), both using the Link-ID as a parameter.
4. A Structured Event is generated that uses the Link-ID as a value - thereby creating a link that can be followed from the Snowplow Cookie ID to the Link-ID. 
5. When the user now uses th first link to download the app they are send to the snowplow collector and an event is created with parameters set on creation of the link, including the Link-ID. Through this we are able to pinpoint the download to a specific ID \(the Link-ID\) and further \(by following the link to the Snowplow Cookie ID\) to the user. Using the User agent of the request we are able to distinguish whether the donwload was performed on a desktop machine or a mobile device.
6. Once the app is installed the user can use the second link provided to open the app on a specific activity/screen along with parameters we provided. In our case this is the Link-ID. Once the Link-ID is received by the app another Structured Event is send providing the Link-ID and the Snowplow App ID \(used for normal in-app tracking\) 
7. And at last, a Screen View Event is send \(simulating normal App tracking, without relevancy of the link\)

#### Viewing the events in the Event Store \(ToDo\)

#### Creating Belts for our Link \(ToDo\)

#### Viewing the links in the Profiles using our API \(ToDo\)

####  



