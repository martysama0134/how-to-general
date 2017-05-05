# Doxygen Code with syntax highlighting

_Based on Doxygen 1.8.13 and HighLightJS 9.11.0_

To make the `@snippet` code be syntax highlighted by highlight.js, there are few steps to do:

1. Generate the default .html templates by doing:

	```sh
	doxygen.exe -w html header.html footer.html customdoxygen.css
	```

2. Download highlight.js from [here](https://highlightjs.org/download/) specifying which languages you need.

	Inside the zip you will find highlight.pack.js and a list of styles (I'll use style/github.css in this case)

3. Edit header.html adding this before `</head>`:

	```html
	<!--HIGHLIGHT.JS INSTALL BEGIN-->
	<link rel="stylesheet" href="$relpath^github.css">
	<script src="$relpath^highlight.pack.js"></script>
	<script>
	$(function() {
		hljs.configure({useBR: false});
		$(".fragment").each(function(i,node) {
			$(node).removeClass("fragment");
			hljs.highlightBlock(node);
		});
	});
	</script>
	<!--HIGHLIGHT.JS INSTALL END-->
	```

4. Edit Doxygen by adding:

	```sh
	HTML_HEADER            = header.html
	HTML_EXTRA_FILES       = highlight.pack.js github.css
	```

5. Be sure to have all the files checked and it's ready to be compiled.


Result:

1. How you setup a `@snippet` tag code in source:

	[![Result Label](http://i.imgur.com/SJfXFee.png)](http://i.imgur.com/SJfXFee.png)

1. How you setup the real snippet code:

	[![Result Label](http://i.imgur.com/vhKkzzC.png)](http://i.imgur.com/vhKkzzC.png)

1. How it will appear in doxygen: (github.css style)

	[![Result Label](http://i.imgur.com/gCFItXm.png)](http://i.imgur.com/gCFItXm.png)


Notes:

1. The inline comments in lua are a little buggy, so I had to use the in-section comments `--[[...]]` instead.
1. Sometimes, I had to indent the whole code with an extra initial tab/space character to show the initial tags well colored
1. The CDN-hosted of highlightjs packages are here:

	1. [highlight.min.js](//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.11.0/highlight.min.js)
	1. [lua.min.js](//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.11.0/languages/lua.min.js)

