Title: Checking Custom Domain Name IP Address in Windows
Published: 12/08/2018
Tags: 
  - Note To Self
  - Ping
  - Windows
---
I finally have my own custom domain and this site is now on "dinobansigan.com" as opposed to "dinobansigan.github.io". 
I initially had trouble getting the site up and running on my custom domain. I [learned](https://help.github.com/articles/setting-up-an-apex-domain/) that you need to add A records to make a custom 
domain work with Github pages. So I did that but I still couldn't get to the site. 

Did a search on bing (*yes I use Bing*) and the top troubleshooting tip was to verify which IP address my site was resolving to. 
To do that, I would have to run the Dig command on my domain name. The Dig command is a linux command and I'm on a Windows PC so that won't work.
Turns out you can use the good old Ping command to do the same thing. So I fired up the command prompt and typed "ping yourdomainname.com". 
I was then able to see what IP address my site was resolving to.

*Note: It took awhile for my site to work with my custom domain even after I correctly setup the A records. 
I was starting to think I did it incorrectly. So just wait 10-20 minutes and try again.*