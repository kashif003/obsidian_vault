

----
#dataextraction #fescreensim #portofiles


To get the data out we need the following things:

1. schema files (.porto files)
2. Translation layers.



Q: why are we not using the normal requests library?
- the software(fe.screen sim) is build on the gRPC(Google remote procedure call) not on the traditional REST API.
- standard web servers like REST API are designed for typical web servers that talk in json or XML or HTTP/1.1 form while the software exposes the  internal variables through gEPC endpoints which does not  have the standdard GET7POST requests, it expects the gRPC client connection using **protocol buffers**.

**Schema files**
.proto: protocol buffer files
+ These files act as the blueprint between the client(python script) and server:
	+ we get to know what are the data structures that exist like variablesearch, tag, adress, interfacename etc.
	+ we also get to know what methods we can call on the server like getvariables value etc.
	+ we have the information about what data types each feild expects.
	+ As we are using the gRPC connection out data is serialized into utlra compact binary format.
Q: why do we need them? 
	
we need to know how the data is packed into the binary for gPRC and unpacked.
these files also store name of the variable value request



**Q: How are we using them?**
	We feed `.proto` files to **`protoc`**, which is a compiler that translates these files into **`.py` files**. These generated `.py` files essentially do the work of translating the raw `.proto` definitions into Python scripts, and we import and use these `.py` scripts directly inside our main code.

**Translation layers.**

Q: How do we compile the files?
	`python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. API/VariablesApi.proto`

*we just give me input and ouput location of the two python files and also the location of our porto file which need conversion.*


Finally using these .py files we can communicate with the software provided we have the adress and port number.



---

# How to get the data for the variable?

SENSOR DATA.
1. need to make a channel throught grpc.py
2. using that channel we need to make a stub which will give us sensor values but it needs following things:
	1. value request.
3. values request need the search rule.
4. in search rule we define the interface name and the sensor tag.

5. finally stub takes value request and meta data which is username and password and gives us the data we need.



---

How are values stored?

for a single RF e.g RF 0304 or RF 0301 we have six or less values:

1. RF 0304 .Pulk *(pull)*   -> E30.5
2. RF 0304 .Rückwärts *(backwards)*    -> A30.3
3. RF 0304 .Spaltkontrolle *(gap control)* DT0209  -> E30.7
4. RF 0304 .Stop *(stop)*       -> E30.6
5. RF 0304 .Stop rückwärts *(reverse)*   -> E30.4
6. RF 0304 .Vorwärts *(forward)*    -> A30.2


and out of these six values we have 4 sensor (E...) values and 2 actuator values (A...).


The numbering/naming of these values are done in order.


**IMPORTANT**
I am not able to locate the  initial actuator name/values which are at the start or at the end of simulation model.

---

How to locate the name and value in sim software?

At the bottom of each bed we have a box click on it to extract the name of it (RF0214)

go to the connection table and in variables search that name with space in between (RF 0214).

then variables matching the RF and the number will give us the actuator/ sensor address, tag, value.


----




