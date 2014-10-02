# SASS Grid
A simple but powerful SCSS-based grid system for achieiving 100% fluid grid layouts. It's pretty sweet.

## Sample Usage
```sass
@import 
  "sassgrid/_variables.scss",
	"sassgrid/_reset.scss",
	"sassgrid/_grid.scss",
	"sassgrid/_mixin.scss";


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
## Helpful Mixins
One of the coolest parts of SASSGrid is the (ever growing) collection of helpful mixins. Below are just a couple of them.

#### @mixin triangle
The triangle mixin is a handy shorthand for creating 100% pure CSS triangles/arrows. It is commonly used inside of a &:before or &:after pseudo element in order to append a triangle to an element.
	
| Argument     | Values                                                               |
| ------------ | -------------------------------------------------------------------- |
| $direction   | **up**, down, left, right, downComm                                  |
| $color       | *(any valid CSS color value)*                                        |
| $width       | a numeric value that represents the desired width of the triangle    |
| $height      | a numeric value that represents the desired height of the triangle   |
	
**Example Usage**
```sass
.element {
	&:before {
		@include triangle(down,white,10,10);
	}
}
```

#### @mixin trans-bg
The trans-bg mixin is a quick and easy way to generate a background color with support for alpha transparency. 

| Argument  | Values                                                                  |
| --------- | --------                                                                |
| $color    | Any valid CSS color value                                               |
| $alpha    | a numeric value from 0 to 1 representing the opacity for the background |

**Example Usage**
```sass
.element {
	@include trans-bg(#ffffff,.5);
}
```

#### @mixin gradient-trans
Allows quick and easy creation of CSS-based gradients for the background of an element.

| Argument     | Values                                                                  |
| ---------    | --------                                                                |
| $start_color | Any valid CSS color value                                               |
| $start_alpha | a numeric value from 0 to 1 representing the opacity for the background |
| $end_color   | Any valid CSS color value                                               |
| $end_alpha   | a numeric value from 0 to 1 representing the opacity for the background |

**Example Usage**
The following style would generate a gradient that goes from fully opaque white to 20% opaque white.
```sass
.element {
	@include gradient-trans(white,1,white,.2);
}
```