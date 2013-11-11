Project: /chrome/mobile/_project.yaml
Book: /chrome/mobile/_book.yaml

# WebView for Android

Android 4.4 (KitKat) includes a new WebView component based on the Chromium
open source project. The new WebView includes an updated version of the V8
JavaScript engine and support for modern web standards that were missing in 
the old WebView. It also shares the same rendering engine as Chrome for Android,
so rendering should be much more consistent between the WebView and Chrome.

If you're a web developer looking to start developing a WebView-based Android application, see [Getting Started: WebView-based Applications for Web Developers](/chrome/mobile/docs/webview/gettingstarted).

For tips on scaling WebView content for mobile devices, see [Pixel-Perfect UI in the WebView](/chrome/mobile/docs/webview/pixelperfect). 

The new WebView also supports [remote debugging](/chrome-developer-tools/docs/remote-debugging)
using the Chrome DevTools.

[[TOC]]

## WebView FAQ

### What version of Chrome is it based on?

The WebView shipped with Android 4.4 (KitKat) is based on the same code as
Chrome for Android version 30.  The WebView does not have full feature parity with 
Chrome for Android and is currently given the version number 30.0.0.0.

### What is the default user-agent?

The new WebView adds **Chrome/_version_** to the user-agent string. Sample
old and new user-agent strings:

* **Old UA**: Mozilla/5.0 (Linux; U; Android 4.1.1; en-gb; Build/KLP) 
  AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Safari/534.30

* **New UA**: Mozilla/5.0 (Linux; Android 4.4; Nexus 5 Build/_BuildID_) 
  AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/30.0.0.0 Mobile 
  Safari/537.36

If you're attempting to differentiate between the WebView and Chrome for Android, 
you should look for the presence of the **Version/_X.X_** string in the WebView
user-agent string. Don't rely on the specific Chrome version number, 
**30.0.0.0** as this may change with future releases.

### How do I set the user-agent of the WebView?

You can set the user-agent by using the Java [setUserAgentString 
 API](http://developer.android.com/reference/android/webkit/WebSettings.html#setUserAgentString(java.lang.String)) 
method. This method only changes the user-agent string for requests sent by the WebView itself.

You can't set the user-agent string used for `XMLHttpRequest`s made from JavaScript. Those 
requests always use the default user-agent string.

### Does this mean Chrome for Android is using the WebView?

No, Chrome for Android is separate from WebView. They're both based on the same
code, including a common JavaScript engine and rendering engine.

### Does the new WebView have feature parity with Chrome for Android?

For the most part, features that work in Chrome for Android should work in 
the new WebView.

Chrome for Android supports a few features which aren't enabled in the 
WebView, including: 

* WebGL 3D canvas
* WebRTC
* WebAudio
* Fullscreen API
* Form validation

### Will the WebView have a six-week release cycle like Chrome?

WebView will continue to be tied to releases of the Android platform for the 
time being. However, sharing code with Chrome for Android will make it easier to keep 
the WebView up to date.

### What does the new WebView mean for developers?

This is a big change from the original WebView as it brings a new set of HTML5 
feature support, improved JavaScript performance, and remote debugging of web 
content using the Chrome DevTools. 

There **are** some changes that will affect existing apps. Please read the 
[migration guide](http://developer.android.com/guide/webapps/migrating.html) to understand 
the small number of changes that might impact you.

### How do I enable remote debugging?

See the [remote debugging guide](/chrome-developer-tools/docs/remote-debugging).


### Does the WebView support the Chrome Apps APIs?

No. The Chrome Apps platform isn't yet supported on Android.

### Should I enable hardware acceleration?

Hardware acceleration is enabled by default. If you are explicitly disabling 
it for older versions of Android you should try enabling it for KitKat based 
devices and see if it improves performance. 
