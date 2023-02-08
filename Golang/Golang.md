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