---
title: Python
weight: 3
---

## Pyzmq

<table>
<tr><td>Github</td><td>https://github.com/zeromq/pyzmq</td></tr>
<tr><td>Docs</td><td>https://zeromq.github.io/pyzmq/</td></tr>
<tr><td>Guide</td><td>http://zguide.zeromq.org/py:all</td></tr>
<tr><td>pypi</td><td>https://pypi.org/project/pyzmq/</td></tr>
<table>

### Download

```bash
pip install pyzmq
```

### Example

Server:
```python
#
#   Hello World server in Python
#   Binds REP socket to tcp://*:5555
#   Expects b"Hello" from client, replies with b"World"
#

import time
import zmq

context = zmq.Context()
socket = context.socket(zmq.REP)
socket.bind("tcp://*:5555")

while True:
    #  Wait for next request from client
    message = socket.recv()
    print("Received request: %s" % message)

    #  Do some 'work'
    time.sleep(1)

    #  Send reply back to client
    socket.send(b"World")
```

Client:
```python
#
#   Hello World client in Python
#   Connects REQ socket to tcp://localhost:5555
#   Sends "Hello" to server, expects "World" back
#

import zmq

context = zmq.Context()

#  Socket to talk to server
print("Connecting to hello world server…")
socket = context.socket(zmq.REQ)
socket.connect("tcp://localhost:5555")

#  Do 10 requests, waiting each time for a response
for request in range(10):
    print("Sending request %s …" % request)
    socket.send(b"Hello")

    #  Get the reply.
    message = socket.recv()
    print("Received reply %s [ %s ]" % (request, message))
```