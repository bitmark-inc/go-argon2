# go-argon2

[![GoDoc](https://godoc.org/github.com/bitmark-inc/go-argon2?status.svg)](https://godoc.org/github.com/bitmark-inc/go-argon2)

Go bindings for the reference C implementation of
[Argon2](https://github.com/P-H-C/phc-winner-argon2), the winner of the
[Password Hash Competition](https://password-hashing.net).

## Installation

~~~
$ go get -d github.com/bitmark-inc/go-argon2
~~~

This package depends on `libargon2`, specifically `libargon2.so` and
`argon2.h`.  Make sure the library files are available in `/usr` or
`/usr/local` depending on your OS.  Check that pkg-config can detect
the installed library.

~~~
Debian-like: apt install libargon2-dev
FreeBSD:     pkg install libargon2

all: pkgconfig --cflags --libs libargon2
~~~

Test the Go library

~~~
$ git clone https://github.com/bitmark-inc/go-argon2.git
$ cd go-argon2
$ go test -v ./...


## Usage
### Raw hash with default configuration

~~~go
hash, err := argon2.Hash(argon2.NewContext(), []byte("password"), []byte("somesalt"))
if err != nil {
	log.Fatal(err)
}

fmt.Printf("%x\n", hash)
~~~

### Encoded hash with custom configuration

~~~go
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
~~~
