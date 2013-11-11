Project: /chrome/mobile/_project.yaml
Book: /chrome/mobile/_book.yaml

<style>
figure { text-align: center }
</style>

# Pixel-Perfect UI in the WebView

There are a number of options you can use to create the perfect UI, this article 
will outline some of the best practices for the mobile web in general and then 
some specific tricks you could use for hybrid applications.

[[TOC]]

## Viewport

The viewport meta tag is of the most important tags you need to add to your web 
app. Without it, the WebView may act as if your site is designed for desktop 
browsers. This causes your web page to be given a larger width (typically 980px) 
and scales it to fit the WebView's width. In most cases, the result is a tiny 
overview version of the page that requires the user to pan and zoom to actually 
read content, like the image on the left.

<figure>
<img src="/chrome/mobile/images/webview/viewports.jpg" width="640px" alt=""/> 
</figure>


If you want the width of your site to be 100% of the WebView's width, as shown 
on the right, you need to set the viewport meta tag:

    <meta name="viewport" content="width=device-width, initial-scale=1">

Setting width to the special value device-width will give you more control over 
the page layout. 

By default the WebView will set the viewport to device-width, rather than 
defaulting to a desktop viewport. However, for reliable and controlled behaviour 
it's good practice to include the viewport meta tag.

### Desktop Sites

In some cases, you may need to display content that isn't designed for mobile 
devices -- for example, if you're displaying content you don't control. In this 
case, you need to force the WebView to use a desktop-size viewport:

* [setUseWideViewPort(true)](http://developer.android.com/reference/android/webkit/WebSettings.html#setUseWideViewPort(boolean))
* [setLoadWithOverviewMode(true)](http://developer.android.com/reference/android/webkit/WebSettings.html#setLoadWithOverviewMode(boolean))

Without these methods being set and no viewport the WebView will try and set the 
viewport width based on the content size. 

In addition to doing this, you may want to use the new layout algorithm `TEXT_AUTOSIZING` 
introduced in Android 4.4, which increases the font size to make it more readable on a mobile 
device. See [setLayoutAlgorithm](http://developer.android.com/reference/android/webkit/WebSettings.html#setLayoutAlgorithm(android.webkit.WebSettings.LayoutAlgorithm)). 

<figure>
<img src="/chrome/mobile/images/webview/text-autosize.jpg" width="658" alt=""/> 
</figure>

## Responsive Design

Responsive design is the notion of changing your UI depending on the dimensions 
of the screen size. Here we will look at some simple examples of how you can 
adapt your UI and images, but if you want to dig in to other topics then this 
article on [HTML5Rocks](http://www.html5rocks.com/en/mobile/responsivedesign/) 
is a good point of reference.

One of the main CSS features you'll want to use is media queries.

A media query is a way of applying CSS to elements based on a device's 
characteristics. For example, suppose you  wanted to go from a vertical layout 
to a horizontal layout based on orientation.

First, set CSS properties to default to portrait:

    .page-container {
        display: -webkit-box;
        display: flex;
    
        -webkit-box-orient: vertical;
        flex-direction: column;
    
        padding: 20px;
        box-sizing: border-box;
    }

Then to switch to a horizontal layout you just need to switch the flex-direction 
property based on the orientation:

    @media screen and (orientation: landscape) {
      .page-container.notification-opened {
        -webkit-box-orient: horizontal;
        flex-direction: row;
      }
    
      .page-container.notification-opened > .notification-arrow {
        margin-right: 20px;
      }
    }

<figure>
<img src="/chrome/mobile/images/webview/orientation-changes.jpg" width="826px" alt=""/> 
</figure>

This technique can be used to change layout based on the width of the screen.

For example, adjusting the size of the button from being 100% to something 
smaller as the physical screen size gets larger.

    button {
      display: block;
      width: 100%;
      ...
    }

    @media screen and (min-width: 500px) {
      button {
        width: 60%;
      } 
    }

    @media screen and (min-width: 750px) {
      button {
        width: 40%;
        max-width: 400px;
      } 
    }

<figure>
<img src="/chrome/mobile/images/webview/button-width.jpg" width="580" alt=""/> 
</figure>

These are minor changes, but depending on your UI, media queries can help you to 
make much larger changes to appearance of your application, while keeping the 
same HTML.

<aside class="note"><b>Tip:</b> Try to change the UI based on width as a component 
starts to look out of place, rather than focusing on device sizes. By doing this, 
your app will look great on a range of devices, not just the ones you test on. An 
easy way to do this is play around with the size of a browser window. But remember that 
actual devices have different screen densities than your monitor, so always test on a 
range of devices.</aside>

There will be best practices for organising CSS and designing for mobiles on 
[HTML5Rocks](http://www.html5rocks.com/en/) soon.

## Images

The variety of screens sizes and densities also presents challenges for images.
Smaller images require less memory and are faster to load, but blur if you scale them up.

The images below show the blurring that occurs when you scale a low-density image up for a 
high-denity screen, compared with the crisp display of an appropriately-sized image.

<figure>
<img src="/chrome/mobile/images/webview/ship1xand2x.png" width="820px" alt=""/>
</figure>

Here are a few tips and tricks to make sure your images look crisp and clear on any screen:

* Use CSS for scalable effects. 
* Use vector graphics.
* Provide high-resolution photos.

### Use CSS for scalable effects

Make use of CSS3 where you can for borders, drop shadows, border-radius, 
and so on instead of images. These features can scale easily. However, 
some combinations of CSS properties can be expensive to render, so you 
should always test the specific combinations you're using. (For some 
sample data on fast and slow CSS properties, see: [CSS Paint Times and 
Page Render Weight](http://www.html5rocks.com/en/tutorials/speed/css-paint-times/) 
on HTML5Rocks.)

### Use vector graphics

Scalable Vector Graphics (SVGs)  are a great way of providing scalable 
version of images. For images that are well-suited to vector graphics, SVG 
provides high quality images with very small file sizes. For more 
information, see [Splash Vector Graphics on your Responsive 
Site](http://www.html5rocks.com/en/tutorials/svg/mobile_fundamentals/) on 
HTML5Rocks.

### Provide high-resolution photos

Use a version suitable for a high-DPI device and scale the image using CSS.
This way the image has a high quality across devices. If you use
high compression (low quality setting) when generating the image, you 
may be able to acheive good visual results with a reasonable file size.

This approach is simple, but has a couple of 
potential downsides: highly-compressed images may show some visual 
artifacts, so you need to experiment to determine what level of 
compression you find acceptable. And resizing the image in CSS can be an 
expensive operation.

If high compression is not suitable for your needs, try the WebP format, 
which gives a high quality image with relatively small file size. See [the 
official WebP site](/speed/webp/) for details. Don't forget that you'll 
need to provide a fallback for older versions of Android where WebP isn't supported.

For more information on this topic:

* [High DPI Images for Variable 
Pixel Densities](http://www.html5rocks.com/en/mobile/high-dpi/) on HTML5Rocks.
* [Seeing the World Through High DPI](https://developers.google.com/events/io/sessions/350992350) video from Google I/O 2013.

### Fine grained control

In many cases, you can't use a single image for all devices. In this case, you can
select different images based on the screen size and density. You can use media queries to select 
background images by screen size and density. You can also use JavaScript to control how images load.

#### Media queries and screen density

To select an image based on the screen density, you need to use `dpi` or 
`dppx` units in your media query. The `dpi` unit represents dots per inch, while `dppx` 
represents dots per pixel.

Looking at the table below you can see the relation between `dpi` and `dppx`. 1`dppx` 
is equivalent to 96`dpi`, 2`dppx` == 192`dpi` == 2 x 96`dpi`, and so on.

<table>
<tr>
<td>Device pixel ratio</td>
<td>Generalized screen density</td>
<td>Dots per inch (<code>dpi</code>)</td>
<td>Dots per pixel (<code>dppx<code>)</td>
</tr>
<tr>
<td>1x</td>
<td>MDPI</td>
<td>96<code>dpi</code></td>
<td>1<code>dppx</code></td>
</tr>
<tr>
<td>1.5x</td>
<td>HDPI</td>
<td>144<code>dpi</code></td>
<td>1.5<code>dppx</code></td>
</tr>
<tr>
<td>2</td>
<td>XHDPI</td>
<td>192<code>dpi</code></td>
<td>2<code>dppx</code></td>
</tr>
</table>

The generalized screen density buckets are defined by the Android native platform 
and are used in other places to express screen density (for example, 
[http://screensiz.es](http://screensiz.es/phone)).

#### Background Images

You can use media queries to assign background images to elements. For example, 
if you have a logo image with 256px x 256px size on a device with a pixel ratio 
of 1.0, you might use the following CSS rules:

    .welcome-header > h1 {
      flex: 1;

      width: 100%;
      
      max-height: 256px;
      max-width: 256px;

      background-image: url('../images/html5_256x256.png');
      background-repeat: no-repeat;
      background-position: center;
      background-size: contain;
    }

To swap this out for a larger image on devices with device pixel ratio of 1.5 (hdpi) and 2.0 
(xhdpi), you can add the following rules:

    @media screen and (min-resolution: 1.5dppx) {
      .welcome-header > h1{
          background-image: url('../images/html5_384x384.png');
      }
    }

    @media screen and (min-resolution: 2dppx) {
      .welcome-header > h1{
          background-image: url('../images/html5_512x512.png');
      }
    }

<figure>
<img src="/chrome/mobile/images/webview/device-pixel-ratio.png" width="808px" alt=""/> 
</figure>

You can then merge this technique with other media queries like `min-width`, which 
is useful as you start to account for different form factors.

    @media screen and (min-resolution: 2dppx) {
      .welcome-header > h1{
              background-image: url('../images/html5_512x512.png');
      }
    }

    @media screen and (min-resolution: 2dppx) and (min-width: 1000px) {
      .welcome-header > h1{
              background-image: url('../images/html5_1024x1024.png');

              max-height: 512px;
          max-width: 512px;
      }
    }

You might notice that the `max-height` and `max-width` are set to 512px for 2`ddpx`
resolution, with an image of 1024x1024px. this is because a CSS "pixel" actually takes 
into account the device pixel ratio (512px * 2 = 1024px).

#### What About &lt;img />?

The web today doesn't have a solution for this. There are some proposals in the works, but 
they aren't available in current browsers or in the WebView.

In the mean time, if you generate your DOM in JavaScript, you can create 
multiple image resources in a sane directory structure:

<pre>
images/
    mdpi/
        imagename.png
    hdpi/
        imagename.png
    xhdpi/
        imagename.png
</pre>

Then use the pixel ratio to try and pull the most appropriate image:

    function getDensityDirectoryName() {
      if(!window.devicePixelRatio) {
        return 'mdpi';
      }

      if(window.devicePixelRatio > 1.5) {
        return 'xhdpi';
      } else if(window.devicePixelRatio > 1.0) {
        return 'hdpi';
      }

      return 'mdpi';
    }

The alternative to implementing the above in JS, is to alter the base URL of the 
page to define the relative URLs for images.

    <!doctype html>
    <html class="no-js">
    <head>
      <script>
        function getDensityDirectoryName() {
          if(!window.devicePixelRatio) {
              return 'mdpi';
          }

          if(window.devicePixelRatio > 1.5) {
              return 'xhdpi';
          } else if(window.devicePixelRatio > 1.0) {
              return 'hdpi';
          }

          return 'mdpi';
        }

        var baseUrl = 
            'file:///android_asset/www/img-js-diff/ratiores/'+getDensityDirectoryName()+'/';
        document.write('<base href="'+baseUrl+'">');
      </script>

        ...
    </head>

    <body>
        ...
    </body>
    </html>

The major downsides to this are that it blocks the page load and forces you to 
use absolute paths for scripts, CSS files, links and so on, since the base URL points to 
a density-specific directory.
