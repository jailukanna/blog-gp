---
title: socket example client serv (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'client serv'

Functions in program: 
* `def run_client(server_name: str):`
* `def run_server():`
* `def read_stdin(message, custom_socket, addresses, from_client=False):`
* `def read_server_sockets(sock, server_socket, client_addresses):`

Modules used in program: 
* `import sys`
* `import select`
* `import socket`
* `import argparse`

## python client serv

Python socket example: client serv

```python
"""
This module implement a UDP group chat without threads
"""
import argparse
import socket
import select
import sys


def read_server_sockets(sock, server_socket, client_addresses):
    """
    This function read incoming data from server sockets. If data arrive on port 12345, we add the client to our
    client cache and send the data to the other client except the one who send the data.
    :param: sock: One of the socket of the server.
    :param: server_socket: The attended socket server.
    :param: client_addresses: Cache of our clients
    """
    if sock != server_socket:
        return
    response, address = server_socket.recvfrom(255)
    if address not in client_addresses:
        client_addresses.append(address)
    if not response:
        return
    decoded_response = response.decode()
    for client_address in client_addresses:
        if " by " in decoded_response:
            print("{} by ({}, {})".format(decoded_response.split(" by ")[0], address[0], address[1]))
        else:
            print("{}".format(decoded_response))
        if client_address == address:
            continue
        server_socket.sendto(response, client_address)


def read_stdin(message, custom_socket, addresses, from_client=False):
    """
    This function check if there is any message sent by the server.
    :param: message: The message received by stdin
    :param: custom_socket: The socket we used to send data
    :param: addresses: the list of address
    :param: from_client: True if it comes from client
    """
    if not message:
        return
    message = sys.stdin.readline().strip()
    for address in addresses:
        if from_client:
            message = "{} by ({}, {})".format(message, address[0], address[1])
        custom_socket.sendto(message.encode(), address)


def run_server():
    """
    This function start the process of running server
    """
    print('Start running server')
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    try:
        server_socket.bind(("", 12345))
    except OSError:
        print("You are not allowed to spawn multiple servers on the same machine")
        sys.exit()

    client_addresses = []
    while True:
        read_sockets, _, _ = select.select([server_socket], [], [], 0.5)
        for sock in read_sockets:
            read_server_sockets(sock, server_socket, client_addresses)

        message, _, _ = select.select([sys.stdin], [], [], 2)
        read_stdin(message, server_socket, client_addresses)

    server_socket.close()


def run_client(server_name: str):
    """
    This function start the process of running client.
    :param: server_name: The FQDN or IP address server.
    """
    print('Start running client')
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    # This first send is used to tag the client in the server client's list
    client_socket.sendto("".encode(), (server_name, 12345))

    while True:
        read_sockets, _, _ = select.select([client_socket], [], [], 0.5)
        for sock in read_sockets:
            if sock == client_socket:
                response, address = client_socket.recvfrom(255)
                if not response:
                    continue
                print("{} ({}, {})".format(response.decode(), address[0], address[1]))
        message, _, _ = select.select([sys.stdin], [], [], 2)
        read_stdin(message, client_socket, [(server_name, 12345)], True)

    client_socket.close()


if __name__ == '__main__':
    PARSER = argparse.ArgumentParser(description='Create a client or server for a group chat depending of the number \
                                                  of arguments in the cli')

    PARSER.add_argument('server', type=str, nargs='*', help='IP address or FQDN server')
    ARGS = PARSER.parse_args()

    if not ARGS.server:
        run_server()
    else:
        run_client(ARGS.server[0])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
