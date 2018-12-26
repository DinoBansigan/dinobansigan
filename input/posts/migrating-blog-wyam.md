Title: Migrating Blog to Wyam
Published: 12/25/2018
Tags:
   - Wyam
   - Blog
---
As I sat down to write the post on how I added a basic Tags page to my previously [Jekyll Now](https://github.com/barryclark/jekyll-now) powered blog/website, I thought about how I'm playing catch up as far as blogging related features go. I had to add an Archive page. I had to add a basic Tags page. The sitemap and feeds feature didn't seem to work and I haven't spent the time to figure those out yet. So before I write even more blog posts and in the interest of saving myself time, I've switched to using [Wyam](https://wyam.io/) to generate this blog/website.

I should have gone with [Wyam](https://wyam.io/) from the start, but I found the [Jekyll Now](https://github.com/barryclark/jekyll-now) repository last minute and thought it seemed too easy not to give it a try. So I gave it a try and yes, it is very easy to get a blog up and running without touching the command line at all. A lot of people have used it and are happy with their blogs. However I think it's time for me to get back to what I really intended to do in the first place, and that is building this static blog/website using [Wyam](https://wyam.io/).

Here are some reasons as to why I want to use [Wyam](https://wyam.io/) for this blog/website:
- It is a static site generator that is written using [.NET Core](https://docs.microsoft.com/en-us/dotnet/core/about). Being a .NET software developer myself, using it to generate my blog/website is extremely rewarding in some way. I've also wanted to play with .NET Core for awhile now, so this is just another excuse to try it out.
- It supports building pages/websites using Razor, which I think is awesome.
- The configuration file is written using C# code and I love C#.
- You end up with a fully featured Blog when using the [Blog recipe](https://wyam.io/recipes/blog/). By that I mean, you have a working Archive and Tags page, as well as automatically generated feeds.
- It is very easy to switch themes, even on an existing live site.

I must point out that creating/maintaining a blog using [Wyam](https://wyam.io/) is slightly more challenging than using the [Jekyll Now](https://github.com/barryclark/jekyll-now) approach. You will have to use a command prompt/terminal to build your site and getting it hosted is not as simple as just creating a repository in Github and changing some settings. I like the extra challenge though. And writing blog posts using text editors and building the site using a terminal, kinda makes me feel like I'm [blogging like a hacker](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html), except I'm using [.NET Core](https://docs.microsoft.com/en-us/dotnet/core/about) and [Wyam](https://wyam.io/) instead of [Jekyll](https://jekyllrb.com/).