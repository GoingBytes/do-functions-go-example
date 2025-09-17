# `do-functions-go-example`

## Install

```bash
git clone https://github.com/egorsmkv/do-func-golang-example

doctl serverless install

doctl serverless deploy do-func-golang-example --verbose-build --remote-build
```

## Development

### Lint

```bash
golangci-lint run packages/sample/hello
```

### Test

```bash
gotestsum -- ./packages/sample/hello
```
