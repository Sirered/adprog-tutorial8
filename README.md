# Tutorial 8 Reflections

## What is amqp?

Amqp stands for Advanced Messaging Queue Protocol. Message queues are integrated in some event-driven architectures to store messages being passed from message producers/senders who had just made a change or needs to communicate a change that needs to be made to receivers who will act on the information in the messages that they have received. These message queues will store the messages when the receivers are busy and will distribute the messages to the receivers according to the order that the messages were received in. Amqp specifically is a message queue protocol that is used by RabbitMQ, due to it's ability to reliably deliver and distribute messages regardless of the scale and can do so asynchronously. In the code, amqp is within the URI amqp://guest:guest@localhost:5672, which signifies that the listener on our subscriber code will use the amqp protocol to service messages/requests. Thus, that means that requests will be enqueued in a message queue temporarily if our subscriber is still processing other requests and then will be dequeued and distributed using amqp. 

## what it means? guest:guest@localhost:5672 , what is the first quest, and what is the second guest, and what is localhost:5672 is for?

guest:guest@localhost:5672 is the URI that represents the connection details for a RabbitMQ server that the subscriber application will connect to. This RabbitMQ server will act as a message queue that will be used to collect, handle and distribute messages to the subscriber.The URI contains information regarding authentication and address of the server

* the first 'guest' is the username ued to authenticate to the server
* the second 'guest' is the password for the 'guest' user of the server
* 'localhost' is the hostname where the server is on. We use localhost, because the messages are being passed between programs on the same device.
* '5672' is the port that the server is listening on to get requests. This is specified so that if there are multiple programs that are receiving messages running on the same device/host, then the RabbitMQ server will know to only collect messages that are being sent to port 5672, and it will guarantee no other programs will be getting the messages that is supposed to be only for RabbitMQ
