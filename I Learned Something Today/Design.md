# Physical Design
1. Differentiate your inputs. Do it with different shapes, sizes, colours, etxtures, modalities (switches vs buttons, vs dials, etc..), positions, whatever. This issue has caused a ton of plane crashes, car recalls, [etc...](https://interactionmagic.com/UX-LEGO-Interfaces). This is also why single screen UIs are horrible in vehicles. People rely on mucle memory when their full attention isn't on something, don't move things on them.
1. When organizing inputs, there are a [handful of approaches](https://interactionmagic.com/UX-LEGO-Interfaces) one might take. You might group by operation (like switches together), or by feature (all lighting controls in one place, all airflow ones in another). Consider [this picutre](https://interactionmagic.com/images/pages/UX-LEGO-Interfaces/organisation.jpg) of Lego UIs demonstrating examples.
1. The 1964 Tokyo Games gave us the first system for [sports+services pictograms](https://olympic-museum.de/pictograms/olympic-games-pictograms-1964.php). Yes, this is where the symbols for men's+women's bathrooms come from.

# Web Design
1. The web as a medium is flexible and adaptable. [Don't try to control the user's browser](https://alistapart.com/article/dao/). "Designing adaptable pages is designing accessible pages".
1. ["Let form follow function, don't take a design and make it work"](https://alistapart.com/article/dao/#section6)
1. Separate your appearance from your content, you [shouldn't be using HTML for presentation.](https://alistapart.com/article/dao/#section6) If there's an appropriate element for the purpose, use it.
1. "In practical terms, there are some things you should and some things you shouldn’t do when designing style sheets that will impact on the adaptability of your pages. Above all, don’t rely on any aspect of style sheets to work in order for a page to be accessible. Absolute units, like pixels and points are to be avoided (if that comes as a surprise, read on), and color needs to be used carefully, and never relied on."[Here](https://alistapart.com/article/dao/#section6)
1. Don't specify font pts, you can't guess at what the user uses as accessible. You can, however, suggest larger font sizes for headings and whatnot. CSS allows you to specify fontsize as a percent of its parent, and you should use that.
1. Percentages remain useful when specifying [layouts](https://alistapart.com/article/dao/#section8). Notably, margins can be specified using the `em` unit, which is relative to the fontsize.
1. People are colourblind, [don't rely on colour alone to convey meaning](https://alistapart.com/article/dao/#section9). Use stylesheets to specify colour, so users can override them with their user style sheet.

## CSS
1. `.a + .b` is the sibling selector. This will apply a style to an element with the class `b` below/next to an element with the class `a`.
