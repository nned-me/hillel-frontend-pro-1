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







