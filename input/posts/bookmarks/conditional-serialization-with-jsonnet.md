Title: Conditionally Serializing Fields and Properties with Json.NET - Marius Schulz
Published: 01/24/2019
Category: Bookmarks
Tags:
   - JSON.Net
   - Programming
   - .NET
   - Serialization
   - Bookmarks
---
Recently I ran into an issue where I needed to exclude a property from getting serialized using Json.NET. The easy answer is to add a `[JsonIgnore]` attribute to the property. The problem with doing that is it will also ignore the same property during deserialization. So I needed a solution that allows me to ignore a property using serialization, but still set that property's value during deserialization. Thankfully I found a [blog post](https://mariusschulz.com/blog/conditionally-serializing-fields-and-properties-with-jsonnet) from 2013 that explains exactly how to do that. I would have wasted more hours searching for an answer had I not found this solution right away.

> <p>There's a little known feature of Json.NET that lets you determine at runtime whether or not to serialize a particular object member: On the object you're serializing, you have to define a public method named ShouldSerialize{MemberName} returning a boolean value.</p>- Marius Schulz

Visit Original Post: [Conditionally Serializing Fields and Properties with Json.NET](https://mariusschulz.com/blog/conditionally-serializing-fields-and-properties-with-jsonnet)

It was only after I found Marius' blog post that I then found the documentation talking about [conditional property serialization](https://www.newtonsoft.com/json/help/html/ConditionalProperties.htm) on the Newtonsoft website.

*This is one of the rare instances where I didn't find the answer in StackOverflow. It makes me grateful for the developers who are still cranking out blog posts and sharing solutions to problems on their personal blogs/websites.*