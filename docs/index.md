---
layout: docs
title: Migrating to v4
description: Bootstrap 4 is a major rewrite of the entire project. The most notable changes are summarized below, followed by more specific changes to relevant components.
group: migration
toc: true
---

## Stable changes

Moving from Beta 3 to our stable v4.0 release, there are no breaking changes, but there are some notable changes.

### Printing
- Fixed broken print utilities. Previously, using a `.d-print-*` class would unexpectedly overrule any other `.d-*` class. Now, they match our other display utilities and only apply to that media (`@media print`).

- Expanded available print display utilities to match other utilities. Beta 3 and older only had `block`, `inline-block`, `inline`, and `none`. Stable v4 added `flex`, `inline-flex`, `table`, `table-row`, and `table-cell`.

- Fixed print preview rendering across browsers with new print styles that specify `@page` `size`.

## Beta 3 changes

While Beta 2 saw the bulk of our breaking changes during the beta phase, but we still have a few that needed to be addressed in the Beta 3 release. These changes apply if you're updating to Beta 3 from Beta 2 or any older version of Bootstrap.

### Miscellaneous

- Removed the unused `$thumbnail-transition` variable. We weren't transitioning anything, so it was just extra code.
- The npm package no longer includes any files other than our source and dist files; if you relied on them and were running our scripts via the `node_modules` folder, you should adapt your workflow.

### Forms

- Rewrote both custom and default checkboxes and radios. Now, both have matching HTML structure (outer `<div>` with sibling `<input>` and `<label>`) and the same layout styles (stacked default, inline with modifier class). This allows us to style the label based on the input's state, simplifying support for the `disabled` attribute (previously requiring a parent class) and better supporting our form validation.

  As part of this, we've changed the CSS for managing multiple `background-image`s on custom form checkboxes and radios. Previously, the now removed `.custom-control-indicator` element had the background color, gradient, and SVG icon. Customizing the background gradient meant replacing all of those every time you needed to change just one. Now, we have `.custom-control-label::before` for the fill and gradient and `.custom-control-label::after` handles the icon.

  To make a custom check inline, add `.custom-control-inline`.

- Updated selector for input-based button groups. Instead of `[data-toggle="buttons"] { }` for style and behavior, we use the `data` attribute just for JS behaviors and rely on a new `.btn-group-toggle` class for styling.

- Removed `.col-form-legend` in favor of a slightly improved `.col-form-label`. This way `.col-form-label-sm` and `.col-form-label-lg` can be used on `<legend>` elements with ease.

- Custom file inputs received a change to their `$custom-file-text` Sass variable. It's no longer a nested Sass map and now only powers one string—the `Browse` button as that is now the only pseudo-element generated from our Sass. The `Choose file` text now comes from the `.custom-file-label`.

### Input groups

- Input group addons are now specific to their placement relative to an input. We've dropped `.input-group-addon` and `.input-group-btn` for two new classes, `.input-group-prepend` and `.input-group-append`. You must explicitly use an append or a prepend now, simplifying much of our CSS. Within an append or prepend, place your buttons as they would exist anywhere else, but wrap text in `.input-group-text`.

- Validation styles are now supported, as are multiple inputs (though you can only validate one input per group).

- Sizing classes must be on the parent `.input-group` and not the individual form elements.

## Beta 2 changes

While in beta, we aim to have no breaking changes. However, things don't always go as planned. Below are the breaking changes to bear in mind when moving from Beta 1 to Beta 2.

### Breaking

- Removed `$badge-color` variable and its usage on `.badge`. We use a color contrast function to pick a `color` based on the `background-color`, so the variable is unnecessary.
- Renamed `grayscale()` function to `gray()` to avoid breaking conflict with the CSS native `grayscale` filter.
- Renamed `.table-inverse`, `.thead-inverse`, and `.thead-default` to `.*-dark` and `.*-light`, matching our color schemes used elsewhere.
- Responsive tables now generate classes for each grid breakpoint. This breaks from Beta 1 in that the `.table-responsive` you've been using is more like `.table-responsive-md`. You may now use `.table-responsive` or `.table-responsive-{sm,md,lg,xl}` as needed.
- Dropped Bower support as the package manager has been deprecated for alternatives (e.g., Yarn or npm). [See bower/bower#2298](https://github.com/bower/bower/issues/2298) for details.
- Bootstrap still requires jQuery 1.9.1 or higher, but you're advised to use version 3.x since v3.x's supported browsers are the ones Bootstrap supports plus v3.x has some security fixes.
- Removed the unused `.form-control-label` class. If you did make use of this class, it was duplicate of the `.col-form-label` class that vertically centered a `<label>` with it's associated input in horizontal form layouts.
- Changed the `color-yiq` from a mixin that included the `color` property to a function that returns a value, allowing you to use it for any CSS property. For example, instead of `color-yiq(#000)`, you'd write `color: color-yiq(#000);`.

### Highlights

- Introduced new `pointer-events` usage on modals. The outer `.modal-dialog` passes through events with `pointer-events: none` for custom click handling (making it possible to just listen on the `.modal-backdrop` for any clicks), and then counteracts it for the actual `.modal-content` with `pointer-events: auto`.


## Summary

Here are the big ticket items you'll want to be aware of when moving from v3 to v4.

### Browser support

- Dropped IE8, IE9, and iOS 6 support. v4 is now only IE10+ and iOS 7+. For sites needing either of those, use v3.
- Added official support for Android v5.0 Lollipop's Browser and WebView. Earlier versions of the Android Browser and WebView remain only unofficially supported.

### Global changes

- **Flexbox is enabled by default.** In general this means a move away from floats and more across our components.
- Switched from [Less](http://lesscss.org/) to [Sass](https://sass-lang.com/) for our source CSS files.
- Switched from `px` to `rem` as our primary CSS unit, though pixels are still used for media queries and grid behavior as device viewports are not affected by type size.
- Global font-size increased from `14px` to `16px`.
- Revamped grid tiers to add a fifth option (addressing smaller devices at `576px` and below) and removed the `-xs` infix from those classes. Example: `.col-6.col-sm-4.col-md-3`.
- Replaced the separate optional theme with configurable options via SCSS variables (e.g., `$enable-gradients: true`).
- Build system overhauled to use a series of npm scripts instead of Grunt. See `package.json` for all scripts, or our project readme for local development needs.
- Non-responsive usage of Bootstrap is no longer supported.
- Dropped the online Customizer in favor of more extensive setup documentation and customized builds.
- Added dozens of new [utility classes]({{ site.baseurl }}/docs/{{ site.docs_version }}/utilities/) for common CSS property-value pairs and margin/padding spacing shortcuts.

### Grid system

- **Moved to flexbox.**
  - Added support for flexbox in the grid mixins and predefined classes.
  - As part of flexbox, included support for vertical and horizontal alignment classes.
- **Updated grid class names and a new grid tier.**
  - Added a new `sm` grid tier below `768px` for more granular control. We now have `xs`, `sm`, `md`, `lg`, and `xl`. This also means every tier has been bumped up one level (so `.col-md-6` in v3 is now `.col-lg-6` in v4).
  - `xs` grid classes have been modified to not require the infix to more accurately represent that they start applying styles at `min-width: 0` and not a set pixel value. Instead of `.col-xs-6`, it's now `.col-6`. All other grid tiers require the infix (e.g., `sm`).
- **Updated grid sizes, mixins, and variables.**
  - Grid gutters now have a Sass map so you can specify specific gutter widths at each breakpoint.
  - Updated grid mixins to utilize a `make-col-ready` prep mixin and a `make-col` to set the `flex` and `max-width` for individual column sizing.
  - Changed grid system media query breakpoints and container widths to account for new grid tier and ensure columns are evenly divisible by `12` at their max width.
  - Grid breakpoints and container widths are now handled via Sass maps (`$grid-breakpoints` and `$container-max-widths`) instead of a handful of separate variables. These replace the `@screen-*` variables entirely and allow you to fully customize the grid tiers.
  - Media queries have also changed. Instead of repeating our media query declarations with the same value each time, we now have `@include media-breakpoint-up/down/only`. Now, instead of writing `@media (min-width: @screen-sm-min) { ... }`, you can write `@include media-breakpoint-up(sm) { ... }`.

### Components

- **Dropped panels, thumbnails, and wells** for a new all-encompassing component, [cards]({{ site.baseurl }}/docs/{{ site.docs_version }}/components/card/).
- **Dropped the Glyphicons icon font.** If you need icons, some options are:
  - the upstream version of [Glyphicons](https://glyphicons.com/)
  - [Octicons](https://octicons.github.com/)
  - [Font Awesome](https://fontawesome.com/)
  - See the [Extend page]({{ site.baseurl }}/docs/{{ site.docs_version }}/extend/icons/) for a list of alternatives. Have additional suggestions? Please open an issue or PR.
- **Dropped the Affix jQuery plugin.**
  - We recommend using `position: sticky` instead. [See the HTML5 Please entry](http://html5please.com/#sticky) for details and specific polyfill recommendations. One suggestion is to use an `@supports` rule for implementing it (e.g., `@supports (position: sticky) { ... }`)/
  - If you were using Affix to apply additional, non-`position` styles, the polyfills might not support your use case. One option for such uses is the third-party [ScrollPos-Styler](https://github.com/acch/scrollpos-styler) library.
- **Dropped the pager component** as it was essentially slightly customized buttons.
- **Refactored nearly all components** to use more un-nested class selectors instead of over-specific children selectors.

### By component

This list highlights key changes by component between v3.x.x and v4.0.0.

### Reboot

New to Bootstrap 4 is the [Reboot]({{ site.baseurl }}/docs/{{ site.docs_version }}/content/reboot/), a new stylesheet that builds on Normalize with our own somewhat opinionated reset styles. Selectors appearing in this file only use elements—there are no classes here. This isolates our reset styles from our component styles for a more modular approach. Some of the most important resets this includes are the `box-sizing: border-box` change, moving from `em` to `rem` units on many elements, link styles, and many form element resets.
