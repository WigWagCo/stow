Stow 
==========

[![GoDoc](https://godoc.org/github.com/djherbis/stow?status.svg)](https://godoc.org/github.com/djherbis/stow)
[![Release](https://img.shields.io/github/release/djherbis/stow.svg)](https://github.com/djherbis/stow/releases/latest)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE.txt)
[![Build Status](https://travis-ci.org/djherbis/stow.svg?branch=master)](https://travis-ci.org/djherbis/stow) 
[![Coverage Status](https://coveralls.io/repos/djherbis/stow/badge.svg?branch=master)](https://coveralls.io/r/djherbis/stow?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/djherbis/stow)](https://goreportcard.com/report/github.com/djherbis/stow)

Usage
------------

This package provides a persistence manager for objects backed by boltdb.

```go
package stow

import (
  "os"
  "testing"

  "github.com/boltdb/bolt"
)

func TestJson(t *testing.T) {

  // Create a boltdb database
  db, err := bolt.Open("my.db", 0600, nil)
  if err != nil {
    t.Error(err.Error())
  }
  defer os.Remove("my.db")
  defer db.Close()


  // Defined our test Type
  type MyType struct {
    FirstName string `json:"first"`
    LastName  string `json:"last"`
  }

  // Create a Json-encoded Store, Xml and Gob are also built-in
  // We'll we store the name in a boltdb bucket named "names"
  store := NewJSONStore(db, []byte("names"))

  // Store the object
  store.Put("hello", &MyType{"Derek", "Kered"})

  // For each element in the store
  store.ForEach(func(key string, name MyType){
    fmt.Println(key, name)
  })

  // Get the object
  var name MyType
  store.Pull("hello", &name)

  // Verify
  if name.FirstName != "Derek" || name.LastName != "Kered" {
    t.Errorf("Unexpected name: %v", name)
  }
}
```

Installation
------------
```sh
go get gopkg.in/djherbis/stow.v2
```
