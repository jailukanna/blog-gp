---
title: socket example main (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'main'

Functions in program: 
* `def home():`
* `def device_client_socket(ws):`
* `def unity_client_socket(ws):`
* `def make_player(ws):`

Modules used in program: 
* `import json`

## python main

Python socket example: main

```python
from flask import Flask
from flask_sockets import Sockets
import json
from random import randint

app = Flask(__name__)
sockets = Sockets(app)

current_players = {}
game_x_min = 0
game_x_max = 100
app_port = 5000

ug_socket = {
    'ws': None
}


def make_player(ws):
    player = {
        'colour': [randint(0, 255) for _ in range(3)],
        'hookpos': 0,
        'xposition': randint(game_x_min, game_x_max)
    }
    current_players[ws] = player
    # inform the unity game about this?


@sockets.route('/unity')
def unity_client_socket(ws):
    # should check that unity is running locally only
    if ug_socket['ws'] is not None or ws.handler.client_address[0] != '127.0.0.1':
        return
    ug_socket['ws'] = ws

    while not ws.closed:
        # do stuff here i guess...
        message = ws.receive().lower()
    ug_socket['ws'] = None


@sockets.route('/client')
def device_client_socket(ws):
    make_player(ws)
    while not ws.closed:
        message = ws.receive().lower()
        player_state = current_players[ws]
        if message == 'left':
            player_state['xposition'] = max(game_x_min, player_state['xposition'] - 1)
        elif message == 'right':
            player_state['xposition'] = min(game_x_max, player_state['xposition'] + 1)
        ws.send(json.dumps(current_players[ws]))
    del current_players[ws]


@app.route('/')
def home():
    return """
<html>
  <head>
    <title>
      Fishing Game
    </title>

  </head>
  <body>
    <h2><a href="#">Left</a></h2>
    <h2><a href="#">Right</a></h2>
    <h2><a href="#">Drop</a></h2>
    <pre id="output"></pre>
  </body>
  <script src="//cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
  <script type="text/javascript">
    $(document).ready(function() {
      ws = new WebSocket('ws://'+document.domain+':{APP_PORT}/client')
      ws.onmessage = function (event) {
        console.log(event.data);
        $('#output').text(event.data);
      }
       $('h2').click(function() {
          ws.send($(this).text());
       });
    });
  </script>
</html>    
    """.replace('{APP_PORT}', str(app_port))


if __name__ == "__main__":
    from gevent import pywsgi
    from geventwebsocket.handler import WebSocketHandler

    server = pywsgi.WSGIServer(('localhost', app_port), app, handler_class=WebSocketHandler)
    server.serve_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
