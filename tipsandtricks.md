Project: /chrome/mobile/_project.yaml
Book: /chrome/mobile/_book.yaml

<style>
button {
      display: inline-block;
      background-color: #f39c12;
      padding: 14px;
      
      border-radius: 5px;
      border-bottom-style: solid;
      border-width: 4px;
      border-color: #DA8300;
      font: 24px cursive;
      width: 300px;

      color: white;
      outline: 0;
      -webkit-tap-highlight-color: rgba(0,0,0, 0.0);
}

button:focus {
    background-color: #E68F05;
    border-color: #DA8300;
}

button:active {
    background-color: #FFA91F;
    border-color: #E68F05;
}
</style>

# WebView Tips & Tricks

This article outlines some useful tips to enhance your application's
user experience.

[[TOC]]

## Flicker of colors when the application loads

Ever noticed a flash of black and white when an application loads up with a 
WebView?

This tends to be caused by the loading of the window background color of app 
(usually set in the theme), followed by the white flash of the WebView 
background before it loads any content and then the final color defined by the 
pages CSS.

The fix for this white flash is simple.

### Set the background of the layout

All you need to do is set the background color of the WebView, which 
removes the white flash. The only delay in displaying this color will be in 
the native app to draw the WebView:

    mWebView.setBackgroundColor(Color.parseColor("#3498db"));

In Android it's generally good practice to define color values 
in a <code>res/values/colors.xml</code> file, as described in 
<a href="http://developer.android.com/guide/topics/resources/more-resources.html#Color">
the Android App Resource guide</a>. Using a color defined in the application's resources,
the above line would become:</p>
<pre>
mWebView.setBackgroundColor(getResources().getColor(R.color.my_color_name));
</pre>

## Touch Feedback

One of the common differences between native and web applications is the lack 
of touch feedback in many web apps.

The solution is simple, you need to support the :active pseudo class.

If you have a simple button with the following styling:

    .btn {
      display: inline-block;
      background-color: #f39c12;
      padding: 14px;
      
      border-radius: 5px;
      border-bottom-style: solid;
      border-width: 4px;
      border-color: #DA8300;
    }

<img src="/chrome/mobile/images/webview/1-button.png" width="320" alt=""/> 

The pressed state may look like:

    .btn:active {
      margin-top: 2px;
      background-color: #E68F05;
      border-color: #CD7600;
      border-width: 2px;
    }

<img src="/chrome/mobile/images/webview/2-button-touched.png" width="320" alt=""/> 

All this does is darken the background color and border color slightly, decrease the 
border size, so it looks like the button is sinking into the UI. The added margin 
balances the smaller border.

### Lighten / Darken

If you use Sass you can use the darken / lighten mixins to alter the colors of 
elements without needing to know the exact hex value. Or you can make use of 
online tools such as [http://hexcolortool.com/](http://hexcolortool.com/).

### System highlights

Many user agents will add some form of touch feedback to elements, preventing 
the need for the page to define anything specific. In the WebView you may notice 
an orange color on elements or an orange ring around links and elements.

For example a button element in the new WebView is given an orange outline:

<img src="/chrome/mobile/images/webview/3-outline.png" width="300" alt="button with outline highlight"/> 

In the old webview a highlight color is applied on top of certain 
elements:

<img src="/chrome/mobile/images/webview/4-tap-highlighting.png" width="469" alt=""/> 

If you are taking care of the touch and focus feedback yourself, you can 
override the defaults with the following CSS properties:

    -webkit-tap-highlight-color: rgba(0,0,0, 0.0);
    outline: none;

And set your own colors like so:

    button {
      …
      outline: 0;
      -webkit-tap-highlight-color: rgba(0,0,0, 0.0);
    }

    button:focus {
      background-color: #E68F05;
      border-color: #DA8300;
    }

    button:active {
      background-color: #FFA91F;
      border-color: #E68F05;
    }

This causes the button to have different colors depending on the state, so there 
is the default state, a focused color and then a pressed (or active) state.

<button>Press Me</button>
<button>No, Press Me</button>

The main areas to set these properties are form input fields and 
anchor tags.
