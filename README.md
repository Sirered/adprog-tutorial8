# Tutorial 8 Reflections

## What is amqp?

Amqp stands for Advanced Messaging Queue Protocol. Message queues are integrated in some event-driven architectures to store messages being passed from message producers/senders who had just made a change or needs to communicate a change that needs to be made to receivers who will act on the information in the messages that they have received. These message queues will store the messages when the receivers are busy and will distribute the messages to the receivers according to the order that the messages were received in. Amqp specifically is a message queue protocol that is used by RabbitMQ, due to it's ability to reliably deliver and distribute messages regardless of the scale and can do so asynchronously. In the code, amqp is within the URI amqp://guest:guest@localhost:5672, which signifies that the listener on our subscriber code will use the amqp protocol to service messages/requests. Thus, that means that requests will be enqueued in a message queue temporarily if our subscriber is still processing other requests and then will be dequeued and distributed using amqp. 

## what it means? guest:guest@localhost:5672 , what is the first quest, and what is the second guest, and what is localhost:5672 is for?

guest:guest@localhost:5672 is the URI that represents the connection details for a RabbitMQ server that the subscriber application will connect to. This RabbitMQ server will act as a message queue that will be used to collect, handle and distribute messages to the subscriber.The URI contains information regarding authentication and address of the server

* the first 'guest' is the username ued to authenticate to the server
* the second 'guest' is the password for the 'guest' user of the server
* 'localhost' is the hostname where the server is on. We use localhost, because the messages are being passed between programs on the same device.
* '5672' is the port that the server is listening on to get requests. This is specified so that if there are multiple programs that are receiving messages running on the same device/host, then the RabbitMQ server will know to only collect messages that are being sent to port 5672, and it will guarantee no other programs will be getting the messages that is supposed to be only for RabbitMQ

## Simulation slow subscriber

**I had run the publisher 5 times. The first screenshot is to show the Total amount of messages in queue that was recorded after I run the publishers(with 21 Total messages) and the second screenshot is to show the shape of the queue graph, after the entire queue was processed**

![image](https://github.com/Sirered/adprog-tutorial8-publisher/assets/126568984/e5098060-5327-4fc6-a7e0-856aa070681f)
![image](https://github.com/Sirered/adprog-tutorial8-publisher/assets/126568984/6932a062-ee55-49e5-bbba-b95553dfa26b)

**Why did it show 21 total messages were in queue at a time**

When we run the publisher 5 times in quick succession, we would be sending a total of 25 messages at once. Since it takes a second for our application to process each, most of those messages will have to be enqueued into the message queue before they can be sent to the subscriber (otherwise they would not be processed and just be dropped). Thus a lot of them would have to wait for their turn in the queue, while subscriber handles the messages that came before them, thus leading to the spike of messages we saw in the message queue grah. As to why it states 21 specifically, despite the fact that there should be 25 messages is probably because in the time that it took for the RabbitMQ Server UI to update (which it does so quite slowly), the subscriber had handled 3 of the messages and is on their fourth, thus leaving only 21 messages unprocessed and in queue. After peaking at 21, the total amount of messages went to 16 then 11 then 5 then 0 (I am not 100% sure that the number after 11 was 5, I just remember it was either 4, 5 or 6), further proving that the RabbitMQ UI doesn't update in real time, and in the time between updates more than 1 message could be distributed to subscriber and processed. 
