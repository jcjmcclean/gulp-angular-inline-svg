# melon-gulp-angular-inline-svg

You:
- have SVG files you’d like to use
- are using Angular 1.x
- don’t want to use sprites

We:
- provide a Gulp task that concatenates your icons into an Angular constant

You:
- inject the constant into your components (or create a component wrapper)
- drop the SVG into your markup using `ng-bind-html` or something similar
- use CSS to size and style your SVG files in markup
- celebrate

## Installation

```
npm install gulp-angular-inline-svg
```

## Gulp Example

```
var gulp = require( 'gulp' );
var icons = require( 'gulp-angular-inline-svg' );

gulp.task('icons', function(cb) {
	pump([
		gulp.src('./app/assets/icons/*.svg'),
		icons({
			module: 'app',
			constant: 'ICONS',
			optimize: true,
			file: 'icon.constant.js'
		}),
		gulp.dest('./app/components/common/config')
	], cb);
});
```

## Options
The task `icons` takes in an options object like so `.pipe( icons( options ) )`. The available options are:
- `module` the name of the Angular module that goes into the result file
- `constant` the name of the Angular constant that goes into the result file
- `file` the result filename
- `optimize` whether to run the files through SVGO to optimize the markup

## Usage Recommendation

The best way to use this is through a component like this:

```
angular
	.module('app')
	.component('icon', {
		controller: iconController,
		template: '<span ng-bind-html="$ctrl.markup"></span>',
		bindings: {
			name: '@'
		}
	});

function iconController(ICONS, $sce) {
	if(ICONS && ICONS[this.name] && ICONS[this.name].data) {
		this.markup = $sce.trustAsHtml(ICONS[this.name].data);
	}
}
```

which you can use in a page like so:

```
<icon name="clock"></icon>
```
