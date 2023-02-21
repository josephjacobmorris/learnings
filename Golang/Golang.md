# Golang

## Testing

### Commands used to test

```go test```

To run the tests with coverage and get the result in html format use

```bash
go test -coverprofile=<file-name>.out
go tool cover -html <file-name>.out -o <file-name>.html
```  

Note :
The current directory should have Go files for the above to work

## JSON Marshalling

### Using easyjson library

easyjson is an open source library that generated JSON marshalling code specifically for structs. It is documented to be x4-x5 times faster than "encoding/json" library which is used by default.

Steps to generate JSON marshalling code

```bash
# Step1:
go get github.com/mailru/easyjson

# Step2:
# add _ "github.com/mailru/easyjson/gen" in the imports of model

# Step3:
easyjson -all model.go
```

## References

* <https://stackoverflow.com/questions/40587860/using-easyjson-with-golang>

* <https://github.com/mailru/easyjson>