# snap-plugin-example

To get these example collector, processor, and publisher plugins to build properly and work with Snap you will need to have the following installed on your $GOPATH:
* [glide](https://glide.sh/) if you do not already have it 
* [gPRC](http://www.grpc.io/docs/) `go get google.golang.org/grpc`
If you don’t already have [Snap](https://github.com/intelsdi-x/snap) installed, check out these docs for [Snap setup details] (https://github.com/intelsdi-x/snap/blob/master/docs/BUILD_AND_TEST.md#getting-started).

If already have Snap installed follow these steps:

1. go to your intelsdi-x folder: 
`$ cd $GOPATH/src/github.com/intelsdi-x`

2. Fork the [Snap Plugin Library for Go Repository](https://github.com/intelsdi-x/snap-plugin-lib-go) & clone to the intelsdi-x folder:
`$ git clone https://github.com/sarahjhh/snap-plugin-lib-go.git` (don’t forget to use your github username instead)!

3. Once you have the repo downloaded go to the snap-plugin-lib-go folder and update to the newest versions of the package:

```
$ cd snap-plugin-lib-go
$ glide up
[INFO]	Downloading dependencies. Please wait...
[INFO]	--> Fetching updates for github.com/jtolds/gls.
[INFO]	--> Fetching updates for github.com/golang/protobuf.
[INFO]	--> Fetching updates for github.com/smartystreets/assertions.
[INFO]	--> Fetching updates for github.com/smartystreets/goconvey.
[INFO]	--> Fetching updates for google.golang.org/grpc.
[INFO]	--> Fetching updates for github.com/gopherjs/gopherjs.
[INFO]	--> Fetching updates for golang.org/x/net.
[INFO]	--> Setting version for github.com/golang/protobuf to 888eb0692c857ec880338addf316bd662d5e630e.
[INFO]	--> Setting version for github.com/smartystreets/assertions to 443d812296a84445c202c085f19e18fc238f8250.
[INFO]	--> Setting version for github.com/smartystreets/goconvey to 995f5b2e021c69b8b028ba6d0b05c1dd500783db.
[INFO]	--> Setting version for github.com/gopherjs/gopherjs to 4b53e1bddba0e2f734514aeb6c02db652f4c6fe8.
[INFO]	--> Setting version for github.com/jtolds/gls to 8ddce2a84170772b95dd5d576c48d517b22cac63.
[INFO]	--> Setting version for google.golang.org/grpc to 0032a855ba5c8a3c8e0d71c2deef354b70af1584.
[INFO]	--> Setting version for golang.org/x/net to 154d9f9ea81208afed560f4cf27b4860c8ed1904.
[INFO]	Resolving imports
[INFO]	Downloading dependencies. Please wait...
[INFO]	Setting references for remaining imports
[INFO]	Exporting resolved dependencies...
[INFO]	--> Exporting github.com/gopherjs/gopherjs
[INFO]	--> Exporting github.com/golang/protobuf
[INFO]	--> Exporting github.com/jtolds/gls
[INFO]	--> Exporting github.com/smartystreets/assertions
[INFO]	--> Exporting github.com/smartystreets/goconvey
[INFO]	--> Exporting google.golang.org/grpc
[INFO]	--> Exporting golang.org/x/net
[INFO]	Replacing existing vendor dependencies
[INFO]	Project relies on 7 dependencies. ```
```
4. You can test the main.go file by:
```
$ go run examples/collector/main.go 
{"Meta":{"Type":0,"Name":"test-rand-collector","Version":1,"RPCType":2,"RPCVersion":1,"ConcurrencyCount":5,"Exclusive":false,"Unsecure":true,"CacheTTL":0,"RoutingStrategy":0},"ListenAddress":"127.0.0.1:56914","Type":0,"State":0,"ErrorMessage":""}
Heartbeat started
Heartbeat timeout reset
Heartbeat timeout 1 of 3.  (Duration between checks 1.5s)Heartbeat timeout 2 of 3.  (Duration between checks 1.5s)Heartbeat timeout 3 of 3.  (Duration between checks 1.5s)Heartbeat timeout expired!
```

5. You can then build the collector, processor, or publisher plugins in the examples folder.

```
$ go build -o example-collector examples/collector/main.go
$ go build -o example-processor examples/processor/main.go 
$ go build -o example-publisher examples/publisher/main.go 
```

6. Once you build the plugins you can load them into Snap and watch them work. Check out more Snap commands [here](??) to load plugins, see the metric or plugin list, create tasks, and collect data.

```
$ export SNAP_PATH=$GOPATH/snap/build

$ $SNAP_PATH/bin/snapctl plugin load example-collector
Plugin loaded
Name: test-rand-collector
Version: 1
Type: collector
Signed: false
Loaded Time: Fri, 23 Sep 2016 17:41:44 PDT

$ $SNAP_PATH/bin/snapctl plugin list
NAME 			 VERSION 	 TYPE 		 SIGNED 	 STATUS 	 LOADED TIME
test-rand-collector 	 1 		 collector 	 false 		 loaded 	 Fri, 23 Sep 2016 17:41:44 PDT

$ $SNAP_PATH/bin/snapctl metric list
NAMESPACE 		 VERSIONS
/random/float 		 1
/random/integer 	 1
/random/string 		 1

$ $SNAP_PATH/bin/snapctl plugin load example-processor
Plugin loaded
Name: test-reverse-processor
Version: 1
Type: processor
Signed: false
Loaded Time: Fri, 23 Sep 2016 17:44:12 PDT

$ $SNAP_PATH/bin/snapctl plugin load example-publisher
Plugin loaded
Name: test-file-publisher
Version: 1
Type: publisher
Signed: false
Loaded Time: Fri, 23 Sep 2016 17:44:23 PDT

$ $SNAP_PATH/bin/snapctl plugin list
NAME 			 VERSION 	 TYPE 		 SIGNED 	 STATUS 	 LOADED TIME
test-rand-collector 	 1 		 collector 	 false 		 loaded 	 Fri, 23 Sep 2016 17:41:44 PDT
test-reverse-processor 	 1 		 processor 	 false 		 loaded 	 Fri, 23 Sep 2016 17:44:12 PDT
test-file-publisher 	 1 		 publisher 	 false 		 loaded 	 Fri, 23 Sep 2016 17:44:23 PDT
```


