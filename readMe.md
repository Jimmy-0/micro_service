# microservice archi

## gateway service
- /auth/access.py/login(request): Take HTTP request from the client and validate the token 
- /upload: Validate the token, upload to mongoDB with the video_id. Once the upload is complete, it will send a message using RabbitMQ which means "the video is ready to be converted"
- /download: Download the mp3 file, 
- synchronous communciton: must get a response before sending a new request. Might takedown the performance. We must have the JWT to perform the following service.

## Auth 
- Since our internal services live within a private network. We need auth to determine whether we allow the request in from the open internet. If the username and password match the MySQL, we will return a JWT. The client will use JWT for the follow up operations.
- JWT
    - header: Contain information about the signing algorithm
    - payload: Contain claims or statements about the authenticated user or entity.
    - signature: Ensure the integrity and authenticity of the token.

## RabbitMQ
- In order to achieve asynchronous communciton, client can send the request whenever you want. Example, [Gateway] -[rq]->[converted_service]
- Use `pika` to interface with queue in rabbitmq to store message 
- Stateful state
    - persistent identifier that is maintained across any rescheduling
    - persisting the data within the queue
    - To mount the physical storage to the container instance 

## Convert service
- The convert service will consume the message from the rabbitMQ and use the video_id to start converting.
- After the converting is complete, it will update the mongoDB and send message to the rabbitMQ.


## Notification service






