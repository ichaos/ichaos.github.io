---
title: how to use github pages
---

## How to use github pages?
### Templating using Markdown

We’re just about finished, but I’m going to cover one more thing, and that’s how to use the Markdown templating system.

Create a file named demo.markdown in your repo and add the following content:


		---
		title: This will be used as the title-tag of 		the page head
		---

		# This is a H1

		[the clickable text](http://xlson.com/)

		* Bullet lists are also easy to create
		* One more

Important: the top part containing the --- and title segment is required and if you skip it, no conversion from Markdown to HTML will happen.

Try pushing the new file to your gh-pages branch, or personal repository. GitHub will turn this template into an ordinary HTML file named demo.html using the conversions specified in the sample. Pages supports a few other templating engines (like Textile) but Markdown is the only one I use.

There’s also support for the static blog generator Jekyll (which I’ve used to create this blog). Getting started with Jekyll is out of the scope of this article, but if you’ve gotten this far, getting Jekyll running should be no problem. Check the “Getting Started” instructions over at Jekyllrb.com to get started.