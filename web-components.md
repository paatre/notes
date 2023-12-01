# Web components

## Jim Nielsen's Blog

I've gathered notes under this heading from [Jim Nielsen's Blog](https://blog.jim-nielsen.com/). He seems to have great articles talking about web components so I gather some of the things he's written. The information is organized per blog article.

### HTML Web Components

Link: [https://blog.jim-nielsen.com/2023/html-web-components/](https://blog.jim-nielsen.com/2023/html-web-components/)  
Date: 2023-11-13

Jim has two main themes in the article: **composition** and **web's longevity via augmentation**.

#### Composition

Web components can render before JavaScript, which React cannot do at all. This encourages into composability design where you use normal HTML elements as the base and then augment the elements by wrapping them into a custom element that brings additional functionalities and enhancements. This way to creating components plays into the [progressive enhancement](https://en.wikipedia.org/wiki/Progressive_enhancement) strategy as well because using HTML elements as the core element of the web component, the page will show the interactable user content before JavaScript has loaded or if it doesn't load at all. This helps with accessibility, usability and user experience.

There are couple of example components in the article. First one is "a typical React component":

```javascript
<UserAvatar
    src="https://example.com/path/to/img.jpg"
    alt="..."
/>
```

Then there's a web component that has *not* been written in a composable way but rather in the same way as the React component above:

```html
<user-avatar
    src="https://example.com/path/to/img.jpg"
    alt="..."
></user-avatar>

```

Third example is the composable way of writing a web component:

```html
<user-avatar>
    <img src="https://example.com/path/to/img.jpg" alt="..." />
</user-avatar>
```

A good thought from Jim was that a web component should possibly be just a "shell" for HTML elements, extending and augmenting functionality instead of replacing the whole elements and/or depending on JavaScript.

Note: Dan Abramov, developer of React, Redux and Create React App, does mention composability as "top react skill to learn in 2023" in a tweet: [https://twitter.com/dan_abramov/status/1623771055943831553](https://twitter.com/dan_abramov/status/1623771055943831553)

#### Web's longevity via augmentation

The main take away from Jim regarding this matter is that we shouldn't try to replace what web already has provided us for years. We already have HTML and the web APIs. We don't need to replace it, we need to *augment* and *enhance* it. Jim also arguments that it's also a good bet for in the long game because there have been multiple times when previous attempts to replace the old standard and create something totally new have actually resulted in augmenting these ideas into the browser as well. Jim mentions the following technologies: HTML5 augmented XHTML5, networking libraries were brought into `fetch`, Sass and jQuery were ported into browser and Typescript as well.

### Upcoming articles and material

- As Good as HTML: [https://blog.jim-nielsen.com/2023/as-good-as-html/](https://blog.jim-nielsen.com/2023/as-good-as-html/)
- There’s TypeScript and Then There’s TypeScript: [https://blog.jim-nielsen.com/2023/the-flavors-of-typescript/](https://blog.jim-nielsen.com/2023/the-flavors-of-typescript/)
- Better Apps: Delivering Universal UI Patterns as Web Components: [https://www.youtube.com/watch?v=mtHf7crZZIQ](https://www.youtube.com/watch?v=mtHf7crZZIQ)
- Component.Kitchen: [https://component.kitchen/](https://component.kitchen/)
- Using Web Components on My Icon Galleries Websites: [https://blog.jim-nielsen.com/2023/web-components-icon-galleries/](https://blog.jim-nielsen.com/2023/web-components-icon-galleries/)
- Blinded By the Light DOM: [https://meyerweb.com/eric/thoughts/2023/11/01/blinded-by-the-light-dom/](https://meyerweb.com/eric/thoughts/2023/11/01/blinded-by-the-light-dom/)

