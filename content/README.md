
In this article, we look at 20 ways to optimise CSS so that it’s faster-loading, easier to work with and more efficient.

Here, we’ll concentrate on CSS. Admittedly, CSS is rarely the worst culprit and a typical site uses 40KB spread over five stylesheets. That said, there are still optimisations you can make, and ways to change how we use CSS that will boost site performance.

## 1. Learn to Use Analysis Tools

You can’t address performance problems unless you know where the faults lie. Browser DevTools are the best place to start: launch from the menu or hit F12, Ctrl + Shift + I or Cmd + Alt + I for Safari on macOS.

All browsers offer similar facilities, and the tools will open slowly on badly-performing pages! However, the most useful tabs include the following …

The Network tab displays a waterfall graph of assets as they download. For best results, disable the cache and consider throttling to a lower network speed. Look for files that are slow to download or block others. The browser normally blocks browser rendering while CSS and JavaScript files download and parse.

The Performance tab analyses browser processes. Start recording, run an activity such as a page reload, then stop recording to view the results. Look for:

Excessive layout/reflow actions where the browser has been forced to recalculate the position and size of page elements.
Expensive paint actions where pixels are changed.
Compositing actions where the painted parts of the page are put together for displaying on-screen. This is normally the least processor-intensive action.
Chrome-based browsers provide an Audits tab which runs Google’s Lighthouse tool. It’s often used by Progressive Web App developers, but also makes CSS performance suggestions.

Online Options
Alternatively, use online analysis tools that are not influenced by the speed and capabilities of your device and network. Most can test from alternative locations around the world:

    Pingdom Website Speed Test
    GTmetrix
    Google PageSpeed Insights
    WebPageTest

## 2. Make Big Wins First
CSS is unlikely to be the direct cause of performance issues. However, it may load heavy-hitting assets which can be optimized within minutes. Examples:

Activate HTTP/2 and GZIP compression on your server
Use a content delivery network (CDN) to increase the number of simultaneous HTTP connections and replicate files to other locations around the world
Remove unused files.
Images are normally the biggest cause of page bulk, yet many sites fail to optimize effectively:

Resize bitmap images. An entry-level smartphone will take multi-megapixel images that can’t be displayed in full on the largest HD screen. Few sites will require images of more than 1,600 pixels in width.

Ensure you use an appropriate file format. Typically, JPG is best for photographs, SVG for vector images, and PNG for everything else. You can experiment to find the optimum type.

Use image tools to reduce file sizes by striping metadata and increasing 
compression factors.

That said, be aware that xKb of image data is not equivalent to xKb of CSS code. Binary images download in parallel and require little processing to place on a page. CSS blocks rendering and must be parsed into an object model before the browser can continue.

## 3. Replace Images with CSS Effects
It’s rarely necessary to use background images for borders, shadows, rounded edges, gradients and some geometric shapes. Defining an “image” using CSS code uses considerably less bandwidth and is easier to modify or animate later.

## 4. Remove Unnecessary Fonts
Services such as Google Fonts make it easy to add custom fonts to any page. Unfortunately, a line or two of code can retrieve hundreds of kilobytes of font data. Recommendations:

Only use the fonts you need.
Only load the weights and styles you require — for example, roman, 400 weight, no italics.
Where possible, limit the character sets. Google fonts allows you to pick certain characters by adding a &text= value to the font URL — such as fonts.googleapis.com/css?family=Open+Sans&text=SitePon for displaying “SitePoint” in Open Sans.
Consider variable fonts, which define multiple weights and styles by interpolation so files are smaller. Support is currently limited to Chrome, Edge, and some editions of Safari but should grow rapidly. See How to Use Variable Fonts.
Consider OS fonts. Your 500Kb web font may be on-brand, but would anyone notice if you switched to the commonly available Helvetica or Arial? Many sites use custom web fonts, so standard OS fonts are considerably less common than they were!

## 5. Avoid @import
The @import at-rule allows any CSS file to be included within another. For example:

	/* main.css */
	@import url("base.css");
	@import url("layout.css");
	@import url("carousel.css");
This appears a reasonable way to load smaller components and fonts. It’s not. @import rules can be nested so the browser must load and parse each file in series.

Multiple <link> tags within the HTML will load CSS files in parallel, which is considerably more efficient — especially when using HTTP/2:

	<link rel="stylesheet" href="base.css">
	<link rel="stylesheet" href="layout.css">
	<link rel="stylesheet" href="carousel.css">
That said, there may be more preferable options …

## 6. Concatenate and Minify
Most build tools allow you to combine all partials into one large CSS file that has unnecessary whitespace, comments and characters removed.

Concatenation is less necessary with HTTP/2, which pipelines and multiplexes requests. In some cases, separate files may be beneficial if you have smaller, regularly-changing CSS assets. However, most sites are likely to benefit from sending a single file that is immediately cached by the browser.

Minification may not bring considerable benefits when you have GZIP enabled. That said, there’s no real downside.

Finally, you could consider a build process that orders properties consistently within declarations. GZIP can maximize compression when commonly used strings are used throughout a file.

## 7. Use Modern Layout Techniques
For many years it was necessary to use CSS float to lay out pages. The technique is a hack. It requires lots of code and margin/padding tweaking to ensure layouts work. Even then, floats will break at smaller screen sizes unless media queries are added.

The modern alternatives:

> CSS Flexbox for one-dimensional layouts which (can) wrap to the next
> row according to the widths of each block. Flexbox is ideal for menus,
> image galleries, cards, etc.
> 
> CSS Grid for two-dimensional layouts with explicit rows and columns.
> Grid is ideal for page layouts.

Both options are simpler to develop, use less code, can adapt to any screen size, and render faster than floats because the browser can natively determine the optimum layout.

## 8. Reduce CSS Code
The most reliable and fastest code is the code you need never write! The smaller your stylesheet, the quicker it will download and parse.

All developers start with good intentions, but CSS can bloat over time as the feature count increases. It’s easier to retain old, unnecessary code rather than remove it and risk breaking something. A few recommendations to consider:

Be wary of large CSS frameworks. You’re unlikely to use a large percentage of the styles, so only add modules as you need them.
Organize CSS into smaller files (partials) with clear responsibilities. It’s easier to remove a carousel widget if the CSS is clearly defined in widgets/_carousel.css.
Consider naming methodologies such as BEM to aid the development of discrete components.
`Avoid deeply nested Sass/preprocessor declarations. The expanded code can become unexpectedly large.`
	`Avoid using !important to override the cascade.`
	`Avoid inline styles in HTML.`
Tools such as UnCSS can help remove redundant code by analyzing your HTML, but be wary about CSS states caused by JavaScript interaction.


## 9. Cling to the Cascade!
The rise of CSS-in-JS has allowed developers to avoid the CSS global namespace. Typically, randomly generated class names are created at build time so it becomes impossible for components to conflict.

If your life has been improved by CSS-in-JS, then carry on using it. However, it’s worth understanding the benefits of the CSS cascade rather than working against it on every project. For example, you can set default fonts, colors, sizes, tables and form fields that are universally applied to every element in a single place. There is rarely a need to declare every style in every component.

## 10. Simplify Selectors
Even the most complex CSS selectors take milliseconds to parse, but reducing complexity will reduce file sizes and aid browser parsing. Do you really need this sort of selector?!

	body > main.main > section.first h2:nth-of-type(odd) + p::first-line > a[href$=".pdf"]
Again, be wary of deep nesting in preprocessors such as Sass, where complex selectors can be inadvertently created.

## 11. Be Wary of Expensive Properties
Some properties are slower to render than others. For added jankiness, try placing box shadows on all your elements!

	*, ::before, ::after {
	  box-shadow: 5px 5px 5px rgba(0,0,0,0.5);
	}
Browser performance will vary but, in general, anything which causes a recalculation before painting will be more costly in terms of performance:

	border-radius
	box-shadow
	opacity
	transform
	filter
	position: fixed

## 12. Adopt CSS Animations
Native CSS transitions and animations will always be faster than JavaScript-powered effects that modify the same properties. CSS animations will not work in older browsers such as IE9 and below, but those users will never know what they’re missing.

That said, avoid animation for the sake of it. Subtle effects can enhance the user experience without adversely affecting performance. Excessive animations could slow the browser and cause motion sickness for some users.

## 13. Avoid Animating Expensive Properties
Animating the dimensions or position of an element can cause the whole page to re-layout on every frame. Performance can be improved if the animation only affects the compositing stage. The most efficient animations use:

opacity and/or
transform to translate (move), scale or rotate an element (the original space the element used is not altered).
Browsers often use the hardware-accelerated GPU to render these effects. If neither are ideal, consider taking the element out of the page flow with position: absolute so it can be animated in its own layer.

## 14. Indicate Which Elements Will Animate
The will-change property allows CSS authors to indicate an element will be animated so the browser can make performance optimizations in advance. For example, to declare that an element will have a transform applied:

	.myelement {
	  will-change: transform;
	}
Any number of comma-separated properties can be defined. However:

use will-change as a last resort to fix performance issues
don’t apply it to too many elements
give it sufficient time to work: that is, don’t begin animations immediately.

## 15. Adopt SVG Images
Scalable vector graphics (SVGs) are typically used for logos, charts, icons, and simpler diagrams. Rather than define the color of each pixel like JPG and PNG bitmaps, an SVG defines shapes such as lines, rectangles and circles in XML. For example:

	<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600">
	  <circle cx="400" cy="300" r="50" stroke-width="5" stroke="#f00" fill="#ff0" />
	<svg>
Simpler SVGs are smaller than equivalent bitmaps and can infinitely scale without losing definition.

An SVG can be inlined directly in CSS code as a background image. This can be ideal for smaller, reusable icons and avoids additional HTTP requests. For example:

	.mysvgbackground {
	  background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600"><circle cx="400" cy="300" r="50" stroke-width="5" stroke="#f00" fill="#ff0" /></svg>') center center no-repeat;
	}

## 16. Style SVGs with CSS
More typically, SVGs are embedded directly within an HTML document:

	<body>
	  <svg class="mysvg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600">
	    <circle cx="400" cy="300" r="50" />
	  <svg>
	</body>
This adds the SVG nodes directly into the DOM. Therefore, all SVG styling attributes can be applied using CSS:

	circle {
	  stroke-width: 1em;
	}

	.mysvg {
	  stroke-width: 5px;
	  stroke: #f00;
	  fill: #ff0;
	}
The volume of embedded SVG code is reduced and the CSS styles can be reused or animated as necessary.

Note that using an SVG within an <img> tag or as a CSS background image means they’re separated from the DOM, and CSS styling will have no effect.

## 17. Avoid Base64 Bitmap Images
Standard bitmap JPGs, PNGs and GIFs can be encoded to a base64 string within a data URI. For example:

.myimg {
  background-image: url('data:image/png;base64,ABCDEFetc+etc+etc');
}
Unfortunately:

base64 encoding is typically 30% larger than its binary equivalent
the browser must parse the string before it can be used
altering an image invalidates the whole (cached) CSS file.
While fewer HTTP requests are made, it rarely provides a noticeable benefit — especially over HTTP/2 connections. In general, avoid inlining bitmaps unless the image is unlikely to change often and the resulting base64 string is unlikely to exceed a few hundred characters.

## 18. Consider Critical CSS
Those using Google page analysis tools will often see suggestions to “inline critical CSS” or “reduce render-blocking stylesheets”. Loading a CSS file blocks rendering, so performance can be improved with the following steps:

Extract the styles used to render elements above the fold. Tools such as criticalCSS can help.
Add those to a <style> element in the HTML <head>.
Load the main CSS file asynchronously using JavaScript (perhaps after the page has loaded).
The technique undoubtedly improves performance and could benefit Progressive Web or single-page apps that have consistent interfaces. Gains may be less clear for other sites/apps:

It’s impossible to identify the “fold”, and it changes on every device.
Most sites have differing page layouts. Each one could require different critical CSS, so a build tool becomes essential.
Dynamic, JavaScript-driven events could make above-the-fold changes that are not identified by critical CSS tools.
The technique mostly benefits the user’s first page load. CSS is cached for subsequent pages so additional inlined styles will increase page weight.
That said, Google will love your site and push it to #1 for every search term. (SEO “experts” can quote me on that. Everyone else will know it’s nonsense.)

## 19. Consider Progressive Rendering
Rather than using a single site-wide CSS file, progressive rendering is a technique that defines individual stylesheets for separate components. Each is loaded immediately before a component is referenced in the HTML:

	<head>

	  <!-- core styles used across components -->
	  <link rel='stylesheet' href='base.css' />

	</head>
	<body>

	  <!-- header component -->
	  <link rel='stylesheet' href='header.css' />
	  <header>...</header>

	  <!-- primary content -->
	  <link rel='stylesheet' href='content.css' />
	  <main>

	  <!-- form styling -->
	  <link rel='stylesheet' href='form.css' />
	  <form>...</form>

	  </main>

	  <!-- header component -->
	  <link rel='stylesheet' href='footer.css' />
	  <footer>...</footer>

	</body>
Each <link> still blocks rendering, but for a shorter time, because the file is smaller. The page is usable sooner, because each component renders in sequence; the top of the page can be viewed while remaining content loads.

The technique works in Firefox, Edge and IE. Chrome and Safari “optimize” the experience by loading all CSS files and showing a white screen while that occurs — but that’s no worse than loading each in the <head>.

Progressive rendering could benefit large sites where individual pages are constructed from a selection of different components.

## 20. Learn to Love CSS
The most important tip: understand your stylesheets!

Adding vast quantities of CSS from StackOverflow or BootStrap may produce quick results, but it will also bloat your codebase with unused junk. Further customization becomes frustratingly difficult, and the application will never be efficient.

CSS is easy to learn but difficult to master. You can’t avoid the technology if you want to create effective client-side code. A little knowledge of CSS basics can revolutionise your workflow, enhance your apps, and noticeably improve performance.
