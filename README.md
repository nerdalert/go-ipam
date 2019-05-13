# go-ipam

[![Build Status](https://travis-ci.org/metal-pod/go-ipam.svg?branch=master)](https://travis-ci.org/metal-pod/go-ipam)
[![GoDoc](https://godoc.org/github.com/metal-pod/go-ipam?status.svg)](https://godoc.org/github.com/metal-pod/go-ipam)
[![Go Report Card](https://goreportcard.com/badge/github.com/metal-pod/go-ipam)](https://goreportcard.com/report/github.com/metal-pod/go-ipam)
[![codecov](https://codecov.io/gh/metal-pod/go-ipam/branch/master/graph/badge.svg)](https://codecov.io/gh/metal-pod/go-ipam)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/metal-pod/go-ipam/blob/master/LICENSE)

go-ipam is a module to handle IPAddress management. It can operate on Networks, Prefixes and IPs.

## IP

Most obvious this library is all about ip management. the main purpose is to acquire and release an ip, or a bunch of
ip's from prefixes.

## Prefix

A prefix is a network with ip and mask, typically in the form of *192.168.0.0/24*. To be able to manage IPs you have to create a prefix first.

example usage:

```go

package main

import (
    "fmt"
    goipam "github.com/metal-pod/go-ipam"
)


func main() {
    // create a ipamer with in memory storage
    ipam := goipam.New()

    prefix, err := ipam.NewPrefix("192.168.0.0/24")
    if err != nil {
        panic(err)
    }

    ip, err := ipam.AcquireIP(prefix)
    if err != nil {
        panic(err)
    }
    fmt.Printf("got IP: %s", ip.IP)

    prefix, err = ipam.ReleaseIP(ip)
    if err != nil {
        panic(err)
    }
    fmt.Printf("IP: %s released.", ip.IP)
}
```

## Performance

```bash
BenchmarkNewPrefixMemory-4                500000              4629 ns/op
BenchmarkNewPrefixPostgres-4                 200           7425393 ns/op
BenchmarkAcquireIPMemory-4                2000000              1110 ns/op
BenchmarkAcquireIPPostgres-4                  100          10508799 ns/op
BenchmarkAcquireChildPrefix1-4             300000              3693 ns/op
BenchmarkAcquireChildPrefix2-4             300000              3727 ns/op
BenchmarkAcquireChildPrefix3-4             300000              4036 ns/op
BenchmarkAcquireChildPrefix4-4             300000              4372 ns/op
BenchmarkAcquireChildPrefix5-4             200000              5448 ns/op
BenchmarkAcquireChildPrefix6-4             300000              3729 ns/op
BenchmarkAcquireChildPrefix7-4             300000              3766 ns/op
BenchmarkAcquireChildPrefix8-4             500000              3986 ns/op
BenchmarkAcquireChildPrefix9-4             300000              3934 ns/op
BenchmarkAcquireChildPrefix10-4            300000              3858 ns/op
```
