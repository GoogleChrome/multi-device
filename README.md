The Chrome Multi-Device documentation
============

This is the source of the official [Chrome Multi-Device documentation](https://developer.chrome.com/multidevice/index) on developer.chrome.com (aka "DCC").

### Running the site

1. In the root of the project, start a server on port 8000
  * See this [script for Python server](https://github.com/paulirish/dotfiles/blob/3fa2e7dc1f1ea5eaf7f6a2531b937ff8bd8833f9/.functions#L25-L32).
  * It's easier if your server can also do a directory listing.
2. Open [http://localhost:8000/_preview.html](http://localhost:8000/_preview.html)
3. You will see the boilerplate with the index.html file already included
4. To preview another document, add a url paramater with the filename
  * Something like: [http://localhost:8000/_preview.html?data-compression.html](http://localhost:8000/_preview.html?data-compression.html)
  * Or: [http://localhost:8888/_preview.html?webview/gettingstarted.html](http://localhost:8888/_preview.html?webview/gettingstarted.html)
  * Things mostly work but is not exactly the same as viewing through DCC.

### Deployment

Once pushed to master, updates will go live to the DCC site within a few minutes or so.

### Troubleshooting

* If you can't find the content with the devtools-docs repo, it might be part of the Chromium repo
  * CSS, JavaScript, and navigation bugs related to developer.chrome.com can be logged to the [Chromium issue tracker](http://crbug.com) 
  
## License

Except as otherwise noted, the content of the DevTools documentation is licensed under the [Creative Commons Attribution 3.0 License](http://creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](http://www.apache.org/licenses/LICENSE-2.0).
