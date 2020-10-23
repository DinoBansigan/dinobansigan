Title: Visual Studio Code Comparison View Modes
Published: 10/22/2020 04:27:00
Tags:
   - Note To Self
   - Visual Studio
---
For some reason, maybe because I accidentally pressed something on my keyboard, Visual Studio 2019 started comparing code in inline mode. By inline mode, I mean I only got one code comparison window, and the changes were stacked on top of each other. This made it hard for me since I was so used to the side by side code comparison mode, where I had two windows and I can clearly see what's different between files. 

It took awhile for me to figure out how to set it back. I spent some time looking through the options in Visual Studio but couldn't figure it out. I even had a hard time searching the internet for answers, because I wasn't sure what to search for. The search key that finally worked for me was *"vs 2019 diff inline mode"*. That led me to the Stack Overflow post I linked below.

*Sidenote: I find it hilarious that the question in Stack Overflow was for Visual Studio 2012. Here I am with Visual Studio 2019 and the answer still works. Amazing. And what a gem Stack Overflow is for software developers.*

Anyway, there are two ways to set it back to side-by-side mode. You can use the shortcut button for it in the menu bar. Or, you can use the shortcut `CTRL + \`, `CTRL + 2`.

![Visual Studio Diff View Modes Shortcut](/assets/images/vs-diff-view-modes.png "Visual Studio Diff View Modes Shortcut")

### Reference:
[https://stackoverflow.com/questions/20067385/how-to-switch-view-modes-in-built-in-diff-viewer-of-visual-studio-2012-and-2013](https://stackoverflow.com/questions/20067385/how-to-switch-view-modes-in-built-in-diff-viewer-of-visual-studio-2012-and-2013)