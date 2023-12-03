# HTML

This note uses [WHATWG HTML Living Standard](https://html.spec.whatwg.org/) and [MDN Web Docs](https://developer.mozilla.org/en-US/) as reference.

## Elements

### `script` element

The `script` element lets you include dynamic interactivity and data to a document with JavaScript.

#### `type` attribute

Indicates the type of script. By default, when the `type` attribute is omitted, scripts are "classic scripts", containing JavaScript code. The attribute can contain two different case-insensitive values preset values:

- `importmap`: the `script` element is an import map. An import map script includes a JSON object which can be used to control how JavaScript modules are imported. The JSON object includes an `imports` key which includes remappings of module specifiers which are replaced by the value of the specifier when the specifier is used during an import.
- `module`: the script's body content or the file referenced in the `src` attribute is treated as a module. Read more from [https://javascript.info/modules-intro](https://javascript.info/modules-intro).

#### `defer` and `async` attribute

`defer` and `async` are boolean attributes that indicate how a script is fetched and evaluated. Because these attributes affect on how and when a script is fetched, the attributes can't be used on a script without a `src` attribute.

Script fetching and evaluation differs a bit between classic scripts and module scripts.

By default, when a classic script element with no `defer` and `async` attributes is met by the HTML parser, parsing stops and the script is fetched and then evaluated. HTML parsing then continues after that. Due to render blocking, scripts placed on the `body` element are usually placed at the end of `body` to ensure that the script is the last thing to resolve in the parsing phase. Otherwise, the page rendering would be blocked at the load and the page can flash (FOUC), be white or look broken if the script fetching and evaluation takes long. `defer` attribute the script can be fetched in parallel with the HTML parsing and the script is evaluated after the HTML parsing is done. `async` attribute will do the same with fetching as `defer` but on top of parallel fetching, the script will also be evaluated as soon as possible, which blocks the HTML parsing.

With module scripts, `defer` has no effect because module scripts are deferred by default. `async` does the same as with classic scripts, it the script will be evaluated as soon as possible after fetching.

Here's a schematic diagram which summarizes the effect of `defer` and `async` to fetching, evaluation and HTML parsing:

![Schematic diagram of defer and async](./images/asyncdefer.svg)
