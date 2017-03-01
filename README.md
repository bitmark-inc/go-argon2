# go-argon2

[![GoDoc](https://godoc.org/github.com/bitmark-inc/go-argon2?status.svg)](https://godoc.org/github.com/bitmark-inc/go-argon2)

Go bindings for the reference C implementation of
[Argon2](https://github.com/P-H-C/phc-winner-argon2), the winner of the
[Password Hash Competition](https://password-hashing.net).

## Installation
This package depends on `libargon2`, specifically the ~~static~~ dynamic library
~~libargon2.a~~ `libargon2.so` and header `argon2.h`. If these are already available in your
default search paths, you can simply install it directly using `go get`:

```
$ go get github.com/bitmark-inc/go-argon2
```

Note: see https://github.com/bitmark-inc/freebsd-ports under the
`security/libargon2` directory for building and installing the dynamic
library on FreeBSD.

~~Otherwise, get this package without installing it directly and use the library
submodule in this repository:~~

```
$ go get -d github.com/bitmark-inc/go-argon2
$ cd $GOPATH/src/github.com/bitmark-inc/go-argon2
$ git submodule update --init
$ cd libargon2
$ make && make test
$ go test github.com/bitmark-inc/go-argon2
```

~~Until the library API has stabilized, it's probably better to use the latter
approach.~~

## Usage
### Raw hash with default configuration

```go
hash, err := argon2.Hash(argon2.NewContext(), []byte("password"), []byte("somesalt"))
if err != nil {
	log.Fatal(err)
}

fmt.Printf("%x\n", hash)
```

### Encoded hash with custom configuration

```go
ctx := &argon2.Context{
	Iterations:  5,
	Memory:      1 << 16,
	Parallelism: 2,
	HashLen:     32,
	Mode:        argon2.ModeArgon2i,
    Version:     argon2.Version13,
}

s, err := argon2.HashEncoded(ctx, []byte("password"), []byte("somesalt"))

if err != nil {
	log.Fatal(err)
}
fmt.Println(s)
```
