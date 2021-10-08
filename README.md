# gRPC POC 

Perchè [GRPC](https://grpc.io/docs/languages/java/basics/) (Goole Remote Procedure Call) per la comunicazione tra microservizi:

1) **definizione dei servizi su file .proto** (protobuf) da cui si possono generare client e server, esempio:

```protobuf
syntax = "proto3";

option java_multiple_files = true;
option java_package = "net.devh.boot.grpc.examples.lib";
option java_outer_classname = "HelloWorldProto";

// service definition.
service Simple {
    // Sends a greeting
    rpc SayHello (HelloRequest) returns (HelloReply) {
    }
}

// The request message containing the user's name.
message HelloRequest {
    string name = 1;
}

// The response message containing the greetings
message HelloReply {
    string message = 1;
}
```

2) **possibilità di streaming tra microservizi** ( anche bi-direzionale ) perchè è basato su HTTP 2 invece che HTTP 1 ;

3) **performance**: i dati sono compressi ( protocollo http2 + protocol buffers, i dati viaggiano in binario, non si passa alle stringhe JSON  ) ( gRPC’s low-latency and high-speed throughput communication make it particularly useful for connecting architectures that consist of lightweight microservices where the efficiency of message transmission is paramount. [LINK](https://blog.dreamfactory.com/grpc-vs-rest-how-does-grpc-compare-with-traditional-rest-apis/) ) 
Esempio di comparison performances:  https://medium.com/@EmperorRXF/evaluating-performance-of-rest-vs-grpc-1b8bdf0b22da 

![img](https://miro.medium.com/max/2000/1*fPilxI_9oncBC1bJSh4Hsg.png)

4) integrazione già prevista (e testata) con spring-cloud per il **logging distribuito**.

___



Questo progetto è basato interamente su https://github.com/yidongnan/grpc-spring-boot-starter ed è diviso in 3 cartelle


- client -> web server tomcat con controller rest standard

  - localhost:8080/sync permette di inviare un messaggio sincrono
  - localhost:8080/async invia un messaggio asincrono ( usando i feature java )
    

- server -> implementazione spring boot embedded di Netty NIO (non blocking i-o) server;

- lib  -> contiene il file di definizione e le classi generate da esso per la comunicazione tra client e server; per la generazione basta "buildare" il progetto e installarlo sul repo locale usando gradle publish



