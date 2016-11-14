<article id="post-247229" class="instapaper_body h-entry e-content">
One of the things that I'm really interested in about CSS is the new 
[color-mod][1] [function][2]. It will give us the ability to do color
manipulations right in the browser. For example, when hovering over a button, 
you can change the color by using something like
`color: color(black darkness(50%));`, without the use of any CSS preprocessor
like Sass.

But as the support of these CSS color functions is zero nowadays, we can 
[temporarily][3] use PostCSS and compile them as regular colors. Or we can
experiment and discover the power of CSS`rgba()` color functions to change
colors on the fly! Let's see how we can use it.

### How it works<figure id="post-247472" class="align-right media-247472">

![basic idea][4]</figure>
In a design app, when we place a black and white boxes over a bigger box with a
blue background color (As in the example), the result will be a lighter and 
darker blue, respectively.

That's because when decreasing the opacity, the colors will blend together
based on if it's white or black. Can you expect what will happen if we changed 
the blue background to green? You guessed it!<figure id="post-247471" class="
align-right media-247471
">

![Basic Idea Green][5]</figure>
As you see, by changing the background color to green, the small boxes looks
different now! We have light and dark green. We also can change the opacity 
value to pick a darker or lighter color. Let's work off this basic premise to 
dive into some real-world examples.

### Applying the concept

To keep the above example concise, we played with the opacity. In our actual
design, we will need to use`rgba()` alpha value.

    .header {
      background: rgba(0, 0, 0, 0.5); /* Black color with 50% alpha/opacity */
    }

We'll use the background instead of opacity here because if we used the opacity
value, all of the child elements will be affected, which is not what we want to 
achieve. If we use the alpha channel of the background property, we're ensured 
that we'll only update the element that we'd like to change.

Note: in the demos we will use Sass `rgba()` function to make things faster.
For example:

    .elem {
      background: rgba(white, 0.5);
    }

will be translated to:

    .elem {
      background: rgba(255, 255, 255, 0.5);
    }

#### Theming a website header

The first use case for `rgba()` is to theme a web app header. In [Trello][6]
they are using a background color with`rgba()` for the header child elements (
logo, search input, buttons
):

    .search {
      background: rgba(255, 255, 255, 0.5); /* White with 50% alpha */
    }<figure id="post-247464" class="align-right media-247464">

![Trello with Devtools][7]</figure>
With that in place, we will get the ability to theme the header by only
changing its background, and then the child elements will change as well.

In our example, we will make something similar of Trello header and play with
the background from dev tools.<figure id="post-247467" class="align-right media
-247467
">

![Trello Header][8]</figure>
Notice how the child elements background color is different in the 2 headers.
If we want to inspect element and change the parent background, we can theme our
header very easily.<figure id="post-247480" class="align-right media-247480">

![Theming Header][9]</figure>
See the Pen [Header][10] by Ahmad Shadeed ([@shadeed][11]) on [CodePen][12].

#### Article header<figure id="post-247465" class="align-right media-247465">

![Article Header][13]</figure>
In this example, using `rgba()` will be beneficial for:

*   Borders at the top and bottom edges.
*   Background color for the centered wrapper.
*   Background color for the category link.<figure id="post-247477" class="
align-right media-247477
">

![Article Header Annotated][14]</figure>
    .parent {
      background: #5aaf4c; /* parent background */
      box-shadow: 
        inset 0 8px 0 0 rgba(255, 255, 255, 0.5), 
        inset 0 -8px 0 0 rgba(255, 255, 255, 0.5); /* top and bottom borders */
    }
    
    .contain {
      background: rgba(0, 0, 0, 0.1);
    }
    
    .category {
      background: rgba(255, 255, 255, 0.5);
    }

See the Pen [Article Header][15] by Ahmad Shadeed ([@shadeed][11]) on 
[CodePen][12].

#### Buttons

When theming buttons, we can use `rgba()` to make the hover and focus effects
for things like borders, shadows.

    .button {
      background: #026aa7;
      box-shadow: inset 0 -4px 0 0 rgba(0, 0, 0, 0.2);
    }
    
    .button:hover {
      box-shadow: inset 0 -4px 0 0 rgba(0, 0, 0, 0.6), 0 0 8px 0 rgba(0, 0, 0, 0.5);
    }
    
    .button:focus {
      box-shadow: inset 0 3px 5px 0 rgba(0, 0, 0, 0.2);
    }

See the Pen [Buttons][16] by Ahmad Shadeed ([@shadeed][11]) on [CodePen][12].

#### Gradients

Another useful use case is to add the background as a solid color and then use
a pseudo element with a`rgba()` colors for the gradient color stops.<figure id
="post-247473" class="align-right media-247473
">

![Gradients][17]</figure>
    .elem {
      background: green;
    }
    
    .elem:before {
      content: "";
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(to right, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.7));
    }

This will also give us the ability to animate the gradients by only changing
the background color.

    .elem {
      /* other styles */
      animation: bg 2s ease-out alternate infinite;
    }
    
    @keyframes bg 
      to {
        background: green;
      }
    }

See the Pen [Gradients][18] by Ahmad Shadeed ([@shadeed][11]) on [CodePen][12]

#### Sub Element

If we have a navigation list inside a header element, we can add a background
color with`rgba()` to the navigation. This will make the background a bit
transparent and so it will blend with the parent background.

See the Pen [Sub Element][19] by Ahmad Shadeed ([@shadeed][11]) on 
[CodePen][12].

And the same could be used for making dynamic hover effects:

See the Pen [Hover Effect][20] by Ahmad Shadeed ([@shadeed][11]) on 
[CodePen][12].

#### Dark/Light Variations of a color palette

We can use this concept to generate different shades of a specific color
palette by positioning a pseudo element on each color box with a specific
`rgba()` value.

See the Pen [Color Palette][21] by Ahmad Shadeed ([@shadeed][11]) on 
[CodePen][12].

#### Image Effects

If we want to make an image darker or lighter, we can use a pseudo element with
`rgba()` background.<figure id="post-247474" class="align-right media-247474
">

![][22]</figure>
By using a colored background, we can create a color tint effect. Other than
that, we can use`mix-blend-mode` property to blend the background with the
image so we can different results.

It's important to check support tables before using `mix-blend-mode`:

    .elem:before {
      background: rgba(0, 0, 0, 1);
      mix-blend-mode: color;
    }

If `mix-blend-mode` is not supported, the image will be black and the user won'
t get it. It's better to work on such effects as an enhancement, but don't 
depend on it. To do so, you can use`@support` or Modernizr. 

    @supports (mix-blend-mode: color) {
      /* your enhanced styles go there */
    }

See the Pen [Images][23] by Ahmad Shadeed ([@shadeed][11]) on [CodePen][12].

### Theming with CSS Variables

By using [CSS Variables][24] (custom properties) for the parent elements, when
the variable is changed then all child elements will be affected. For example:

    :root {
      --brand-color: #026aa7;
    }
    
    /* Checking for the support of CSS Variables */
    @supports (--color: red) {
      .elem {
        background: var(--brand-color);
      }
    }

    var colors = ["#026aa7", "#5aaf4c", "#00bcd4", "#af4c4c"];
    var colorOptions = document.querySelectorAll(".option");
    var colorLabels = document.querySelectorAll(".option + label");
    
    for (var i = 0; i < colorOptions.length; i++) {
    
      /* Add an event listener to each radio button */
      colorOptions[i].addEventListener('click', changeColor);
    
      /* Add a value to each radio button based on colors[] array */
      colorOptions[i].value = colors[i];
    
      colorLabels[i].style.background = colors[i];
    }
    
    function changeColor(e) {
      /* calling the root element and set the value of a specific property. In our case: --brand-color */
      document.documentElement.style.setProperty('--brand-color', e.target.value);
    }

By combining CSS Variables and `rgba()`, we can make our layouts and colors a
bit more dynamic without re-defining a new color for each element.

See the Pen [Header - CSS Variables][25] by Ahmad Shadeed ([@shadeed][11]) on
[CodePen][12].

### Things to take in consideration

#### Fallback Color

Although the global support for CSS colors is [97.39%][26], it's important to
provide a fallback for non-supporting browsers.

    .elem {
      background: #fff; 
      background: rgba(255, 255, 255, 0.5); /* non supporting browsers will ignore this declaration */
    }

#### Color Contrast Check

We should ensure that the contrast between elements meets the accessibility
guidelines. Sometimes, using`rgba()` might result in a poor color that is very
hard to read. You can use tools like Lea Verou's[Contrast Check][27] to help
determine if colors meet accessibility standards.</article>

 [1]: https://drafts.csswg.org/css-color/#modifying-colors
 [2]: https://github.com/postcss/postcss-color-function
 [3]: https://cloudfour.com/thinks/building-themes-with-css4-color-features/
 [4]: img/basic-idea-768x442.jpg%20768w
 [5]: img/basic-idea-green-768x442.jpg%20768w
 [6]: https://trello.com
 [7]: img/trello-768x211.jpg%20768w
 [8]: img/Screen-Shot-2016-10-22-at-6.38.58-AM-768x156.png%20768w
 [9]: img/theming-header.gif
 [10]: http://codepen.io/shadeed/pen/79bb883e8f17c41a90f46c8fdf1a40e2/
 [11]: http://codepen.io/shadeed
 [12]: http://codepen.io
 [13]: img/Screen-Shot-2016-10-22-at-6.56.33-AM-768x307.png%20768w
 [14]: img/article-header-768x306.jpg%20768w
 [15]: http://codepen.io/shadeed/pen/fc254c1f120cc38a1b199f96d1d07a85/
 [16]: http://codepen.io/shadeed/pen/f822dd1cac006c00d5418cddac19efac/
 [17]: img/gradients-768x490.jpg%20768w
 [18]: http://codepen.io/shadeed/pen/808cbbdee59ddffdd39c91c2f1a290f1/
 [19]: http://codepen.io/shadeed/pen/760a044dd7e6a7fed972ce3ee08dc02a/
 [20]: http://codepen.io/shadeed/pen/eb5a955b69c2e31b3dcc0e9131db21b2/
 [21]: http://codepen.io/shadeed/pen/c2a60d0ed9e0313a76cc2c24e05402c7/
 [22]: img/images-768x227.jpg%20768w
 [23]: http://codepen.io/shadeed/pen/759699e8fc8d5253540fd33880cafa81/
 [24]: https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables
 [25]: http://codepen.io/shadeed/pen/a78a3d28eee617784cc75e2fa3dfad76/
 [26]: http://caniuse.com/#search=rgba
 [27]: http://leaverou.github.io/contrast-ratio/