---
title: "hz layout"
date: 2023-02-21
weight: 5
keywords: ["hz layout"]
description: "The structure of hz generated code."
---

## The structure of the generated code

The structure of the code generated by hz is similar. Below, we will use the thrift code in the [hz usage(thrift)](/docs/hertz/tutorials/toolkit/usage-thrift/) section as an example to illustrate the meaning of the code generated by hz and hz client.

### hz

```
.
├── biz                                // business layer, which stores business logic related processes
│   ├── handler                        // store handler file
│   │   ├── hello                      // hello/example corresponds to the namespace defined in thrift IDL; for protobuf IDL, it corresponds to the last level of go_package
│   │   │   └── example
│   │   │       └── hello_service.go   // the handler file, the user will implement the method defined by the IDL service in this file, it will search for the existing handler in the current file when "update" command, and append a new handler to the end
│   │   └── ping.go                    // ping handler carried by default, used to generate code for quick debugging, no other special meaning
│   ├── model                          // IDL content-related generation code
│   │   └── hello                      // hello/example corresponds to the namespace defined in thrift IDL; for protobuf IDL, it corresponds to the last level of go_package
│   │       └── example
│   │           └── hello.go           // the product of thriftgo, It contains go code generated from the contents of hello.thrift definition. And it will be regenerated when use "update" command
│   └── router                         // generated code related to the definition of routes in IDL
│       ├── hello                      // hello/example corresponds to the namespace defined in thrift IDL; for protobuf IDL, it corresponds to the last level of go_package
│       │   └── example
│       │       ├── hello.go           // the route registration code generated for the routes defined in hello.thrift by hz; this file will be regenerated each time the relevant IDL is updated
│       │       └── middleware.go      // default middleware function, hz adds a middleware for each generated route group by default; when updating, it will look for the existing middleware in the current file and append new middleware at the end
│       └── register.go                // call and register the routing definition in each IDL file; when a new IDL is added, the call of its routing registration will be automatically inserted during the update; do not edit
├── go.mod                             // go.mod file, if not specified on the command line, defaults to a relative path to GOPATH as the module name
├── idl                                // user defined IDL, location can be arbitrary
│   └── hello.thrift
├── main.go                            // program entry
├── router.go                          // user defined routing methods other than IDL
└── router_gen.go                      // the route registration code generated by hz, for calling user-defined routes and routes generated by hz
├── .hz                                // create code flags for hz without modification
├── build.sh                           // program compilation script cannot be run under Windows, you can directly use the go build command to compile the program
├── script                                
│   └── bootstrap.sh                   // program running script, cannot run under Windows, can directly run main.go
└── .gitignore
```

### hz client

```
.
├── biz                                  // business layer, which stores business logic 
│   └── model                            // IDL content-related generation code
│       └── hello                        // hello/example corresponds to the namespace defined in thrift IDL; for protobuf IDL, it corresponds to the last level of go_package
│           └── example
│               ├── hello.go             // the product of thriftgo, It contains go code generated from the contents of hello.thrift definition. And it will be regenerated when use "update" command
│               └── hello_service        // the http request one click call code generated based on idl, similar to the RPC form, can directly communicate with the server code generated by hz
│                   ├── hello_service.go
│                   └── hertz_client.go
│
├── go.mod                               // go.mod file, if not specified on the command line, defaults to a relative path to GOPATH as the module name
└── idl                                  // user defined IDL, location can be arbitrary
    └── hello.thrift
```
