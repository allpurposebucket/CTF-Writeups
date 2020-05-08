# **Move ZIG**
## Points: 75
### **Description:** CATS is back at it on challenge.acictf.com:21191. Only this time he's throwing 500 problems at you. If you didn't use a program to solve the last one, you'll probably want [this](https://challenge.acictf.com/static/709b888d2c6be377ffa796a3a6c14ce8/starter_code.py)..

This challenge is simple in concept, but tedious in execution. All we have to do is read what the server sends us, parse that, and convert data from one encoding to another.
Instead of accounting for every single conversion (36 or so), I converted everything to a common, intermediate encoding (bytes) and then converting the bytes data to the final encoding for each problem. 
Using the provdided starter code, this is the code that I wrote. 

```python
#!/usr/bin/python3
import argparse
import socket
import base64
import math

# 'argparse' is a very useful library for building python tools that are easy
# to use from the command line.  It greatly simplifies the input validation
# and "usage" prompts which really help when trying to debug your own code.
parser = argparse.ArgumentParser(description="Solver for 'All Your Base' challenge")
parser.add_argument("ip", help="IP (or hostname) of remote instance")
parser.add_argument("port", type=int, help="port for remote instance")
args = parser.parse_args();

# This tells the computer that we want a new TCP "socket"
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# This says we want to connect to the given IP and port
sock.connect((args.ip, args.port))

# This gives us a file-like view for receiving data from the connection which
# makes handling messages from the server easier since it handles the
# buffering of lines for you.  Note that this only helps us on receiving data
  # from the server and we still need to send data over the underlying socket
# (i.e. `sock.send(...)` at the end of the loop below).
f = sock.makefile()

def toInterm(inBase, data):
	if inBase == 'bin':
		return toBytes(int(data, 2))
	elif inBase == 'hex':
		return bytes.fromhex(data)
	elif inBase == 'oct':
		print("OCT: ", len(data), len(int(data, 8).to_bytes(len(data), byteorder='big')))
		return toBytes(int(data, 8))
	elif inBase == 'raw':
		return data.encode('UTF-8')
	elif inBase == 'b64':
		return base64.standard_b64decode(bytes(data, 'UTF-8'))
	elif inBase == 'dec':
		return toBytes(int(data))
	else:
		assert False

def toFinal(interm, outBase):
	if outBase == 'bin':
		return bin(int.from_bytes(interm, 'big'))[2:]
	elif outBase == 'hex':
		return hex(int.from_bytes(interm, 'big'))[2:]
	elif outBase == 'oct':
		return oct(int.from_bytes(interm, 'big'))[2:]
	elif outBase == 'raw':
		print("RAW: ", len(interm), len (interm.decode('UTF-8')))
		return interm.decode('UTF-8').lstrip('\0')
	elif outBase == 'b64':
		return base64.standard_b64encode(interm).decode('UTF-8').lstrip('A')
	elif outBase == 'dec':
		return str(int.from_bytes(interm, 'big'))

def toBytes(i):
	return i.to_bytes(math.ceil(i.bit_length()/8), 'big')


def process(inBase, outBase, data):
	interm = toInterm(inBase, data)
	return toFinal(interm, outBase)

def error(f):
	for line in f:
		print(f"error: {line.strip()}")

def skip(f):
	for line in f:
		print(f"skip: {line.strip()}")
		if "-"*78 in line:
			break

while True:

	skip(f)

	line = f.readline().strip()
	print("Start: \t" + line)
	if '->' not in line:
		error(f)
	(inBase, arrow, outBase) = line.split()
	data = f.readline().strip()
	print(data)
	answer = process(inBase, outBase, data)
	print(f"   {answer}")
	sock.send((answer + "\n").encode())
	skip(f)
```

500 conversions later I got the flag.

## **Flag:** ACI{for_great_justice_80c6b165}
