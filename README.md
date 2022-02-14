This repository contains Exabel APIs.

All APIs should follow Google's [API Design Guide](https://cloud.google.com/apis/design/).
Also see the [Google AIPs](https://google.aip.dev/).

## Java

Each API folder is also a maven module that will generate Java classes of the proto buffer
definitions and proto descriptors. The `protobuf-maven-plugin` is used to automatically download
and invoke the correct version of `protoc`.

`mvn clean install` will generate source code for protocol buffers and gRPC services and compile
the sources into the the module's artifact. It will also generate protobuf descriptors to
`target/generated-resources/protobuf/descriptor-sets/descriptor.pb`.

### Plugins

We are using the following protoc plugins:

* `protoc-gen-grpc-java`: To generate Java stubs of gRPC servers and clients.
* `exabel-proto-extensions`: To generate additional setter and getter methods on proto messages.

## Prototool

This repository uses [prototool](https://github.com/uber/prototool) to format proto buffers and to
detect breaking changes. The tool is configured using [prototool.yaml].

### Installation

(Also see https://github.com/uber/prototool/blob/dev/docs/install.md.)

Mac: `brew install prototool`

Linux:
```bash
curl -sSL \
  https://github.com/uber/prototool/releases/download/v1.9.0/prototool-$(uname -s)-$(uname -m).tar.gz | \
  tar -C /usr/local --strip-components 1 -xz
```
(`sudo` may be required.)

### Linting and formatting

```bash
prototool lint
prototool format -d
```

All code must be linted and formatted before it can be submitted. You can auto-format using
```bash
prototool format -w
```

### Breaking change

```bash
prototool break check [--git-branch baseline]
```

Breaking changes should not be submitted. (This may be relaxed in the future.)

## OpenAPI v2

### Prerequisites

1. Install `go`: https://go.dev/doc/install
2. Install the protocol compiler plugins for Go (from the root of this repository):
   ```bash
   go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway \
     github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2 \
     google.golang.org/protobuf/cmd/protoc-gen-go \
     google.golang.org/grpc/cmd/protoc-gen-go-grpc
   ```
5. Update your PATH so that the protoc compiler can find the plugins:
   ```bash
   export PATH="$PATH:$(go env GOPATH)/bin"
   ```

### Genereate OpenAPIv2 specification

```bash
mkdir -p target/openapiv2
mkdir -p target/go
protoc -I . --go_out=target/go --go_opt=module=exabel.com/api/data/v1 --go-grpc_out=target/go \
  --go-grpc_opt=module=exabel.com/api/data/v1 --openapiv2_out target/openapiv2 \
  --openapiv2_opt allow_merge=true,merge_file_name=data,openapi_naming_strategy=fqn,logtostderr=true \
  exabel/api/data/v1/{data_set_,entity_,relationship_,signal_,time_series_,}service.proto
```
**Note:** This excludes the file `internal_entity_service.proto`

### Test with Swagger UI:

```bash
docker run -p 8080:8080 -e SWAGGER_JSON=/openapiv2/data.swagger.json -v ${PWD}/target/openapiv2:/openapiv2 swaggerapi/swagger-ui
```
