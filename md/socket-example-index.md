---
title: socket example index (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'index'

Functions in program: 
* `def ping_test():`
* `def long_running():`
* `def connected():`

## python index

Python socket example: index

```python
# Set this variable to "threading", "eventlet" or "gevent" to test the
# different async modes, or leave it set to None for the application to choose
# the best option based on available packages.
# Taken from https://github.com/miguelgrinberg/Flask-SocketIO/blob/master/example/app.py#L30-L31
async_mode = None

if async_mode is None:
    try:
        import eventlet
        async_mode = 'eventlet'
    except ImportError:
        print('Sm')
        pass

    if async_mode is None:
        try:
            from gevent import monkey
            async_mode = 'gevent'
        except ImportError:
            pass

    if async_mode is None:
        async_mode = 'threading'

    print('async_mode is ' + async_mode)

if async_mode == 'eventlet':
    import eventlet
    eventlet.monkey_patch()
elif async_mode == 'gevent':
    from gevent import monkey
    monkey.patch_all()

# End patching

# Do imports 

app = Flask(__name__)
socketio = SocketIO(app, async_mode=async_mode)

# Set tread defaults
thread = None
ping_thread = None
run_threads = True


@socketio.on('connect')
def connected():
    
    global run_threads
    global thread
    global ping_thread

    run_threads = True
    
    if thread is None or not thread.isAlive():
        # Set up the long running loop to listen for any changes from the Sonos
        thread = Thread(target=long_running)
        thread.daemon = True
        thread.start()
    if ping_thread is None or not ping_thread.isAlive():
        ping_thread = Thread(target=ping_test)
        ping_thread.daemon = True
        ping_thread.start()


def long_running():
    # Using run_threads so we can terminate when we lose connection.
    global run_threads, thread

    while run_threads:
        # Do stuff here.
        pass
    

def ping_test():
    """
    Test for internet connectivity.
    Will spawn as a seperate thread.
    """
    global run_threads
    while run_threads:
        try:
            urlopen("http://www.google.com").close()
        except URLError:
            logging.error("Internet has been disconnected")
            time.sleep(3)
        else:
            time.sleep(10)
            
            
        
if __name__ == '__main__':
    # use_reloader=False is needed otherwise we will spawn extra threads.
    socketio.run(app, use_reloader=False, debug=app.config['DEBUG'])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
