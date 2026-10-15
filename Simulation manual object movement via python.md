

---

we use ObjectAPI via gRPC ObjectAPiStub to move the objects in simulation.


After providing the details which we give in data collection pipeline as well we get connected to the network.


we target transform component and modify properties like local-position, local-rotation *(need to check about gloabl coordinates)*



we use (x,y,z) values for position and rotation(deg)

IMPORTANT:

when are connected we need to first unlock the object so we use **UnlockObjectrequest** function.


Currently we need to give the GUID of the object to identify it.


