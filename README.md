# Enhanced CSS Slider

> [!warning]
> This project is in its early stages and can be very unstable. The goal is to improve it and standardize its API as it's used in more projects.

Enhanced CSS Slider is a web component that wraps up all the JavaScript logic needed to:

- Add Previous/Next arrows to a slider;
- Add looping functionality;

It **does not add or alter any markup**, it merely adds behaviors to existing elements.

## How to use

Enhanced CSS Slider assumes you have a scrollable element with the following CSS properties:

```css
.parent {
  scroll-snap-type: x mandatory;
}
.child {
  scroll-snap-align: start | center;
}
```

More info about scroll-snap can be seen [on this blog post](https://fantinel.dev/css-scroll-snapping).

With that, you can just wrap your scrollable element in the `enhanced-css-slider` component:

```html
<enhanced-css-slider>
  <ul class="scrollable-list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
  </ul>
</enhanced-css-slider>
```

> [!info]
> Enhanced CSS Slider will look for any `ul` elements inside of it and automatically assume that is the scrollable div that contains the slides. However, in case you need to explicitly tell it what the scrollable element is, you can add the `data-slider-slot="list"` attribute to it.

### Adding previous/next buttons

Since this component doesn't provide any markup, you need to pass in the navigation buttons on your own. Enhanced CSS Slider will then look for elements with the `prev` and `next` classes or `data-slider-slot="prev|next"` attributes and add the event listeners to it.

```html
<enhanced-css-slider>
  <ul class="scrollable-list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
  </ul>

  <button data-slider-slot="prev">Prev</button>
  <button data-slider-slot="next">Next</button>
</enhanced-css-slider>
```

### Adding a current slider indicator

Similar to prev/next buttons, you can also pass an element with the `current` class or `data-slider-slot="current"` attribute, and Enhanced CSS Slider will automatically set the text of that element to the current slider index.

```html
<enhanced-css-slider>
  <ul class="scrollable-list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
  </ul>

  <div>
    Slide <span data-slider-slot="current"></span> of 3
  </div>

  <button data-slider-slot="prev">Prev</button>
  <button data-slider-slot="next">Next</button>
</enhanced-css-slider>
```

## Props

| Prop Name       | Default | Description                                                                                                                                                   |
| --------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| centered        | false   | Use it if the active slider is center-aligned instead of aligned to the left.                                                                                 |
| loop            | false   | Whether the slider should loop around. This creates clones of the first/last slides in order to give an illusion that the slides are looping.                 |
| slides-to-clone | 2       | Only used in `loop` mode. This controls how many slides will be cloned. Increase this number in case the 2 slides aren't enough to give the looping illusion. *Ideally this will be done automatically in the future.* |

## Slots

| Slot    | Description                                                                                                        |
| ------- | ------------------------------------------------------------------------------------------------------------------ |
| list    | The scrollable element. Can be set with `data-slider-slot="list"` or by being a `ul` element.                      |
| current | The current active index of the slider. Can be set with `data-slider-slot="current"` or with the `current` class.  |
| prev    | The button that scrolls to the previous slide. Can be set with `data-slider-slot="prev"` or with the `prev` class. |
| next    | The button that scrolls to the next slide. Can be set with `data-

## Events

| Event        | Params              |
| ------------ | ------------------- |
| slideChanged | activeSlide (index) |

## Dealing with JavaScript unavailability

[In cases where JavaScript isn't available](https://www.kryogenix.org/code/browser/everyonehasjs.html), then this component won't be able to render at all. Luckily, since we're using CSS scroll-snap, the slider should still work normally. However, it's a good idea to hide the elements of the page that won't work. You can do that with a `noscript` tag:

```html
<enhanced-css-slider>
  <ul class="scrollable-list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
  </ul>

  <div class="progress">
    Slide <span data-slider-slot="current"></span> of 3
  </div>

  <div class="navigation">
    <button data-slider-slot="prev">Prev</button>
    <button data-slider-slot="next">Next</button>
  </div>

  <noscript>
      <!-- Hide JS-only elements if JavaScript is unavailable -->
      <style>
          .progress,
          .navigation {
            display: none !important;
          }
      </style>
  </noscript>
</enhanced-css-slider>
```

## Handling cases where slider is not needed

In situations where there is more than enough space to fit all children in the viewport without scrolling, Enhanced CSS Slider will automatically disable the prev/next arrows and add the `no-scroll` class to your list. You can then use CSS to apply any styling rules you'd like. **This behavior is not present if `centered` or `loop` are true.**
