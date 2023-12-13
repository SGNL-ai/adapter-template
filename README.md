# SGNL Ingestion Adapter Template

The SGNL Ingestion Adapter Template is the starting point for creating a new SGNL Ingestion Adapter.

## Prerequisites

- A basic understanding of the Golang programming language.
- An understanding of the [gRPC](https://grpc.io/) framework and [protocol buffers](https://protobuf.dev/).

### Terminology

**Adapter** - A simple gRPC server that queries an external API and parses the response into a format suitable for the SGNL ingestion service. More information on adapters can be found [TODO].

**SGNL ingestion service** - One of SGNL's core microservices which is responsible for ingesting external data into SGNL's graph database.

**System of Record (SoR)** - An external system that provides data to be ingested into SGNL, typically via an API. For example, a CRM tool like Salesforce or HubSpot.

## Background Information

The adapter is a gRPC server which has two main responsibilities:

1. Making requests to a System of Record (SoR) to retrieve data.
2. Transforming the response from the SoR into a format that can be consumed by the SGNL ingestion service.

The adapter server is **stateless**. It simply acts as a proxy to send requests to SoRs and parse the responses.

Requests to the adapter server invoke the `GetPage` method, which is the main entry point.

TODO: Add auth information.

## Getting Started

1. Clone this repository.
1. Update the names of `github.com/sgnl-ai/adapter-template/*` Golang packages in all files to match your new repository's name (e.g. `github.com/your-org/your-repo`):

   ```
   sed -e 's,^module github\.com/sgnl-ai/adapter-template,github.com/your-org/your-repo,' -i go.mod
   ```

   ```
   find pkg/ -type f -name '*.go' | xargs -n 1 sed -n -e 's,github\.com/sgnl-ai/adapter-template,github.com/your-org/your-repo,p' -i
   ```

1. Modify the adapter implementation in package `pkg/adapter` to query your datasource. All the code that must be modified is identified with `SCAFFOLDING` comments.
1. Build the Docker image with the `adapter` command.
   ```
   docker build -t adapter:latest .
   ```
1. **WARNING: Temporary workaround to allow `go` to download the adapter-framework module before it is made public.**
   **The image's history will contain your environment variables, incl. any GitHub credentials that it may contain.**
   ```
   docker build -t adapter:latest --build-arg GITHUB_USER="$GITHUB_USER" --build-arg GITHUB_TOKEN="$GITHUB_TOKEN" .
   ```
1. Run the adapter server as a Docker container.
   ```
   docker run --rm -it adapter:latest
   ```
