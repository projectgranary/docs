# GA Tracker

Snowplow allows you to seamlessly collect Google Analytics events without interfering with your Google Analytics setup.

It does so by teeing your Google Analytics payloads to a Snowplow collector through [the Snowplow Google Analytics plugin](https://github.com/snowplow/snowplow/wiki/plugin).

If you would like to benefit from this feature, it is fairly straightforward to setup this plugin:

```markup
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-12345-1', 'auto');
  ga('require', 'spGaPlugin', { endpoint: 'https://api.grnry.io' });
  ga('send', 'pageview');
</script>
<script async src="https://d1fc8wv8zag5ca.cloudfront.net/sp-ga-plugin/0.1.0/sp-ga-plugin.js"></script>
```

Note that only two lines differ from the usual Google Analytics setup:

* `ga('require', 'spGaPlugin', { endpoint: 'https://api.grnry.io' });`  which will instantiate our plugin configured to tee requests to the [https://events.acme.com](https://events.acme.com/) Snowplow collector endpoint. 
* `<script async src="https://d1fc8wv8zag5ca.cloudfront.net/sp-ga-plugin/0.1.0/sp-ga-plugin.js"></script>` which will load our plugin code.

Once the plugin has been set up you Google Analytics events will start flowing to both Google Analytics and your Snowplow pipeline.

