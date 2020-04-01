---
title: socket example socket wrapper (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket wrapper'

Functions in program: 
* `def get_calling_code(origin_function):`
* `def wrap(origin_function):`
* `def args_to_dict(args):`
* `def args_to_dict_accessor(args):`

## python socket wrapper

Python socket example: socket wrapper

```python
def args_to_dict_accessor(args):
    code = ""
    for arg in args:
        code += f"content['{arg}'], "
    return code[:-2]


def args_to_dict(args):
    code = "{"
    for arg in args:
        code += f"'{arg}': {arg}, "
    code = code[:-2]
    code += "}"
    return code


def wrap(origin_function):
    left_bracket_index = origin_function.find("(")
    right_bracket_index = origin_function.find(")")

    args_list = origin_function[left_bracket_index +
                                1:right_bracket_index].split(", ")
    func_name = origin_function[:left_bracket_index].replace("def ",
                                                             '').strip()
    port = 8080
    print()

    code = "import json\n"
    code += "import threading\n"
    code += "from socket import *\n\n"

    code += f"""def {func_name}_thread_response(server_socket, content, addr):
    ret_value = {func_name}({args_to_dict_accessor(args_list)})\n"""
    code += """    result = {"return": ret_value}
    server_socket.sendto(bytes(json.dumps(result), 'utf-8'), addr)\n\n"""

    code += f"""def {func_name}_wrapper_server():
    server_socket = socket(AF_INET, SOCK_DGRAM)
    server_socket.bind(('', {port}))
    while True:
        data, addr = server_socket.recvfrom(65000)
        content = json.loads(data.decode('utf-8'))
        thread = threading.Thread(target={func_name}_thread_response, args=(server_socket, content, addr)).start()"""
    return code


def get_calling_code(origin_function):
    left_bracket_index = origin_function.find("(")
    right_bracket_index = origin_function.find(")")
    args_str = origin_function[left_bracket_index:right_bracket_index + 1]
    args_list = origin_function[left_bracket_index +
                                1:right_bracket_index].split(", ")
    func_name = origin_function[:left_bracket_index].replace("def ",
                                                             '').strip()
    port = 8080

    code = f"""import json
from socket import *

def call_{func_name}{args_str}:
    client_socket = socket(AF_INET, SOCK_DGRAM)
    client_socket.connect(('127.0.0.1', 8080))
    content = {args_to_dict(args_list)}
    client_socket.send(json.dumps(content).encode('utf-8'))
    data = client_socket.recv(65000)
    client_socket.close()
    return json.loads(data.decode('utf-8'))['return'] """
    return code


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
