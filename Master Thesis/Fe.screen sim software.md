
---

Q. How to use Grpcsample tool to log the data?

1. Need to make sure the gRPC api iterface is active in fe.screen-sim core settings.
2. Need to generate .proto files which generate client stubs in preferred programming language.
3. write a script which will query continously the state of the target sensors and actuators at defined intervals.
4. log the data using the the script to save the data into desired formate

Note: use dotnet run

TODO: 


To have the simulation window

windows-> render window
