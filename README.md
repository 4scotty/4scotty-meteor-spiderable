# spiderable-longer-timeout

`spiderable` is part of [Webapp](https://www.meteor.com/webapp). It's
one possible way to allow web search engines to index a Meteor
application. It uses the [AJAX Crawling
specification](https://developers.google.com/webmasters/ajax-crawling/)
published by Google to serve HTML to compatible spiders (Google, Bing,
Yandex, and more).

When a spider requests an HTML snapshot of a page the Meteor server runs the
client half of the application inside [phantomjs](http://phantomjs.org/), a
headless browser, and returns the full HTML generated by the client code.

In order to have links between multiple pages on a site visible to spiders, apps
must use real links (eg `<a href="/about">`) rather than simply re-rendering
portions of the page when an element is clicked. Apps should render their
content based on the URL of the page and can use [HTML5
pushState](https://developer.mozilla.org/en-US/docs/DOM/Manipulating_the_browser_history)
to alter the URL on the client without triggering a page reload. See the [Todos
example](http://meteor.com/examples/todos) for a demonstration.

When running your page, `spiderable` will wait for all publications
to be ready. Make sure that all of your [`publish functions`](#meteor_publish)
either return a cursor (or an array of cursors), or eventually call
[`this.ready()`](#publish_ready). Otherwise, the `phantomjs` executions
will fail.

## Notes

This is a branch of the standard meteor spiderable, with some merged code from
ongoworks:spiderable. Primarily, this lengthens the timeout to 30 seconds and
size limit to 10MB. In addition, it attempts to deal with the /dev/stdin bug, which
seems to break phantom on some servers.

In addition, it waits for Iron Router to complete routing.

*Important*
You will need to set *Meteor.isRouteComplete= true* when your route is finished, in order to publish.

I also set Spiderable.originalRequest to the http request. See [issue 1](https://github.com/jazeee/jazeee-meteor-spiderable/issues/1).

If you deploy your application with `meteor bundle`, you must install
`phantomjs` ([http://phantomjs.org](http://phantomjs.org/)) somewhere in your
`$PATH`. If you use `meteor deploy` this is already taken care of.
