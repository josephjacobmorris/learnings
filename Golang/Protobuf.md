# Protobuf

## Steps to use protobuf for go

```bash
// This installs protoc
sudo apt install protobuf-compiler

// This installs $GOPATH/bin/protoc-gen-micro
go get github.com/micro/micro/v2/cmd/protoc-gen-micro@master

// This installs $GOPATH/bin/protoc-gen-go
go get -u github.com/golang/protobuf/protoc-gen-go

export PATH=$GOPATH/bin:$PATH
```

## Generate Code from .proto file

```bash
protoc -I <input-folder> --micro_out=<output-folder> --go_out=output-folder>  <proto file name>
```

## References

* <https://grpc.io/docs/languages/go/quickstart/>
