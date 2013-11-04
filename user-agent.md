Project: /chrome/mobile/_project.yaml
Book: /chrome/mobile/_book.yaml

# Chrome for Android User-Agent

Chrome for Android reports its user agent string (UA) in the following
formats, depending on whether the device is a phone or a tablet:

Phone UA:

    Mozilla/5.0 (Linux; <Android Version>; <Build Tag etc.>)
    AppleWebKit/<WebKit Rev> (KHTML, like Gecko) Chrome/<Chrome Rev> Mobile
    Safari/<WebKit Rev>

Tablet UA:

    Mozilla/5.0 (Linux; <Android Version>; <Build Tag etc.>)
    AppleWebKit/<WebKit Rev>(KHTML, like Gecko) Chrome/<Chrome Rev>
    Safari/<WebKit Rev>

Here's an example of the Chrome user agent string on a Galaxy Nexus:

    Mozilla/5.0 (Linux; Android 4.0.4; Galaxy Nexus Build/IMM76B)
    AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.133 Mobile
    Safari/535.19

For comparison, here are examples of other popular user agent strings:

Desktop Chrome: `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_3)
AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.151
Safari/535.19`

Desktop Safari: `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_3)
AppleWebKit/534.53.11 (KHTML, like Gecko) Version/5.1.3
Safari/534.53.10`

iPhone Safari: `Mozilla/5.0 (iPhone; U; CPU like Mac OS X; en)
AppleWebKit/420+ (KHTML, like Gecko) Version/3.0 Mobile/1A543
Safari/419.3`

iPad Safari: `Mozilla/5.0 (iPad; U; CPU OS 3_2 like Mac OS X; en-us)
AppleWebKit/531.21.10 (KHTML, like Gecko) Version/4.0.4 Mobile/7B334b
Safari/531.21.10`

## User agent mechanics

Like all other browsers, Chrome for Android sends this information
in the `User-Agent` HTTP header every time it makes a request to any
site. It’s also available in the client through JavaScript using the
`navigator.userAgent` call.

If you are parsing user agent strings using regular expressions, the
following can be used to check against Chrome for Android phones and
Android tablets:

- Phone pattern: `'Android' + 'Chrome/[.0-9]* Mobile'`
- Tablet pattern: `'Android' + 'Chrome/[.0-9]* (?!Mobile)'`

# Chrome for iOS User-Agent

The User-Agent string in Chrome for iOS is the same as the Mobile Safari
User-Agent, with `CriOS/<ChromeRevision>` instead of
`Version/<VersionNum>`.

Here's an example of a Chrome for iPhone User-Agent string:

`Mozilla/5.0 (iPhone; U; CPU iPhone OS 5_1_1 like Mac OS X; en)
AppleWebKit/534.46.0 (KHTML, like Gecko) CriOS/19.0.1084.60 Mobile/9B206
Safari/7534.48.3`

For more information about Mobile Safari user agent strings, see [this
article][mobile-safari-ua].

[mobile-safari-ua]: http://developer.apple.com/library/safari/#documentation/appleapplications/reference/safariwebcontent/OptimizingforSafarioniPhone/OptimizingforSafarioniPhone.html

