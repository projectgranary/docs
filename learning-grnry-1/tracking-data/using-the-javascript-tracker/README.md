# JavaScript Tracker

The [Granay JavaScript Tracker](https://github.com/snowplow/snowplow-javascript-tracker/) works in much the same way as JavaScript trackers for other major web analytics solutions including Google Analytics and Omniture. We have tried, as far as possible, to keep the API very close to that used by Google Analytics, so that users who have implemented Google Analytics JavaScript tags have no difficulty also implementing the Granary JavaScript tags.

Tracking is done by inserting JavaScript tags onto pages. These tags run functions defined in [tracker.js](https://github.com/snowplow/snowplow-javascript-tracker/blob/master/src/js/tracker.js), that trigger GET requests of the Snowplow pixel. The JavaScript functions append data points to be passed into Snowplow onto the query string for the GET requests. These then get logged by the Snowplow [collector](https://github.com/snowplow/snowplow/wiki/collectors). For a full list of data points that can be passed into Snowplow in this way, please refer to the [Snowplow tracker protocol](https://github.com/snowplow/snowplow/wiki/snowplow-tracker-protocol) documentation.

The JavaScript Tracker supports both synchronous and asynchronous tags. We recommend the asynchronous tags in nearly all instances, as these do not slow down page load times.

