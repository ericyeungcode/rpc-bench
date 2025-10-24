# Java version: java 8

```bash
sdk default java 8.0.462-tem
```


# Build & run aeron

```bash
mvnd clean package
```


Run media driver first.

```bash
# run in terminal window 1
# aeron need MediaDriver first
java -cp target/rpc-bench-aeron-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.aeron.LowLatencyMediaDriver

# run in terminal window 2
java -cp target/rpc-bench-aeron-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.aeron.BenchServer

# run in terminal window 3
java -cp target/rpc-bench-aeron-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.aeron.BenchClient

```


# Issues


## CnC file not found: cnc.dat
```
java -cp target/rpc-bench-aeron-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.aeron.BenchServer                 ✔ 
Exception in thread "main" io.aeron.exceptions.DriverTimeoutException: CnC file not found: cnc.dat
	at io.aeron.Aeron$Context.connectToDriver(Aeron.java:628)
	at io.aeron.Aeron$Context.conclude(Aeron.java:295)
	at io.aeron.Aeron.<init>(Aeron.java:94)
	at io.aeron.Aeron.connect(Aeron.java:138)
	at au.com.acegi.rpcbench.aeron.BenchServer.<init>(BenchServer.java:109)
	at au.com.acegi.rpcbench.aeron.BenchServer.main(BenchServer.java:116)
```

io.aeron.exceptions.DriverTimeoutException: CnC file not found: cnc.dat
`— means your Aeron Media Driver isn’t running.`

Aeron applications (like BenchServer) need to connect to the MediaDriver process, which manages shared memory (IPC) or UDP communication. The "cnc.dat" file (“command and control” file) is the shared memory connection between your app and the driver — if it doesn’t exist, the driver isn’t running.

You need to start the Aeron MediaDriver first.

If your project already includes one, run:
```bash
java -cp target/rpc-bench-aeron-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.aeron.LowLatencyMediaDriver
```


# Run grpc


```bash
java -cp target/rpc-bench-grpc-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.grpc.BenchServer

# another terminal window
java -cp target/rpc-bench-grpc-0.0.1-SNAPSHOT.jar au.com.acegi.rpcbench.grpc.BenchClient
```