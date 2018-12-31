Title: Adding Time To Read to Wyam Blog
Published: 12/30/2018
Tags:
   - Wyam
   - Blog
---
Adding a time to read estimate was something I've always wanted to do when I created my blog. I like the idea of giving readers an idea of how much time they would spend/lose if they read my posts. This is so they can make better use of their time. If they have enough time, they can read the post, if not, they can bookmark it or add to their reading list.

### A note on customizing a Wyam blog

One of the confusing things for me when I first started using Wyam to create my blog is how to customize it. After running the command 'wyam new -r Blog', you will end up with an "about.md" markdown file, an input folder with a "first-post.md" markdown file in it and that's it. Well there is a "config.wyam" file but that's not what you use to customize the whole site. There are no .html, .cshtml or .css files that you can modify to customize your site. Turns out to customize the site, you will have to add new files to the input folder and add your modifications to those files. The next time Wyam builds your site, Wyam will pick up those files and process them. 

What confused me was the use of the word "theme". When I read theme, I think about the design of the site, mainly modifying the CSS files to customize how it looks. For Wyam though, theme means everything about how the site looks, which includes the design and layout of the content on the site. So say if you wanted to modify how the footer looks, like add content on it, you would have to create a "_Footer.cshtml" file in the input folder and add your customizations to that file. The Wyam documentation actually talks about that [here](https://wyam.io/docs/concepts/themes#overriding-theme-files) and on the Overriding Theme Files section [here](https://wyam.io/recipes/blog/themes/cleanblog). 

Now that we've got that out of the way. Here are the steps I took for adding a time to read estimate for posts on this blog.

## Step 1: Add a _PostHeader.cshtml to the input folder

I wanted to add the time to read estimate on the top of a blog post, around where the Title and Published date shows up. So the file to modify is the _PostHeader.cshtml file. To do this, I created an empty _PostHeader.cshtml in the input folder and copied the contents from this [file](https://github.com/Wyamio/Wyam/blob/develop/themes/Blog/CleanBlog/_PostHeader.cshtml). *Note that the file is from the CleanBlog theme, which is the theme I'm using for this blog.* 

At this point you can do a Wyam build to make sure it works and that nothing is broken. Your site will not look any different because we have not made any modifications, we basically just copied the default code for the _PostHeader.cshtml file. *When you don't have this file in your input folder, Wyam will actually build and process this file behind the scenes when building your site. When you have the same file in the input folder, Wyam knows to take that file and its content as the main source for processing the _PostHeader.cshtml file.*

## Step 2: Add code to calculate "time to read" in the _PostHeader.cshtml file

To calculate the time to read value for a blog post, we can use [Razor](https://docs.microsoft.com/en-us/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c), which is one of the reasons why I like using Wyam. At the top of the file I added the following lines of code:

```
char[] delimiters = new char[] {' ', '\r', '\n' };
var numberOfWords = Document.Content.Split(delimiters, StringSplitOptions.RemoveEmptyEntries).Length;
var TimeToRead = Math.Ceiling(numberOfWords / 200f).ToString();
```
What we're doing here is getting the content of the current blog post, then splitting it into an array to get how many words are in the blog post. I actually got that code from a [StackOverflow answer](https://stackoverflow.com/a/8784662/5041911). Then we divide the number of words by 200 to get a conservative time to read estimate value in minutes, which is then converted to a string.

To display the time to read estimate, I modified the line of code that displays the date the post was published and I ended up with this:

```
<div class="meta">
    @if (Published != default(DateTime))
    {
        <text>@Published.ToLongDateString(Context) &rarr; @TimeToRead min read</text>
    }
</div>
```
I basically just appended an arrow that points to the time to read estimate, on the same line that shows date the post was published.

At this point, you can do a Wyam build and preview your site. All your blog posts should now have a time to read estimate at the top, just below the title. 

To view the whole content of the _PostHeader.cshtml site for this blog, you can check [here](https://github.com/DinoBansigan/dinobansigan/blob/master/input/_PostHeader.cshtml).