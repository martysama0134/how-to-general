It is already setup for highlightjs as well, so remove the lines about `github.css` and `zenburn_mod.css` if you don't need it.

The only problem about this is that you will still get a white 0.1s refresh when switching pages in night mode.

Doxygen:

```
HTML_HEADER            = style/header.html
#highlightjs
HTML_EXTRA_FILES       = style/highlight.min.js style/lua.min.js
HTML_EXTRA_STYLESHEET  += style/github.css
HTML_EXTRA_FILES       += style/zenburn_mod.css
#daynight
HTML_EXTRA_STYLESHEET  += style/doxygen_light.css
HTML_EXTRA_FILES       += style/doxygen_night.css
```

header.html before `</head>`:

```html
<!--DAY/NIGHT INSTALL BEGIN-->
<script>
function daynight_check() {
	if (typeof(Storage) !== "undefined") {
		if (localStorage.getItem("bgColor")=="night") {
			night();
		}
		/*else if (localStorage.getItem("bgColor")=="day") {
			day();
		}*/
	}
};
function changeCSS(cssFile, cssLinkIndex) {

	var oldlink = document.getElementsByTagName("link").item(cssLinkIndex);

	var newlink = document.createElement("link");
	newlink.setAttribute("rel", "stylesheet");
	newlink.setAttribute("type", "text/css");
	newlink.setAttribute("href", cssFile);

	document.getElementsByTagName("head").item(0).replaceChild(newlink, oldlink);
}
function changeCSS2(cssOLdFile, cssNewFile) {
	var oldlink = document.getElementsByTagName("link");
	for (i = 0; i < oldlink.length; i++) {
		if (oldlink.item(i).getAttribute("href")==cssOLdFile) {
			changeCSS(cssNewFile, i)
		}
	}
}
function day() {
	changeCSS2("doxygen_night.css", "doxygen_light.css");
	changeCSS2("zenburn_mod.css", "github.css");
	if (typeof(Storage) !== "undefined") {
		localStorage.setItem("bgColor", "day");
	}
};
function night() {
	changeCSS2("doxygen_light.css", "doxygen_night.css");
	changeCSS2("github.css", "zenburn_mod.css");
	if (typeof(Storage) !== "undefined") {
		localStorage.setItem("bgColor", "night");
	}
};
document.addEventListener('DOMContentLoaded', function() {
	daynight_check();
}, false);
</script>
<!--DAY/NIGHT INSTALL END-->
```

header.html after `<body>`:

```html
<!--DAY/NIGHT INSTALL BEGIN-->
	<input type="button" class="daynightbtn" onclick="day();" value="Day" />
	<input type="button" class="daynightbtn" onclick="night();" value="Night" />
<!--DAY/NIGHT INSTALL END-->
```
