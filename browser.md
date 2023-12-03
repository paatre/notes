# Browser

## Processes and threads

Before we had modern browsers, single-process browsers were used. Due to instability, low performance and insecurity issues, multiprocess browsers were created to solve these problems. Even though multiple processes from a browser can lead to a lot of used system resources, there are big upsides to multiprocess browsers:
- More stability: browser doesn't crash totally if one tab process stops
- More performant: JavaScript loading and execution does not block a process on other tab
- More secure: all renderer processes are ran in a sandbox and isolation from the browser process which includes more critical data such as passwords and cookies

## Navigation

1. Browser process handles user input and composing a URL which is sent to a network process for handling the URL request.
2. Network process checks the browser cache. Either it returns the cached resource to the browser process or starts the request process. The request process contains multiple steps: connection to the internet, a DNS lookup, receiving of the IP address of the target server, and a TLS connection. The target server receives and handles the request and then generates a response back to the browser which is received by the network process. Network process parses headers from the response at this point. During the header parsing, browsers may do security checks, redirections, actions based on the Content-Type header or continue with the "normal" HTML document rendering. In the last case, network process starts to download the HTML document and the browser process prepares a renderer process at the same time.
3. Browser process creates the render process and puts it into a standby mode, and network process downloads the document for the renderer process.
4. Browser process updates TLS, URL, history and session information, and clears the old document. Network process starts to stream the HTML document to the renderer process.
5. Renderer process parses the streaming document data and displays a web page.

## Rendering

1. DOM construction: DOM tree is created out of the HTML document.
2. Style computation: units need to be calculated and styles need to be computed for the elements from stylesheets.
3. Layout: layout tree is constructed from visibility and geometry information.
4. Layer: layer tree is constructed from the layout tree, the stacking context properties and clipped elements (overflow, scroll, etc.).
5. Paint records: paint records are created from layout and layer trees for browser to exexute the painting.
6. Tiling: tiles are created out of paint records and layers.
7. Raster: GPU process generates bitmpas from tiles.
8. Draw and display: renderer process creates a compositor frame which is displayed by a browser process.

## Optimization of loading stage

How to optimize the DOM construction phase which is the most critical point in the rendering phase? DOM construction can be block by both CSS styles and JavaScript files which leads to reduced loading performance. Inline `script` element blocks the HTML parser and the script will be executed, and only after that the parser can continue. If the `script` element has a sourced file to it, the blocked time can be even higher due to needing to load the JavaScript from its source. If a script modifies styles, the renderer process needs to have *computed style* to modify the styles which means the HTML parser is also block until the CSS file is parsed and the script is executed. Same thing can happen with sourced `style` elements. 

*Continuing later from [https://cabulous.medium.com/how-does-browser-work-in-2019-part-4-more-about-rendering-phase-fbba0d94a174](https://cabulous.medium.com/how-does-browser-work-in-2019-part-4-more-about-rendering-phase-fbba0d94a174)*
