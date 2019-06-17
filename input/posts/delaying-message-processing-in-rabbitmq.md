Title: Delaying Message Processing in RabbitMQ
Published: 06/17/2019
Tags:
   - C#
   - .NET
   - Programming
   - RabbitMQ
---

If you search for *"how to delay message processing in RabbitMQ"*, you'll most likely run into two possible solutions for it.
- One solution is to make use of the [message TTL argument](https://www.rabbitmq.com/ttl.html#per-message-ttl) with additional queues to route messages through. *If I understood this approach correctly, you basically route your message to Queue A, where it will sit for some time before it expires and gets moved to another queue, say Queue B. Then you will have your consumer looking for messages at Queue B.*
- The second solution is to use the official [RabbitMQ Delayed Message Plugin](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange).

Both solutions presented above are valid solutions, but I ended up not implementing any of those solutions, and instead went with a solution that is configurable via the consumer application. First, my reasons for not going with the established solutions listed above.
- I did not want to add any more queues or exchanges, especially if their purpose is to just move messages around.
- The [RabbitMQ Delayed Message Plugin](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange) as of this writing is still listed as an *"experimental yet fairly stable"* plugin. The *"experimental"* disclaimer is a matter of concern to me and I would prefer to wait until it matures enough that it is no longer called as such.
- Lastly, I really wanted a solution that is configurable via the consumer application. 

So, the solution I went with was to add a PublishDate via the message headers and then the consumer can delay message processing based on this date value. 

Adding a PublishDate header value is easy, you add it to the Properties.Headers dictionary before publishing the message.
```
var properties = channel.CreateBasicProperties();
properties.Persistent = true;

properties.Headers = new Dictionary<string, object>();
properties.Headers.Add("PublishDate", DateTime.Now.ToString());

channel.BasicPublish(exchange: "",
    routingKey: "task_queue",
    basicProperties: properties,
    body: body);
```
*Note that I'm adding the PublishDate value as a string, instead of a DateTime value. For some reason, adding it to the dictionary as a DateTime value causes an error. I don't remember what the error was, something about an invalid table value, so I just went with a string value.*

On the consumer side, you will need to add code to retrieve the Publish Date from the headers. 
```
consumer.Received += (model, ea) =>
{
    byte[] publishDateHeader = (byte[])ea.BasicProperties.Headers["PublishDate"];
    DateTime publishDate = Convert.ToDateTime(Encoding.UTF8.GetString(publishDateHeader));
    // Now you can delay message processing based on the publish date value

    var body = ea.Body;
    var message = Encoding.UTF8.GetString(body);
    Console.WriteLine(" [x] Received {0}", message);

    channel.BasicAck(deliveryTag: ea.DeliveryTag, multiple: false);
};
```
*Note that I'm first casting the header value to a byte array, before converting it to a string, then finally to a DateTime value. For some reason, adding a string as a custom header turns it into a byte array. Thankfully somebody else ran into this issue before and [shared a solution for it](http://gigi.nullneuron.net/gigilabs/rabbitmq-string-headers-received-as-byte-arrays/).*

With a PublishDate value available, you can now delay message processing however you would like. In my case, I opted to compare the PublishDate value to the DateTime.Now value, which allowed me to check how old the message was. For example, if a message was 5 minutes old, it has been delayed enough and gets processed right away. If the message was only a minute old, the consumer thread will wait until such time that the message was now 5 minutes old, before it processes it.

There are some drawbacks to this approach, namely, you will have to go through the Publisher/Consumer classes to add the code for handling a PublishDate header value. Depending on how your queues are structured and how many publisher-consumer class files you have, you could end up with changes to multiple files just to add this feature. On the flip side though, if only one queue needs this "delayed message processing" feature, then you'll have minimal changes while your other queues continue as is. There are probably more pros and cons to this approach that I haven't thought of. Still I prefer the flexibility with this approach as I only must worry about editing a consumer's config file and it allows me to run multiple consumers each with their own specific message processing setting.

Have you had to design a solution to delay message processing in RabbitMQ? If so, I am curious to hear what approach you went with and why. Please do share in the comments below or send me an email and we can discuss.