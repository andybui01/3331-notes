### Lab 2 pseudocode

```python
get host from args
get port from args
address = "host:port"

socket = new socket
# when creating the socket, AF_INET is IPv4, and SOCK_DGRAM is UDP
# for TCP sockets use SOCK_STREAM

set timeout on socket to 600ms

for i=1 to 15:

    try:
        startTime = get time

        # send message to server at address
        socket.send(message, address)

        # receive packet from server
        socket.receive()

        endTime = get time

        rtt = endTime - startTime

    except timeout:
        rtt = "time out"
    
    print("ping to " + address + "seq = " + i + "rtt = " + rtt)

# print min, max, avg rtt

```