This repository contains Exabel APIs.

All APIs should follow Google's [API Design Guide](https://cloud.google.com/apis/design/).

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
```
curl -sSL \
  https://github.com/uber/prototool/releases/download/v1.9.0/prototool-$(uname -s)-$(uname -m).tar.gz | \
  tar -C /usr/local --strip-components 1 -xz
```
(`sudo` may be required.)

### Linting and formatting

```
prototool lint
prototool format -d
```

All code must be linted and formatted before it can be submitted. You can auto-format using
```
prototool format -w
```

### Breaking change

```
prototool break check [--git-branch baseline]
```

Breaking changes should not be submitted. (This may be relaxed in the future.)
