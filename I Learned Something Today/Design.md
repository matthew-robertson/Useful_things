# Physical Design
1. Differentiate your inputs. Do it with different shapes, sizes, colours, etxtures, modalities (switches vs buttons, vs dials, etc..), positions, whatever. This issue has caused a ton of plane crashes, car recalls, [etc...](https://interactionmagic.com/UX-LEGO-Interfaces). This is also why single screen UIs are horrible in vehicles. People rely on mucle memory when their full attention isn't on something, don't move things on them.
1. When organizing inputs, there are a [handful of approaches](https://interactionmagic.com/UX-LEGO-Interfaces) one might take. You might group by operation (like switches together), or by feature (all lighting controls in one place, all airflow ones in another). Consider [this picutre](https://interactionmagic.com/images/pages/UX-LEGO-Interfaces/organisation.jpg) of Lego UIs demonstrating examples.

# Web Design
1. The web as a medium is flexible and adaptable. [Don't try to control the user's browser](https://alistapart.com/article/dao/). "Designing adaptable pages is designing accessible pages".
1. ["Let form follow function, don't take a design and make it work"](https://alistapart.com/article/dao/#section6)
1. Separate your appearance from your content, you [shouldn't be using HTML for presentation.](https://alistapart.com/article/dao/#section6) If there's an appropriate element for the purpose, use it.
1. "In practical terms, there are some things you should and some things you shouldn’t do when designing style sheets that will impact on the adaptability of your pages. Above all, don’t rely on any aspect of style sheets to work in order for a page to be accessible. Absolute units, like pixels and points are to be avoided (if that comes as a surprise, read on), and color needs to be used carefully, and never relied on."[Here](https://alistapart.com/article/dao/#section6)
1. Don't specify font pts, you can't guess at what the user uses as accessible. You can, however, suggest larger font sizes for headings and whatnot. CSS allows you to specify fontsize as a percent of its parent, and you should use that.
1. Percentages remain useful when specifying [layouts](https://alistapart.com/article/dao/#section8). Notably, margins can be specified using the `em` unit, which is relative to the fontsize.
1. People are colourblind, [don't rely on colour alone to convey meaning](https://alistapart.com/article/dao/#section9). Use stylesheets to specify colour, so users can override them with their user style sheet.
1. When working with icons, you usually have tons of small images that must load immediately and update on application state. [SVGs](https://github.com/simplabs/ember-asset-size-action) are likely the way to go for a handful of reasons.
1. For styling static images that are less important, you likely want to 1) specify dimensions to avoid shifting, 2) lazy load, and 3) asynchronously decode it.
1. When dealing with dynamic stuff (think timelines, api repsonese that give you data), it can be hard to avoid reflowing. A nice way to do it is to have the API tell you how big the images are, etc.. possibly going so far as to hold off rendering until images have loaded (via Ember routing layer). See [here](https://youtu.be/4K6gWIXQDkE).

## CSS
1. `.a + .b` is the sibling selector. This will apply a style to an element with the class `b` below/next to an element with the class `a`.
1. Font Icons (like Font Awesome) [can be rough for accessibility](https://youtu.be/4K6gWIXQDkE). This is because screen readers will try to read them as text.

## HTML
1. WebP is a pretty neat image format supported by most major browsers (Safari's spotty). You can use `<picture>` elements to let the browser decide what source to use to support browers like that. Something like:
```
<picture>
  <source srcset="landing.webp" type="image/webp">
  <img src="/landing.jpg"\>
</picture>
```

# Links
1. [Handling Images on the Web - Marco Otte-Witte](https://youtu.be/4K6gWIXQDkE)
