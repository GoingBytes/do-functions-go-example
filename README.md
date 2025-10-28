# DigitalOcean Functions Go Example

A sample project demonstrating how to build and deploy serverless functions on DigitalOcean using Go. This repository contains a simple "Hello World" function that showcases the basic structure and deployment process for DigitalOcean Functions.

## Overview

This project includes a sample serverless function written in Go that accepts a name parameter and returns a personalized greeting. It demonstrates:

- Go function structure for DigitalOcean Functions
- Request/Response handling with JSON
- Error handling and validation
- Logging with logrus
- Testing serverless functions
- Deployment configuration

## Project Structure

```
.
├── packages/
│   └── sample/
│       └── hello/
│           ├── hello.go       # Main function implementation
│           ├── hello_test.go  # Unit tests
│           ├── go.mod         # Go module dependencies
│           └── go.sum         # Dependency checksums
├── project.yml                # DigitalOcean Functions project configuration
├── app-spec.yaml             # DigitalOcean App Platform specification
├── go.work                   # Go workspace configuration
└── README.md                 # This file
```

## Prerequisites

Before you begin, ensure you have the following installed:

- [Go 1.20](https://golang.org/dl/) or later (CI uses Go 1.25.1 for testing)
- [DigitalOcean CLI (`doctl`)](https://docs.digitalocean.com/reference/doctl/how-to/install/)
- A [DigitalOcean account](https://cloud.digitalocean.com/registrations/new) with Functions enabled
- Git

## Installation

1. Clone the repository:

```bash
git clone https://github.com/GoingBytes/do-functions-go-example
cd do-functions-go-example
```

2. Install DigitalOcean Functions support:

```bash
doctl serverless install
```

3. Connect to your DigitalOcean account (if not already connected):

```bash
doctl auth init
```

## Usage

### Deploy the Function

Deploy the function to DigitalOcean:

```bash
doctl serverless deploy . --verbose-build --remote-build
```

After deployment, `doctl` will output the function's URL.

### Invoke the Function

Once deployed, you can invoke the function using the provided URL:

```bash
# Using curl with a valid name
curl -X POST https://your-namespace.doserverless.co/api/v1/web/sample/hello \
  -H "Content-Type: application/json" \
  -d '{"name": "World"}'

# Expected response:
# {"statusCode":200,"body":"Hello, World"}
```

```bash
# Without providing a name (error case)
curl -X POST https://your-namespace.doserverless.co/api/v1/web/sample/hello \
  -H "Content-Type: application/json" \
  -d '{}'

# Expected response:
# {"statusCode":400,"body":"no name provided"}
```

### Function API

**Request Format:**

```json
{
  "name": "string"
}
```

**Response Format:**

```json
{
  "statusCode": 200,
  "headers": {},
  "body": "string"
}
```

**Parameters:**

- `name` (string, required): The name to include in the greeting

**Status Codes:**

- `200` - Success: Returns a greeting message
- `400` - Bad Request: No name provided

## Development

### Running Tests

Run the unit tests locally:

```bash
cd packages/sample/hello
go test -v
```

Run tests with race detection:

```bash
go test -race -v hello.go hello_test.go
```

### Local Development

To work on the function locally:

1. Navigate to the function directory:

```bash
cd packages/sample/hello
```

2. Install dependencies:

```bash
go mod download
```

3. Make your changes to `hello.go`

4. Run tests to verify your changes:

```bash
go test -v
```

### Adding Dependencies

To add new Go dependencies:

```bash
cd packages/sample/hello
go get <package-name>
go mod tidy
```

## Configuration

### project.yml

Defines the function configuration for DigitalOcean Functions:

- **Package**: `sample`
- **Action**: `hello`
- **Runtime**: `go:1.20`

### app-spec.yaml

Configures the DigitalOcean App Platform deployment:

- **Name**: `go-hello`
- **Region**: `nyc`
- **Deploy on Push**: Enabled for `master` branch (Note: CI workflow uses `main` branch)

## Continuous Integration

This project uses GitHub Actions for automated testing. The workflow:

- Runs on push and pull requests to the `main` branch
- Sets up Go 1.25.1
- Executes unit tests with race detection

## Troubleshooting

**Function deployment fails:**
- Ensure you're authenticated with `doctl auth init`
- Check that Functions are enabled in your DigitalOcean account
- Verify your `project.yml` configuration is valid

**Tests fail:**
- Ensure you're using Go 1.20 or later
- Run `go mod download` to fetch dependencies
- Check that all dependencies are properly installed

## Resources

- [DigitalOcean Functions Documentation](https://docs.digitalocean.com/products/functions/)
- [DigitalOcean Functions Quickstart](https://docs.digitalocean.com/products/functions/quickstart/)
- [Go Documentation](https://golang.org/doc/)
- [doctl Documentation](https://docs.digitalocean.com/reference/doctl/)

## License

This project is provided as an example for educational purposes.
