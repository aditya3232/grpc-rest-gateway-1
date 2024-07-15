## Persiapan protoc-gen

1. Buat folder proto/{nama data} misal proto/user
2. Isi dengan file proto
3. Tentukan nama package, option go_package, message req & resp, dan service nya
4. Hapus file protogen jika dilakukan compile ulang

## Persiapan protoc-gen-grpc-gateway:

1. Download protoc-gen--grpc-gateway dari repo berikut: https://github.com/grpc-ecosystem/grpc-gateway/releases/tag/v2.20.0
2. Rename ke protoc-gen-grpc-gateway.exe (windows)
3. Tambahkan environment variables folder tempat protoc-gen-grpc-gateway.exe berada
4. Cek dengan protoc-gen-grpc-gateway --version
5. Buat folder proto/google/api
6. Download google/api/annotations.proto copy filenya dan masukkan ke proto/google/api/annotations.proto
7. Lakukan juga google/api/http.protocopy file dan masukkan ke proto/google/api/http.proto
8. Buat konfigurasi eksternal grpc-gateway/config.yml
9. Pakai protoc-gen-grpc-gateway untuk membuat rest proxy didalam folder protogen/gateway/go
10. Jangan lupa buat direktori ini terlebih dahulu ***protogen/gateway/go***
11. Hapus protogen/gateway/go jika dilakukan compile ulang

## Persiapan protoc-gen-openapi-v2

1. Download protoc-gen-openapiv2 dari repo berikut: https://github.com/grpc-ecosystem/grpc-gateway/releases/tag/v2.20.0
2. Rename ke protoc-gen-openapiv2.exe (windows)
3. Tambahkan environment variables folder tempat protoc-gen-openapiv2.exe berada
4. Cek dengan protoc-gen-openapiv2 --version
5. Buat konfigurasi eksternal grpc-gateway/config-openapi.yml
6. Buat folder proto/protoc-gen-openapiv2/options
7. Download protoc-gen-openapiv2/options/anntations.proto dan masukkan ke proto/protoc-gen-openapiv2/options/annotations.proto
8. Download protoc-gen-openapiv2/options/openapiv2.proto dan masukkan ke proto/protoc-gen-openapiv2/options/openapiv2.proto
6. buat folder ini ***protogen/gateway/openapiv2***
8. Hapus protogen/gateway/openapiv2 jika dilakukan compile ulang

### Install Plugin Protocol Buffers Go
```shell
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```

### Install Plugin Protocol Buffers Go GRPC

```shell
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

### Run Generate Proto to Go GRPC

```shell
protoc --go_opt=module={module_name} --go_out=. ./proto/*.proto
protoc --go-grpc_opt=module={module_name} --go-grpc_out=. ./proto/*.proto

# example:
protoc --go_opt=module=grpc-rest-gateway-1 --go_out=. ./proto/user/*.proto
protoc --go-grpc_opt=module=grpc-rest-gateway-1 --go-grpc_out=. ./proto/user/*.proto
```

### protoc-gen-grpc-gateway
```shell
protoc -I . \
    --go_out ./gen/go/ --go_opt paths=source_relative \
    --go-grpc_out ./gen/go/ --go-grpc_opt paths=source_relative \
    your/service/v1/your_service.proto

example:

protoc -I . \
	--grpc-gateway_out ./protogen/gateway/go \
	--grpc-gateway_opt logtostderr=true \
	--grpc-gateway_opt paths=source_relative \
	--grpc-gateway_opt grpc_api_configuration=./grpc-gateway/config.yml \
	--grpc-gateway_opt standalone=true \
	--grpc-gateway_opt generate_unbound_methods=true \
	./proto/user/*.proto \

example single line:

protoc -I . --grpc-gateway_out ./protogen/gateway/go --grpc-gateway_opt logtostderr=true --grpc-gateway_opt paths=source_relative --grpc-gateway_opt grpc_api_configuration=./grpc-gateway/config.yml --grpc-gateway_opt standalone=true --grpc-gateway_opt generate_unbound_methods=true ./proto/user/*.proto

```

### protoc-openapiv2-gateway
```bash
example:

protoc -I . --openapiv2_out ./protogen/gateway/openapiv2 \
	--openapiv2_opt logtostderr=true \
	--openapiv2_opt output_format=yaml \
	--openapiv2_opt grpc_api_configuration=./grpc-gateway/config.yml \
  --openapiv2_opt openapi_configuration=./grpc-gateway/config-openapi.yml \
	--openapiv2_opt generate_unbound_methods=true \
	--openapiv2_opt allow_merge=true \
	--openapiv2_opt merge_file_name=merged \
  ./proto/user/*.proto \

  example single line: 

protoc -I . --openapiv2_out ./protogen/gateway/openapiv2 --openapiv2_opt logtostderr=true --openapiv2_opt output_format=yaml --openapiv2_opt grpc_api_configuration=./grpc-gateway/config.yml --openapiv2_opt openapi_configuration=./grpc-gateway/config-openapi.yml --openapiv2_opt generate_unbound_methods=true --openapiv2_opt allow_merge=true --openapiv2_opt merge_file_name=merged ./proto/user/*.proto


```

### Install Golang Grpc

```shell
go get google.golang.org/grpc
```

### Install Dependency

```bash
go mod tidy
```

### Run Program (server/ gateway)

```bash
go run cmd/main.go
```