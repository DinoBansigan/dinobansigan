Title: Lowercase Letters Only with Azure Storage Containers
Published: 02/20/2019
Tags: 
   - Azure
   - Programming
---
As the title states, you cannot use UPPERCASE letters when naming Containers in Azure Storage. I have no problem with this. My issue is that the exception message that is returned when running into this restriction, does not help me figure out what I did wrong.

This is the exception message I got:
> The remote server returned an error: (400) Bad Request.

Yeah, no help at all. Thankfully since I was writing the code, I could see where it was running into the exception. So I know it was an issue when trying to create a Container. It would have been a much bigger headache had I been trying to support a library that didn't have good exception logging or show the correct stacktrace.

I can somewhat understand why they did it this way though; it's most likely for security reasons that they are returning a very vague error. Either way, you have to know that there are a bunch of naming rules concerning how to name things in Azure Storage. For the official list from Microsoft, you can head over [here](https://docs.microsoft.com/en-us/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).