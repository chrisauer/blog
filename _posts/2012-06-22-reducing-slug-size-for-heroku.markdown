---
layout: post
title:  "Reducing Slug Size for Heroku"
date:   2012-06-22 11:42:50
categories: heroku
---
Whether you are receiving the "Heroku push rejected, slug too large" error message while deploying your Java project to Heroku or you are just trying to reduce the size of your slug for faster deploys, you can perform a little post-compile cleanup to reduce the slug size. Here is how …

Heroku uses Maven POM file to compile your project before deploying it. If you are creating a fairly large application to use on Heroku, you might have already run into an error message while trying to deploy your code.

{% highlight bash %}
> Push rejected, your compiled slug is 138.0MB (max is 100MB).
     See: http://devcenter.heroku.com/articles/slug-size !     Heroku push rejected, slug too large

{% endhighlight %}

Heroku is essentially using the entire target directory to compile the final slug before deploying to each running instance. Reduce the size of your compiled slug by removing any duplicate or unnecessary files from your target directory after the install phase. Add the following snippet to your pom.xml plugins element, the result will cause maven to perform a cleanup on the target directory before Heroku compiles your slug.

<script src="https://gist.github.com/1556079.js?file=gistfile1.xml"><!-- --></script>
