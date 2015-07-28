# Rinco Static Generator

[![Build Status](https://travis-ci.org/rincojs/rinco-staticgen.svg?branch=master)](https://travis-ci.org/rincojs/rinco-staticgen)
[![npm version](https://badge.fury.io/js/rinco.svg)](http://badge.fury.io/js/rinco)



![Rinco](https://avatars1.githubusercontent.com/u/7665633?v=3&s=300)

### We are in BETA version yet. Feel free to contribute.


# Suport


<img src="http://agehost.com.br/allanesquina/rinco/coffee.png" alt="coffeescript">
<img src="http://agehost.com.br/allanesquina/rinco/ts.png" alt="typescript">
<img src="http://agehost.com.br/allanesquina/rinco/sass.png" alt="sass">
<img src="http://agehost.com.br/allanesquina/rinco/less.png" alt="less">
<img src="http://agehost.com.br/allanesquina/rinco/stylus.png" alt="stylus">
<img src="http://agehost.com.br/allanesquina/rinco/md.png" alt="markdown">
<img src="http://agehost.com.br/allanesquina/rinco/mustache.png" alt="mustache">


# Install

        npm install rinco -g

# Usage

### Create a new project

        $ rinco
### Templates
Rinco supports templates, but for now, we have only one (blank template).

### Running the development server

        // within of your project's folder
        $ rinco server

### Structure convention

Rinco has a simple path convention that you need to follow:

    - Rinco Project  -> path of your project
    --- assets       -> assets path
    ------ css       -> stylus, sass, less or pure css
    ------ data      -> json to be imported
    ------ img       -> images
    ------ includes  -> (.html|.md) partials to be imported (you can use mustache as template language)
    ------ js        -> coffescript or pure js
    ------ pages     -> main pages of your project (you can use mustache as template language)
    --- public       -> used to development server
    ------ css          ||
    ------ js           ||  
    ------ img          ||
    --- build       -> static files (that was generated by rinco build task)

### Syntax

Rinco has some syntax helpers to improve your development:

- <code>@include()</code>

```
@include(file.html)      // will include the file.html from (assets/includes) folder
@include(path/file.html) // will include the file.html from (assets/includes/path) folder
```

- <code>@data()</code>

```
@data(file.json)      // will include the file.html from (assets/data) folder
@data(path/file.json) // will include the file.html from (assets/data/path) folder
```

You can create a alias for an imported file and use it in your template:
```
@data(file.json as myalias)      // will include the file.html from (assets/data) folder
```

```html
...
	@data(en-en.json as data)
	<h1>{{data.title}}</h1>
...
```

### Real example

- index.html (refers to file <code>assets/pages</code>)

```html
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>{{user.title}}</title>
	<link rel="stylesheet" href="/css/styles.css">
</head>
<body>
	<section>
		<!-- data usage -->
		@data(user.json)
		@data(generic.json as menu)

		<!-- include usage -->
		@include(header.html)
		@include(content.html)
		@include(footer.html)
	</section>
	<script src="/js/app.js" charset="utf-8"></script>
</body>
</html>
```
- header.html (refers to file <code>assets/includes/header.html</code>)


```html
<header>
	<h1>{{user.name}}</h1>
	<nav>
		{{#each menu.items}}
		  <a href="{{link}}">{{name}}</a></h2>  
		{{/each}}
	</nav>
</header>
```

- user.json (refers to file <code>assets/data/user.json</code>)

```json
{
	"name": "Rinco JS",
	"title": "Rinco JS",
	"github": "https://github.com/allanesquina/rincojs"
}
```

- generic.json (refers to file <code>assets/data/generic.json</code>)

```json
{
	"items": [
		{
			"link": "/home",
			"name": "home"
		},
		{
			"link": "/doc",
			"name": "documentation"
		},
		{
			"link": "/download",
			"name": "download"
		}
	]
}

```

### CSS

**Rinco** supports many CSS extension languages like sass, less and stylus. To use it, just change the extension to your prefered language and **Rinco** compile it to you. Don't worry about the choice, you can use all together.
To link a css file use the css filename changing the extention to <code>.css</code>.

```html
<!-- refers to file assets/css/styles.sass -->
<link rel="stylesheet" href="/css/styles.css">
<!-- refers to file assets/css/colors.less -->
<link rel="stylesheet" href="/css/colors.css">
<!-- refers to file assets/css/custom.styl -->
<link rel="stylesheet" href="/css/custom.css">
```

**Rinco** will create the file in <code>public/css</code> folder.


### Javascript

**Rinco** allows you to code in **coffeescript** or **typescript** language, it's similar of the CSS compile behavior, so you just need to change the file extension to <code>.coffee</code> or <code>.ts</code>. To link it on page, change the extension to <code>.js</code>.

```html
<!-- refers to file assets/js/app.coffee -->
<script src="/js/app.js" charset="utf-8"></script>
<!-- refers to file assets/js/test.ts -->
<script src="/js/test.js" charset="utf-8"></script>
```

**Rinco** will create the file in <code>public/js</code> folder

### Ignoring files
You can ignore a file putting <code>_</code> at the beginning of the filename like this:
```
_variables.sass
```
This is helpful to CSS imported files.

### Generate static files

To generate the static files use the build task. The static files will be in **build** folder.

```
$ rinco build
```
