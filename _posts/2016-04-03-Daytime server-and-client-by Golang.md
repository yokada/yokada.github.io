---
layout: post
title: Daytime server and client by Golang
tags: golang, unp
---

Below code is a simple daytime server and client I wrote in Golang.
The server can only tweaks one request per accesse.
"UNIX NETWORK PROGRAMMING" is one of the most practical books for network programming.
So, I have been refered to the book many times.

```go
// daytimeserver.go

package main

import (
    "log"
    "net"
    "time"
)

/*
Usage example:
    $ go run daytimetcpcli.go 98.175.203.200 13
    57480 16-04-02 13:33:59 50 0 0 905.5 UTC(NIST)
You can find valid daytime server URLs and IP Addresses from here:
    [ NIST Internet Time Service ]( http://tf.nist.gov/tf-cgi/servers.cgi )
daytime protocol reference:
    [ RFC 867 - Daytime Protocol ]( https://tools.ietf.org/html/rfc867 )
*/
func main() {
    tcpAddr, err := net.ResolveTCPAddr("tcp", ":8888")
    if err != nil {
        log.Fatal(err)
    }

    listener, err := net.ListenTCP("tcp", tcpAddr)
    if err != nil {
        log.Fatal(err)
    }

    for {
        conn, err := listener.Accept()
        if err != nil {
            log.Fatal(err)
        }

        go handleListener(conn)
    }
}


func handleListener(conn net.Conn) {
    defer conn.Close()
    conn.SetWriteDeadline(time.Now().Add(10 * time.Second))

    t := time.Now()
    //msg := t.Format("Mon Jan 2 15:04:05 2006 -0700 MST")
    msg := t.Format(time.RFC1123)
    conn.Write([]byte(msg))
}
```

And, the client code is here:

```go
// daytimetcpcli.go

package main

import (
    "fmt"
    "flag"
    "log"
    "net"
    "time"
)

/*
Usage example:
    $ go run daytimetcpcli.go 98.175.203.200 13
    57480 16-04-02 13:33:59 50 0 0 905.5 UTC(NIST) *
You can find valid daytime server URLs and IP Addresses from here:
    [ NIST Internet Time Service ]( http://tf.nist.gov/tf-cgi/servers.cgi )
*/
func main() {
    flag.Parse()
    args := flag.Args()
    if len(args) != 2 {
        log.Fatal("Usage: daytimetcpcli.go <IP ADDRESS> <PORT>")
        return
    }
    serverIp := args[0]
    serverPort := args[1]
 
    conn, err := net.Dial("tcp", serverIp+":"+serverPort)
    if err != nil {
        log.Fatal(err)
    }
    defer conn.Close()

    readBuf := make([]byte, 1024)
    // set timeout duration as absolute soconds from now for reading data.
    conn.SetReadDeadline(time.Now().Add(10 * time.Second))
    readLen, err := conn.Read(readBuf)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println(string(readBuf[:readLen]))
}
```



