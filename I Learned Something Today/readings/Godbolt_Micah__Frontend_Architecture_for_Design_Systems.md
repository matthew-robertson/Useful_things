Impression:
On the whole, not very impressed with this one. A lot of stuff that seems dated, obvious, etc... It's not especially focused, and less targeted at what I was hoping to read about than I was hoping. It seems very focused on Red Hat's implementation, with a more static app vs a webapp. Some neat stuff about testing (performance and visual), ideas for creating systems (even if the example systems aren't particularly convincing), etc...

Mechanically, the writing itself is pretty lackluster, and could've used some real tightening up. I'm not convinced this got edited seriously.

Quotes/Annotations:
1. '
These are the rules we eventually came up with [for designing the ruleset for our design system]:
    1. Only include immutable rules, not general instructions
    1. Always boil each rule down to its simplest expression
    1. Always state the rule first, then explain "If not, then what?"
    1. Every rule should include one of the following: *always*, *never*, *only*, *every*, *don't*, or *do*.
'

1. 'For our system and set of rules, the single responsibility principle means that every class I create is made to be used for a singular purpose, in a single place. Therefore, if I make a change to `.rh-standard-band-title`, I can be confident that the only effect this will have on our site is to change the appearance of the title inside of the `rh-standard-band`. This also means that if we decide to deprecate `rh-standard-band`, we can completely remove all of the associated CSS without fear of breaking some other component that "hijacked inheritance" and became reliant on that CSS.'
1. 'As we move away from building pages and into building design systems, we need to make sure that the way we slice up our work reflects our new approach. What this means is that we need to stop writing stories such as "update the 'about us' page." These requests typically involve dozens of small typographical and layout changes, and might include a request like "double the padding on the CTA button."'
1. 'It should be pretty apparent that even something as simple as creating a new button variation could be accomplished through a number of different approaches. And even though all four of these approaches accomplish the same goal, they each have significantly different effects on the design system that we're trying to create and maintain. This is exactly why we spent so much time during the code pillar stage defining the way that we write our selectors, write our CSS, and create element variations. With documentation available to describe our coding practices, the developer will have a guide to creating the correct code and push that new code up to their feature branch.'
1. "Our strict adherence to a schema system has paid off in so many areas that I am confident I will never create another design system without them again. This is a schema-driven design system." - I don't understand this chapter. How does the schema get enforced? Does it validate/generate the template somehow? Does it just act as documentation? How does everything stay in sync? I guess some tool is needed to generate a template from the json schema, but I'm unclear on if there are out of the box tools to do that.
1. '[Start creating a performance budget] by looking at a few of your key competitors' homepages and other key landing pages, and then compare load times, page weight, and other key metrics with your own website. The goal here isn't to just match their metrics. You want to make sure you are beating them by 20% or more... This 20% advantage over your competitor is the difference required for a user to recognize the difference between the two tasks. This is not something you do just once, but something that needs to be monitored regularly.'
1. '[HTTPArchive](http://httparchive.org/) is a great service that measures and records the average value of various website metrics across almost half a million websites. As of April 2014, here are a few values of note: Page weight: 2,061 KB; Total requests: 99; Cacheable resources: 46%'
1.a 'CSS frameworks are often a kitchen sink. They include every little imaginable style you could ever possibly need. While this might be great for prototyping, starting your website with hundreds of kilobytes of CSS and JS is putting yourself in quite a hole before you even write a line of code.' - Oof.
1. 'Lazy-load assets not required for initial page load. This could be JavaScript that isn't needed until the user interacts with the page, or images that are far below the initial load window.'
1. Timing metrics:
    1. Time to first byte
    1. Time to start render
    1. Time to document complete
1. "Any code that you can write without defect can be broken by just a few errant lines from another developer. Someone else, working on some other form component, didn't realize that the classes they were styling were shared with your contact form." - This seems like an excellent argument for utility-based classes.
1. [Quixote](https://github.com/jamesshore/quixote) is an example of a class of CSS diffing tools that looks for unit differences rather than visual ones.
1. 'The magic of [visual regression testing] is that at any given time, every single component in your entire system has a "gold standard" image that the current state can be compared to.' - How do you avoid people just automatically re-generating the images, or catch regressions when they've knowingly changed part of the component?
1. "Documentation is the blueprint of our design system. Without it, we will inevitably create solutions for problems that have already been solved and spend large amounts of time sifting through code to find the simplest answer."
1. [SassDoc](http://sassdoc.com/) is a Node-based *system documentation* tool that claims to be "to Sass what JSDoc is to JavaScript".

