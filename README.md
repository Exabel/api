This repository contains Exabel APIs.

All APIs should follow Google's [API Design Guide](https://cloud.google.com/apis/design/).

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
