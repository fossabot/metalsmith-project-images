# Metalsmith Project Images

[![npm version](https://badge.fury.io/js/metalsmith-project-images.svg)](https://badge.fury.io/js/metalsmith-project-images)
[![Build Status](https://travis-ci.org/hoetmaaiers/metalsmith-project-images.svg?branch=master)](https://travis-ci.org/hoetmaaiers/metalsmith-project-images)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FGosha%2Fmetalsmith-project-images.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2FGosha%2Fmetalsmith-project-images?ref=badge_shield)

A metalsmith plugin that can scan all images in subfolders and add it to a files metadata.

## The idea

Let's say we want to show all images per project. Assume we have a folder structure like below:

```
projects/
|-- hello/
|	|-- hello.md
|	|-- images/
|		|-- image-1.png
|		|-- image-2.png
|-- world/
	|-- world.md
	|-- images/
		|-- beautiful-world.png
		|-- skyfall.jpg
```

This would be possible with the following configuration:

```
var Metalsmith = require('metalsmith');
var images = require('metalsmith-project-images');

var metalsmith = new Metalsmith(__dirname)
  .use(images({
    pattern: 'projects/**/*.md'
  })
  .build();
```

Combined with the [collections metalsmith plugin](https://github.com/segmentio/metalsmith-collections), we can loop over each collection and have access to the images for each project.

```javascript
{{#each project in projects}}
	<h1>{{project.title}}</h1>
	<ul>
		{{#each image in project.images}}
			<li><img src="{{image}}"/></li>
		{{/each}}
	</ul>
{{/each}}
```

## Install

```sh
npm install --save metalsmith-project-images
```

## Quick usage

```js
var Metalsmith = require('metalsmith');
var images = require('metalsmith-project-images');

var metalsmith = new Metalsmith(__dirname)
  .use(images({
  	pattern: 'projects/**/*'
  }))
  .build();
```

## Api

```javascript
var metalsmith = new Metalsmith(__dirname);

var defaultOptions = {
	authorizedExts: ['jpg', 'jpeg', 'svg', 'png', 'gif', 'JPG', 'JPEG', 'SVG', 'PNG', 'GIF'],
	pattern: '',
	imagesDirectory: 'images',
};

metalsmith.use(images(defaultOptions))
// or passing in no options will use the defaults
metalsmith.use(images())
```

##### One options object
```javascript
var options = {
	authorizedExts: ['gif']
	pattern: 'memes/**/*.md',
	imagesDirectory: 'giphys',
}
```

##### Multiple options objects
It is possible to define multiple configuration objects.

```javascript
var options = [
	// only add gif's to memes ;)
	{
		authorizedExts: ['gif']
		pattern: 'memes/**/*.md',
		imagesDirectory: 'giphys',
	},

	// add all images to its matching project
	{
		pattern: 'projects/**/*.md',
	},

	// add all images to its matching project with a different metadata key
	{
		pattern: 'projects/**/*.md',
        imagesDirectory: 'maps',
        imagesKey: 'maps',
	},
]
```

### Options

| name | default | description |
|:------------- |:-------------|:--|
| pattern | `**/*.md` | pattern for files to scan images for |
| authorizedExts | jpg, jpeg, svg, png, gif, JPG, JPEG, SVG, PNG, GIF | allowed image extensions |
| imagesDirectory | `images` | directory inside the pattern to look for images to add |
| imagesKey | `images` | name of metadata key to hold images collection |

Note:

- If `imagesDirectory` does not exist, it is skipped and no collection will be created

## License
MIT


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FGosha%2Fmetalsmith-project-images.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FGosha%2Fmetalsmith-project-images?ref=badge_large)