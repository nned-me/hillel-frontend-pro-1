Lesson points
 - PWA
 
 ## PWA
 
**Progressive**
**Web**
**Applications**

Web app that appears and works like native app. 
Under the hood it launches browser in fullscreen mode and loads site. It can work offline due to **service workers**

![image](https://web-dev.imgix.net/image/tcFciHGuF3MxnTr1y5ue01OGLBn2/1DKtUFjXLJbiiruKA9P1.svg)


PROs:
- one codebase for iOS / Andriod.
- miniscule install size (an installed PWA usually takes less than 1MB).
- instant updating
- no app store involved
- no 30% fees on monetizing

CONS:
- no native app stuff
- not common for users
- some features for native apps may be unavailable for PWA

Main **PWA** principles:
- **Discoverable**, so the contents can be found through search engines.
- **Installable**, so it can be available on the device's home screen or app launcher.
- **Linkable**, so you can share it by sending a URL.
- **Network independent**, so it works offline or with a poor network connection.
- **Progressively enhanced**, so it's still usable on a basic level on older browsers, but fully-functional on the latest ones.
- **Re-engageable**, so it's able to send notifications whenever there's new content available.
- **Responsively designed**, so it's usable on any device with a screen and a browser—mobile phones, tablets, laptops, TVs, refrigerators, etc.
- **Secure**, so the connections between the user, the app, and your server are secured against any third parties trying to get access to sensitive data.


## PWA structure
Main components:
- **Web Manifest**
- **Service Worker**
- **Icon**
- **App shell**


## Web Manifest
```
	<link rel="manifest" href="js13kpwa.webmanifest">
```

```
{
  "background_color": "purple",
  "description": "Shows random fox pictures. Hey, at least it isn't cats.",
  "display": "fullscreen",
  "icons": [
    {
      "src": "icon/fox-icon.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ],
  "name": "Awesome fox pictures",
  "short_name": "Foxes",
  "start_url": "/pwa-examples/a2hs/index.html"
}
```

Little bigger example:
```
{
  "short_name": "Weather",
  "name": "Weather: Do I need an umbrella?",
  "icons": [
    {
      "src": "/images/icons-vector.svg",
      "type": "image/svg+xml",
      "sizes": "512x512"
    },
    {
      "src": "/images/icons-192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/images/icons-512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": "/?source=pwa",
  "background_color": "#3367D6",
  "display": "standalone",
  "scope": "/",
  "theme_color": "#3367D6",
  "shortcuts": [
    {
      "name": "How's weather today?",
      "short_name": "Today",
      "description": "View weather information for today",
      "url": "/today?source=pwa",
      "icons": [{ "src": "/images/today.png", "sizes": "192x192" }]
    },
    {
      "name": "How's weather tomorrow?",
      "short_name": "Tomorrow",
      "description": "View weather information for tomorrow",
      "url": "/tomorrow?source=pwa",
      "icons": [{ "src": "/images/tomorrow.png", "sizes": "192x192" }]
    }
  ],
  "description": "Weather forecast information",
  "screenshots": [
    {
      "src": "/images/screenshot1.png",
      "type": "image/png",
      "sizes": "540x720"
    },
    {
      "src": "/images/screenshot2.jpg",
      "type": "image/jpg",
      "sizes": "540x720"
    }
  ]
}
```


## "Installation"
In Chrome, your Progressive Web App must meet the following criteria before it will fire the beforeinstallprompt event and show the in-browser install promotion:

- The web app is not already installed
- Meets a user engagement heuristic
- Be served over HTTPS
- Includes a web app manifest that includes:
```
	short_name or name
	icons - must include a 192px and a 512px icon
	start_url
	display - must be one of fullscreen, standalone, or minimal-ui
	prefer_related_applications must not be present, or be false
```
- Registers a service worker with a fetch handler

Then an add to desktop / home page button will appear.


## Workbox

- Precaching
- Runtime caching
- Strategies
- Request routing
- Background sync
- Helpful debugging

Just import it in service worker:
```
	importScripts('https://storage.googleapis.com/workbox-cdn/releases/3.6.1/workbox-sw.js');
```



### Routing
This is handy tool to match urls. Like this:

```
	workbox.routing.registerRoute(
	  'https://some-other-origin.com/logo.png',
	  handler
	);
```

```
	workbox.routing.registerRoute(
	  /\.(?:js|css|webp|png|svg)$/,
	  new workbox.strategies.StaleWhileRevalidate()
	);
```

With callback function:
```
	workbox.routing.registerRoute(
	  ({url, request, event}) => {
		  return (url.pathname === '/special/url');
	},
	  new workbox.strategies.StaleWhileRevalidate()
	);

```

With Navigation
```
	import {createHandlerBoundToURL} from 'workbox-precaching';
	import {NavigationRoute, registerRoute} from 'workbox-routing';

	// This assumes /app-shell.html has been precached.
	const handler = createHandlerBoundToURL('/app-shell.html');
	const navigationRoute = new NavigationRoute(handler, {
	  allowlist: [
	    new RegExp('/blog/'),
	  ],
	  denylist: [
	    new RegExp('/blog/restricted/'),
	  ],
	});
	registerRoute(navigationRoute);
```

For non-GET methods:
```
	import {registerRoute} from 'workbox-routing';

	registerRoute(
	  matchCb,
	  handlerCb,
	  'POST'
	);
	registerRoute(
	  new RegExp('/api/.*\\.json'),
	  handlerCb,
	  'POST'
	);
```

### Strategies

- Network first
- Cache first
- Network only
- Cache only
- Stale While Revalidate


```
workbox.routing.registerRoute(
  /\.(?:js|css)$/,
  workbox.strategies.staleWhileRevalidate({
    cacheName: 'static-resources',
  })
);
```


#### Network first
![image](https://miro.medium.com/max/875/1*A888BY4bpbhqs6oFQ8Zlog.png)
This will try and get a request from the network first. If it receives a response, it’ll pass that to the browser and also save it to a cache. 

If the network request fails, the last cached response will be used. A real case for this strategy would be when are network requests that would be beneficial if they were served from the network but could benefit by being served by the cache if the network request is taking too long. For this, you can use a NetworkFirst strategy with the networkTimeoutSeconds option configured. Example:

#### Cache first
![image](https://miro.medium.com/max/875/1*X9DMICiwtZYuby7krAs-mA.png)

This strategy will check the cache for a response first and use that if one is available. If the request isn’t in the cache, the network will be used and any valid response will be added to the cache before being passed to the browser. This is the typical for images.

### Cache only

![image](https://miro.medium.com/max/875/1*jLXSwOud71Xe2MNFxHExxg.png)

Force the response to come from the cache. This is the when it’s considered static to that “version” of a site. It should have cached these in the service worker install event. 

### Network only

![image](https://miro.medium.com/max/875/1*SFPiT968YlM6wf4gkTs_Zg.png)

- force the response to come from the network.
- this case is more related to non-GET requests (for instance a Form Post to a backend).

#### Stale While Revalidate
![image](https://miro.medium.com/max/875/1*x6aD2__d8oTCYCZcrwPtiA.png)
This strategy will use a cached response for a request if it is available and update the cache in the background with a response from the network. (If it’s not cached it will wait for the network response and use that). This is a fairly safe strategy as it means users are regularly updating their cache. The downside of this strategy is that it’s always requesting an asset from the network, using up the user’s bandwidth. This case would be when caching Javascript or CSS.


```
workbox.routing.registerRoute(
  'https://hacker-news.firebaseio.com/v0/api',
  workbox.strategies.networkFirst({
      networkTimeoutSeconds: 3,
      cacheName: 'stories',
      plugins: [
        new workbox.expiration.Plugin({
          maxEntries: 50,
          maxAgeSeconds: 5 * 60, // 5 minutes
        }),
      ],
  })
);
```
