# pingoo-server
A lightweight web service to test the network bandwidth and latency of a mobile client. 

# Technical specification

Pingoo server uses a Django backend that can be used to record network metrics from a users device, regardless of the client being used(mobile or borwser). The web backed uses token based authentication to interact with the client. It can be used to do the following network tests

## Throughput Test

The throughput test is used to check a client's cpacity to transfer and recieve data for a certain interval of time. This srevice runs in the backgroud and record the rate at which packets of specified size can move in and out of a user's mobile.

### Model

**Throughput Test:** A singe session of a Throughput Test that can have one or more Throughput Packets associated to it, this contains the details of number of packets that are to be sent in that test session and the size of each packet.

**Test Packet:** A single packet whose foriegn key is Throughput Test ID in the database, it contains time(ms) at which the packet was sent and recieved from the place of origin(pingoo server or client).

### Workflow
When a session starts, pingoo server creates a new Throughput Test, with the specified number of packets and the size of each packet. The test is marked as incomplete on the time of creation. Once the client and server start exchanging packets, time of each packet is noted. After n th packet is recieved the Test is marked as complete.

## Ping Test

Ping test works similar to ping command in linux, the difference being pingoo servicer is used to get round trip time of a packet from client to the HTTP API(application layer). 

### Workflow

Time of a packet is noted at the time it originates from a client, and then again when it reaches the Pingoo server. However a success respose is also sent back to a client communicating with a REST API, to get a more precise RTT, Pingoo server also records the time at which a client received 200 status or success respose from the server.

## JavaScript SDK

The client can use the javascript [SDK](https://github.com/amnox/pingoo/blob/master/tester/src/pingoo/PingSDK.js) directly to carryout the intricate exchanges between the Pingoo server and the mobile app. After getting the authentication token from the server, client can use the PingSDK directly.

The methods in the class are asynchronous and can run in the background without affecting application's performance.

Pingoo server was developed during my time as a research assistant at Saint Louis University. To get access to the code kindly drop me a mail at aman dot khalid [at] slu dot edu
