Title: Adding an Archive Page to Jekyll Now Blog
Published: 12/17/2018
Tags:
   - Jekyll
   - Liquid
   - Blog
---
As I alluded to in my [previous post](https://dinobansigan.com/Blog-With-JekyllNow-And-GitHub-Pages/), part of the limitations with 
creating a blog using the [Jekyll Now](https://github.com/barryclark/jekyll-now) repository, is that it didn't come with Archive and 
Tags support out of the box. Considering how easy it was to get a blog up and running with Jekyll Now, we shouldn't hold this 
against it or its creator/contributors. Instead, we should take this as an opportunity to learn more about [Jekyll](https://jekyllrb.com/) 
and start writing some code. So here is a quick guide on how you can add an Archive page to your Jekyll Now blog.

## Step 1: Add a new archive.html file inside the _layouts folder in your repository.

The content of this file will be a mixture of HTML and Liquid. [Liquid](https://help.shopify.com/en/themes/liquid) is a template 
language that you can use with Jekyll to help control the pages that are generated for your static site. Think of it as some programming 
code that gets processed when your static site is generated. 

*I actually struggled with this for awhile as I could not reconcile how a static site had, what looked like server side code in its pages. 
What I didn't realize then was that the Liquid code is only used to generate a static page/site. From a .NET developer's perspective, it 
is almost like Razor, except it is only used to generate a static site/page. It is not going to run as server side code.*

The content of the archive.html file will be as follows:
```
---
layout: default
---

<article class="page">
  <h1>{{ page.title }}</h1>
  <div class="entry">
    
    {% assign previousYear = "" %}
    {% for post in site.posts %}
      {% capture currentYear %}
        {{ post.date | date: "%Y" }}
      {% endcapture %}
    
      {% if currentYear != previousYear %}
        {% assign previousYear = currentYear %}
        <h3>{{ currentYear }}</h3>
      {% endif %}
    
      {{ post.date | date: '%B %d, %Y' }} - <a style="font-weight: bold" href="{{ post.url }}">{{ post.title }}</a>
      <br />
    {% endfor %}    
  </div>
</article>
```
All I'm trying to do here is get all the blog posts on this site and list them down by year. *Note that I am relying on the fact that 
the list of blog posts returned by the [Jekyll Site Variable](https://jekyllrb.com/docs/variables/#site-variables) **site.posts** 
remains in reverse chronolical order. For now, that seems to be the case and I don't have to add Liquid code to correctly sort the entries.*

If you're wondering what the line of code **"| date: '%B %d, %Y'"** does, that is what is called a [Liquid filter](https://help.shopify.com/en/themes/liquid/filters) 
and that simply formats the date value to a format I specify. This is similar to calling the **ToString()** method on a DateTime object 
in C# and passing in a custom datetime format string.

## Step 2: Add a link to the Archive page by editing the default.html file.

The default.html file is also found in the _layouts folder in your repository. The link we want to add is this:
```
<a href="{{ site.baseurl }}/archive">Archive</a>
```

To find where to insert that line, just find the block of code in the default.html file, where it has the links to the About and Blog 
page. The code below is an example of what you are looking for.
```
  <nav>
	<a href="{{ site.baseurl }}/about">About</a>
	<a href="{{ site.baseurl }}/">Blog</a>
	<a href="{{ site.baseurl }}/archive">Archive</a>
	<a href="{{ site.baseurl }}/tags">Tags</a>
  </nav>
```
As you can see in the code above, I have added the link to the Archive page after the link to the Blog. *This is also the code to modify 
if you wanted to change the order that the links show up on your website.*

## Step 3: At the root of your repository, add an archive.md file.

The content of the archive.md file will be as follows:
```
---
layout: archive
title: Archive
permalink: /archive/
---
```
Adding a .md (Markdown file) at the root of your repository will force Jekyll to generate a html page for that .md file. *This is also 
how Jekyll decides to create the About page, because there is an about.md file in the root of the repository.*

Some notes on the content of the archive.md file:
- The layout entry simply tells Jekyll to reference the archive.html file that you added to the _layouts folder. So basically when this 
Archive page is generated, it will use the layout/html that you have specified in the archive.html file. 
- The Title entry is simply the Title text that will show up on the Archive page.
- The Permalink entry is used to ensure that the generated Archive page shows up on "yourdomainname.com/archive/". You can also supposedly 
set this in the configuration file, but I just chose to do it here.

There you have it, that wasn't too hard I think. For reference, you can view the source code for this website on [Github](https://github.com/DinoBansigan/dinobansigan.github.io) 
to see how I implemented it.