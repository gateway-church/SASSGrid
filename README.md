# SASS Grid
A simple but powerful SCSS-based grid system for achieiving 100% fluid grid layouts. It's pretty sweet.

The benefit of SASSGrid vs other grid systems is in its ability to create 100% fluid grid layouts without the need for altering markup to make use of the grid system.

__Table of Contents__
* __[Basic Usage](https://github.com/gateway-church/SASSGrid/blob/master/README.md#basic-usage)__
* __[Getting Started](https://github.com/gateway-church/SASSGrid/blob/master/README.md#getting-started)__
* __[Grid System Basics](https://github.com/gateway-church/SASSGrid/blob/master/README.md#grid-system-basics)__
	* @mixin col()
	* Gutters
	* Automatically setting up predefined column classes
* __[Creating Responsive Breakpoints](https://github.com/gateway-church/SASSGrid/blob/master/README.md#creating-breakpoints)__
	* @mixin break-contain
* __[Helpful Mixins](https://github.com/gateway-church/SASSGrid/blob/master/README.md#helpful-mixins)__
	* @mixin triangle()
	* @mixin trans-bg()
	* @mixin gradient()
	* @mixin gradient-trans()
	* @mixin transitions()
	* @mixin outer-glow()
	* @mixin bevel()
	* @mixin border-radius()


## Basic Usage
```scss
@import 
	"sassgrid/_variables.scss",
	"sassgrid/_reset.scss",
	"sassgrid/_grid.scss",
	"sassgrid/_mixin.scss";

//Column Number is used to determine to total amount of columns you desire to generate
$col-number: 4;

@include break-contain(desk) {
	@include col-build(desk,$col-number);
}

@include break-contain(tablet-desk) {
	@include col-build(tablet,$col-number);
}

@include break-contain(v-tablet-tablet) {
	@include col-build(v-tablet,$col-number);
}

@include break-contain(mobile-v-tablet) {
	@include col-build(mobile,$col-number);
}

@include break-contain(v-mobile-mobile) {
	@include col-build(v-mobile,$col-number);
}

.parent-div {
	/*
		* Example Usage for grid() mixin*
		The grid() mixin will take all children of a given parent <div>
		and make all of its children fit into a grid system defined below.
		It's important to define all breakpoints to avoid inheritance issues between
		breakpoints.
	*/
	
	@include grid(desk,2,5);
	@include grid(tablet-desk,2,5);
	@include grid(v-tablet-tablet,2,5);
	@include grid(mobile-v-tablet,1,0);
	@include grid(v-mobile-mobile,1,0);
}


// Sample Column Styles with responsive breakpoints
.col-1-2 {
	// Will be a 50% column with a 5% margin down to
	// the v-tablet breakpoint at which point it becomes 100% width
	@include col(1,2,5);
	
	@include break(v-tablet) {
		width: 100%;
		float: none;
	}
	
	&:last-child {
		@include col-last(1,2,5);
	}
	
}

.col-2-3 {
	@include col(2,3,5);
  @include break(v-tablet) {
		width: 100%;
		float: none;
	}

  &:last-child {
		@include col-last(2,3,5);
	}
  
}

.col-1-3 {
	@include col(1,3,5);
  @include break(v-tablet) {
		width: 100%;
		float: none;
	}
  
  &:last-child {
  	@include col-last(1,3,5);
	}
  
}

.col-1-3-last {
	@include col-last(1,3,5);
		@include break(v-tablet) {
		width: 100%;
		float: none;
	}

}
```

## Getting Started
Thank you for using SASSGrid! Below are a few steps to help you get started using SASSGrid in your projects.



## Grid System Basics
The core of SASSGrid is the grid system. Unlike many popular grid systems SASSGrid doesn't include predefined class selectors out of the box, instead allowing you complete flexability to create your own column structure as needed. One of the great features of SASSGrid is that there's now need for a wrapper container around your set of columns unless you want one, giving you more semantically accurate markup.

### @mixin col()
The primary mixin you're going to be using for your columns is the col() mixin. There is also a col-last() mixin that will be used in some cases, we'll discuss that next.

| Argument     | Values                                                             | Default  |
| :--------    | :--------                                                          | :------- |
| col-count    | (int) The number of total-col you want this col to span, numerator | --       |
| total-col    | (int) The width                                                    | --       |
| gutter-width |                                                                    |          |

The col() mixin's first two arguments are actually the fraction of total width you want the column to take up. So, let's say you wanted a column that was half the width of its parent container. You would do that like so...

```scss
.col-1-2 {
	// produces a column that's half of the width of its parent.
	@include col(1,2,0);
}
```

Notice the first two arguments create the fraction 1/2. Here's how you'd make a 2/3 column.

```scss
.col-2-3 {
	// produced a column that's 2/3 of the width of its parent.
	@include col(2,3,0);
}
.col-1-3 {
	// produces a column that's 1/3 of the width of its parent.
	@include col(1,3,0);
}
```

Please note that the col() mixin always applies a float: left attribute so what if you want to have a column that floats right? That's where the col-last() mixin comes in!

```scss
.col-2-3 {
	@include col(2,3,0);
}

.col-1-3-last {
	// produces a column that's 1/3 of the width of its parent.
	@include col-last(1,3,0);
}
```

The above will produce two columns, one that's 2/3 of the container width and another that's 1/3 of the container width that will float left and right respectively. Next we'll talk about gutters between columns.


### Gutters
You'll typically find it asthetically pleasing to have a bit of margin between your columns, which is where gutters come in handy.

The gutter width is a percentage of space that you want to have built-in between columns, so a gutter width value of 1 is equal to 1%. In addition to manually defining a gutter width you can also use the built-in gutter width variables outlined below.

| Variable   | Default Value   |
| ---------- | --------------- |
| $gut-no    | 0               |
| $gut-sm    | .1              |
| $gut-med   | 3               |
| $gut-lg    | 20              |

These variables are definedin sassgrid/_variables.scss and can be modified to suit your needs. Please note that these gutter variables are used in the predefined column classes outlined below to generate predefined column classes with and without gutters.

### Automatically setting up predefined column classes
Should you find it necessary or helpful to have a predefined set of column classes that you can easily integrate into your markup you can use @mixin col-build.

```scss
//Column Number is used to determine to total amount of columns you desire to generate
$col-number: 4;

@include break-contain(desk) {
	@include col-build(desk,$col-number);
}

@include break-contain(tablet-desk) {
	@include col-build(tablet,$col-number);
}

@include break-contain(v-tablet-tablet) {
	@include col-build(v-tablet,$col-number);
}

@include break-contain(mobile-v-tablet) {
	@include col-build(mobile,$col-number);
}

@include break-contain(v-mobile-mobile) {
	@include col-build(v-mobile,$col-number);
}
```

This will setup a large variety of predefined classes that you can easiy use to apply columning to your markup. The classes generated will be named as follows (assuming the $col-number variable is set to 4):

| Classes for Desk and larger   | Classes for tablet   | Classes for v-tablet   | Classes for Mobile   |
| ----------------------------- | -------------------- | ---------------------- | -------------------- |
| .desk-1-4-gut-lg              | .tablet-1-4-gut-lg   | .v-tablet-1-4-gut-lg   | .mobile-1-4-gut-lg   |
| .desk-1-4-gut-med             | .tablet-1-4-gut-med  | .v-tablet-1-4-gut-med  | .mobile-1-4-gut-med  |
| .desk-1-4-gut-sm              | .tablet-1-4-gut-sm   | .v-tablet-1-4-gut-sm   | .mobile-1-4-gut-sm   |
| .desk-1-4                     | .tablet-1-4          | .v-tablet-1-4          | .mobile-1-4          |
| .desk-1-3-gut-lg              | .tablet-1-3-gut-lg   | .v-tablet-1-3-gut-lg   | .mobile-1-3-gut-lg   |
| .desk-1-3-gut-med             | .tablet-1-3-gut-med  | .v-tablet-1-3-gut-med  | .mobile-1-3-gut-med  |
| .desk-1-3-gut-sm              | .tablet-1-3-gut-sm   | .v-tablet-1-3-gut-sm   | .mobile-1-3-gut-sm   |
| .desk-1-3                     | .tablet-1-3          | .v-tablet-1-3          | .mobile-1-3          |
| .desk-1-2-gut-lg              | .tablet-1-2-gut-lg   | .v-tablet-1-2-gut-lg   | .mobile-1-2-gut-lg   |
| .desk-1-2-gut-med             | .tablet-1-2-gut-med  | .v-tablet-1-2-gut-med  | .mobile-1-2-gut-med  |
| .desk-1-2-gut-sm              | .tablet-1-2-gut-sm   | .v-tablet-1-2-gut-sm   | .mobile-1-2-gut-sm   |
| .desk-1-2                     | .tablet-1-2          | .v-tablet-1-2          | .mobile-1-2          |
| .desk-full                    | .tablet-full         | .v-tablet-full         | .mobile-full         |

What might not be obvious is that by combining multiple classes you can create responsive behaviour.

```html
<div class="desk-1-4 tablet-1-2">
	This will produce a column that goes from 1/4 width of its container on desktop to 1/2 of its container once the tablet breakpoint is triggered and remain 1/2 all the way down to mobile.
</div>

<div class="desk-1-4 tablet-1-2 v-tablet-full">
	This will produce a column that goes from 1/4 width of its container on desktop to 1/2 of its container once the tablet breakpoint is triggered and then goes full width with the v-tablet breakpoint is reached.
</div>
```


## Creating Breakpoints
To make using breakpoints as simple as possible we've created a handy mixin called __break__ which allows you to attribute specific styling to devices with screen resolutions within a specific range. 

By default the breakpoints included in SASSGrid are:

| breakpoint | Default width  |
| ---------- | -------------- |
| desk       | >= 960px       |
| tablet     | 960px          |
| v-tablet   | 760px          |
| mobile     | 660px          |
| v-mobile   | 500px          |

Think of these breakpoints as the width of the viewer/browser at which each breakpoint will be triggered. So, when a browser is 960px or wider it'll fall in the desk breakpoint. If it's between 761 and 960 it'll fall into tablet, etc.

Let's start with an example of how you would create a 50% column that would expand to full width for vertical tablets (v-tablet).

```scss
// Sample Column Styles with responsive breakpoints
.col-1-2 {
	// Will be a 50% column with a 5% margin down to
	// the v-tablet breakpoint at which point it becomes 100% width
	@include col(1,2,5);
	
	@include break(v-tablet) {
		width: 100%;
		float: none;
	}
}
```

Here's how you would do the same but instead of having it change on v-tablet it'll only change on mobile resolutions.

```scss
// Sample Column Styles with responsive breakpoints
.col-1-2 {
	@include col(1,2,5);
	
	@include break(mobile) {
		width: 100%;
		float: none;
	}
}
```
#### @mixin break-contain
Sometimes you'll run into a situation (because of CSS inheritance mostly) where you'll need to contain or restrain a style to a very specific range of breakpoints. For example you may want to apply a style to an element ONLY if it falls in the desktop range and NOT have the style inherited down through the mobile breakpoint. To do this you use the break-contain mixin.

```scss
// Only show an element for larger screens
.desktop-only {
	display: none;

	@include break-contain(desk) {
		display: initial;
	}
}
```

## Helpful Mixins
One of the coolest parts of SASSGrid is the (ever growing) collection of helpful mixins. Below are just a couple of them.

#### @mixin triangle
The triangle mixin is a handy shorthand for creating 100% pure CSS triangles/arrows. It is commonly used inside of a &:before or &:after pseudo element in order to append a triangle to an element.
	
| Argument     | Values                                                                | Defaults  |
| ------------ | :-------------------------------------------------------------------- | :-------- |
| direction    | **up**, down, left, right, downComm                                   | up        |
| color        | *(any valid CSS color value)*                                         | white     |
| width        | a numeric value that represents the desired width of the triangle     | 100       |
| height       | a numeric value that represents the desired height of the triangle    | $width    |
	
**Example Usage**
```scss
.element {
	&:before {
		@include triangle(down,white,10,10);
	}
}
```

#### @mixin trans-bg
The trans-bg mixin is a quick and easy way to generate a background color with support for alpha transparency. 

| Argument  | Values                                                                  | Default  |
| --------- | :--------                                                               | :------- |
| color     | Any valid CSS color value                                               | --       |
| alpha     | a numeric value from 0 to 1 representing the opacity for the background | 1        |

**Example Usage**
```scss
.element {
	@include trans-bg(#ffffff,.5);
}
```

#### @mixin gradient
Allows quick and easy creation of CSS-based gradients for the background of an element.

| Argument    | Values                    | Default  |
| ---------   | :--------                 | :------- |
| start_color | Any valid CSS color value | --       |
| end_color   | Any valid CSS color value | --       |

**Example Usage**
The following style would generate a gradient that goes from white to black.
```scss
.element {
	@include gradient(white,black);
}
```

#### @mixin gradient-trans
Allows quick and easy creation of CSS-based gradients for the background of an element.

| Argument    | Values                                                                  | Default           |
| ---------   | :--------                                                               | :-------          |
| start_color | Any valid CSS color value                                               | darken($grey,20%) |
| start_alpha | a numeric value from 0 to 1 representing the opacity for the background | 0                 |
| end_color   | Any valid CSS color value                                               | darken($grey,60%) |
| end_alpha   | a numeric value from 0 to 1 representing the opacity for the background | .5                |

**Example Usage**
The following style would generate a gradient that goes from fully opaque white to 20% opaque white.
```scss
.element {
	@include gradient-trans(white,1,white,.2);
}
```

#### @mixin transitions
A quick shorthand mixin for adding a transition property to an element.

| Argument  | Values                                                       | Default     |
| --------  | :------                                                      | :-------    |
| selector  | attribute/property to which you want to apply the transition | all         |
| animation | the easing method to use for the transition                  | ease-in-out |

**Example Usage**
```scss
a { // Apply transitions to all attributes
	background: red;
	@include transitions();

	&:hover {
		background: blue;
	}
}
```

#### @mixin outer-glow
Used to create a subtle outer-glow effect similar to Adobe's Photoshop.

| Argument | Values                                                        | Default           |
| -------- | :------                                                       | :-------          |
| size     | The number of pixels the glow should eminate from the element | 10                |
| spread   | The number of pixels the glow should spread from the element  | 0                 |
| color    | The color of the glow, any valid CSS color value              | rgba(0, 0, 0, .4) |

**Example Usage**
```scss
.element {
	@include outer-glow($color:red);
}
```

#### @mixin bevel
Used to create a beveled effect on a given element.

| Argument | Values                                                         | Default  |
| -------- | :------                                                        | :------- |
| size     | The thickness of the bevel (in pixels)                         | 1        |
| strength | How strong/opaque the bevel should be (opacity)                | .3       |
| reverse  | true/false - wether or not the bevel effect should be reversed | false    |

**Example Usage**
```scss
.element {
	@include bevel();
}
```

#### @mixin border-radius
Used to create cross-browser css-based rounded corners

| Argument | Values                   | Default                                        |
| -------- | :------                  | :-------                                       |
| b-radius | The radius of the border | $b-radius-default (defined in _variables.scss) |

**Example Usage**
```scss
.circle { // Create a circle
	width: 100px;
	height: 100px;
	@include border-radius(50%);
}

.round-box { // Create a soft rounded box
	@include border-radius(3px);
}
```

In addition to the border-radius mixin we've also included a few useful mixins for generating a border radius on individual corners as well as top, bottom, left and right corners.

```scss
@mixin border-top-radius($b-radius:$b-radius-default);

@mixin border-bottom-radius($b-radius:$b-radius-default);

@mixin border-left-radius($b-radius:$b-radius-default);

@mixin border-right-radius($b-radius:$b-radius-default);

@mixin border-top-left-radius($b-radius:$b-radius-default);

@mixin border-bottom-left-radius($b-radius:$b-radius-default);

@mixin border-top-right-radius($b-radius:$b-radius-default);

@mixin border-bottom-right-radius($b-radius:$b-radius-default);
```

