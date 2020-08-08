Title: Build Error Reported But Error List Window Is Empty
Published: 08/07/2020
Tags:
   - Visual Studio
   - Tips
---
Was running into a build error in Visual Studio 2017. The Output window says 1 project failed, but the Error List window was empty. The only way I managed to figure out what project was failing, was to copy the contents of the Output window, paste it into Notepad++ and search for the word “FAILED”.

Anyway, I finally figured out that it was the SQL database project that was failing to build. And it turns out, it was getting an "Out of Memory Exception". Not sure if this specific error causes the Error List window to come up empty. In any case, at least now I have an idea of what to look for whenever this happens again.