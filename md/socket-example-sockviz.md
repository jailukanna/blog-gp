---
title: socket example sockviz (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockviz'

Functions in program: 
* `def plot_graphviz_per_pid (processes, output) :`
* `def plot_graphviz_merge_pid (processes, output) :`
* `def plot_graphviz_process_only (processes, output) :`
* `def plot_graphviz (processes, output) :`
* `def parse_lsof_output (lines) :`

Modules used in program: 
* `import pygraphviz as pgv`
* `import commands`
* `import json`
* `import sys`
* `import re`

## python sockviz

Python socket example: sockviz

```python
#!/usr/bin/env python


import re
import sys
import json
import commands
import pygraphviz as pgv


lsofipv4arg = "-n -P -d 0-128 -a -i4 -F"
lsofipv6arg = "-n -P -d 0-128 -a -i6 -F"
lsofipv46arg = "-n -P -d 0-128 -a -i -F"
lsofallarg = "-n -P -d 0-128 -F"

lsofarg = lsofipv46arg
#lsofarg = lsofallarg


outputfile = None
jsonfile = None

for arg in sys.argv :
    if re.search (r'.png$', arg) :
        outputfile = arg
    if re.search (r'.json$', arg) :
        jsonfile = arg

graphlayout = "fdp"


def parse_lsof_output (lines) :

    # generate socket graph from lsof output.
    # this parse IPv4, IPv6 and REG type file descriptors.

    """
    lsof = [ 
             {
                 'pid'  : process_id,
                 'name' : process_name, 
                 'sockets' : [ # multiple sockets may exist for a process
                     { 
                         'type' : IPv4 or IPv6 or REG,
                         'proto' : TCP or UDP,
                         'tst' : tst,  # ESTABLISHED or LISTEN
                         'src' : IPv4SRC:PORT,
                         'dst' : IPv4DST:PORT,  # if tst is ESTABLISHED 
                         'file' : /regular/file/path,
                     },
                 ]
             },
    ]
    """

    lsof = []
    process = None
    sockets = None
    socket = None
    supported_type = [ "IPv4", "IPv6", "REG"]

    for line in lines :

        if line[0] == 'p' :
            # new process
            process = { 'pid' : int (line[1:]), 'sockets' : [] }
            sockets = process['sockets']
            lsof.append (process)

        if line[0] == 'c' :
            process['name'] = line[1:]


        if line[0] == 't' :
            # new socket
            if line[1:] in supported_type :
                socket = { 'type' : line[1:] }
                sockets.append (socket)
            else :
                socket = None
            
        if socket and line[0] == 'P' :
            socket['proto'] = line[1:]

        if socket and line[0] == 'n' :
            if socket['type'] == "REG" :
                socket['file'] = line[1:]

            else :

                if re.search (r'->', line) :
                    (src, dst) = line[1:].split("->")
                    socket['src'] = src
                    socket['dst'] = dst
                else :
                    socket['src'] = line[1:]
                    socket['dst'] = None

        if socket and line[0:4] == "TST=" :
            socket['tst'] = line[4:]
                

    # remove no socket processes

    dellist = []
    for process in lsof :
        if len (process['sockets']) == 0 :
            dellist.append (process)
    for x in dellist :
        lsof.remove (x)

    # merge same command processes into one process
    processes = {} # { process_name : [ process, process, process ] }
    for process in lsof :
        if not processes.has_key (process['name']) :
            processes[process['name']] = []
        processes[process['name']].append (process)

    return processes


def plot_graphviz (processes, output) :
    #plot_graphviz_per_pid (processes, output)
    #plot_graphviz_merge_pid (processes, output)
    plot_graphviz_process_only (processes, output)


def plot_graphviz_process_only (processes, output) :

    def find_dst_process_by_socket (processes, dst) :
        for cmd in processes.keys () :
            for process in processes[cmd] :
                for socket in process['sockets'] :
                    if socket.has_key ('src') and dst == socket['src'] :
                        return process
        return None

    graph = pgv.AGraph ()

    for cmd in processes.keys () :
        graph.add_node (cmd, fontsize = 20, penwidth = 3)
        
    for cmd in processes.keys () :
        for process in processes[cmd] :
            for socket in process['sockets'] :
                if socket['type'] in ("IPv4", "IPv6") :
                    dst = find_dst_process_by_socket (processes,
                                                      socket['dst'])
                    if dst :
                        graph.add_edge (cmd, dst['name'], penwidth = 3)

    graph.layout (prog = graphlayout)
    graph.draw (output)


def plot_graphviz_merge_pid (processes, output) :

    graph = pgv.AGraph ()

    for cmd in processes.keys () :

        graph.add_node (cmd, color = "red", fontsize = 20)

        for process in processes[cmd] :

            for socket in process['sockets'] :
                if socket['type'] == "REG" :
                    graph.add_node (socket['file'], penwidth = 3)
                    graph.add_edge (cmd, socket['file'], penwidth = 3)

                else :
                    # IPv4 or IPv6 socket
                    graph.add_node (socket['src'], color="blue",
                                    shape = "hexagon", penwidth = 3)
                    graph.add_edge (cmd, socket['src'], color = "red",
                                    penwidth = 3)
                    
                    if socket['proto'] == "TCP" :

                        if socket['tst'] in ("CLOSE_WAIT", "ESTABLISHED") :
                            graph.add_node (socket['dst'], color = "blue",
                                            shape = "hexagon", penwidth = 3)
                            graph.add_edge (socket['src'], socket['dst'],
                                            color = "blue", penwidth = 3)

    graph.layout (prog = graphlayout)
    graph.draw (output)




def plot_graphviz_per_pid (processes, output) :

    graph = pgv.AGraph ()

    for cmd in processes.keys () :

        graph.add_node (cmd)

        for process in processes[cmd] :
            graph.add_node (process['pid'], penwidth = 3)
            graph.add_edge (cmd, process['pid'], penwidth = 3)

            for socket in process['sockets'] :
                if socket['type'] == "REG" :
                    graph.add_node (socket['file'], penwidth = 3)
                    graph.add_edge (process['pid'], socket['file'],
                                    penwidth = 3)

                else :
                    # IPv4 or IPv6 socket
                    graph.add_node (socket['src'], penwidth = 3)
                    graph.add_edge (process['pid'], socket['src'],
                                    penwidth = 3)
                    
                    if socket['proto'] == "TCP" :

                        if socket['tst'] in ("CLOSE_WAIT", "ESTABLISHED") :
                            graph.add_node (socket['dst'], penwidth = 3)
                            graph.add_edge (socket['src'], socket['dst'],
                                            penwidth = 3)

    graph.layout (prog = graphlayout)
    graph.draw (output)



lines = commands.getoutput ("%s %s" % ("lsof", lsofarg)).split ("\n")

if not jsonfile :
    processes = parse_lsof_output (lines)
else :
    f = open (jsonfile, 'r')
    processes = json.load (f)
    f.close

print(json.dumps (processes, indent = 4))

if outputfile :
    print(>> sys.stderr, "generate graph file '%s'" % outputfile)
    plot_graphviz (processes, outputfile)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
