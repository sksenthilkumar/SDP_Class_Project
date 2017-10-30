# SDP_Class_Project
Winter Semester 2017: Software Development class exercise

import thread
import time

# ZeroMQ Context
context = zmq.Context()

# Define the socket using the "Context"
sock = context.socket(zmq.PUB)
sock.bind("tcp://*:5680")

sock1 = context.socket(zmq.SUB)
sock1.setsockopt(zmq.SUBSCRIBE, "1")
sock1.connect("tcp://10.0.0.15:5680")

id = 0
#msg = raw_input("Enter your message: ")

def send():
    while True:
        print "send"
        msg = raw_input("Enter your message: ")
        time.sleep(1)
        id, now = id+1, time.ctime()
        message = "1-Update! >> #{id} >> {time}>>{msg}".format(id=id, time=now,msg=msg)
        sock.send(message)
    
def recv():
    while True:

        message2= sock1.recv()
        print message2

# Define a function for the thread
def print_time( threadName, delay):
    count = 0
    while count < 100:
        time.sleep(delay)
        count += 1
        print "%s: %s" % ( threadName, time.ctime(time.time()) )

# Create two threads as follows
try:
    thread.start_new_thread( send() )
    thread.start_new_thread( recv() )
    
except:
    print "Error: unable to start thread"

while 1:
    pass
