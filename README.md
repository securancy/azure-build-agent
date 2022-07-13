# Azure Build Agent for Docker (Linux)
Azure DevOps Pipelines build agent, for Linux. This is a preconfigured Docker image allowing to run basic builds on Linux (Ubuntu) for the following common build scenarios:

* .NET 6
* Javascript (Node)

> The Dockerfile is originally based on instructions provided in [Run a self-hosted agent in Docker](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops). 

Additions and changes:

* ~~Use Ubuntu 22 instead of Ubuntu 20~~ (Ubuntu 22 has libssl issues)
* ~~Add Powershell support~~ (Not available on ARM64 as .deb package)
* Enable support for ArchiveFiles and ExtractFiles

## Usage instructions

### Building the image

From the root of the project, build the image:

```bash
# note: omit `--no-cache` for faster subsequent builds
$ docker build --no-cache -t securancy/azure-build-agent:latest .
```

### Running the image

```bash
$ docker run --name azure-build-agent \
    -e AZP_URL=https://dev.azure.com/<YOUR-ORGANIZATION> \
    -e AZP_TOKEN=<YOUR-ACCESS-TOKEN> \
    -e AZP_AGENT_NAME=<BUILD-AGENT-NAME> \
    -e TARGETARCH=<TARGET-ARCHITECTURE> \
    securancy/azure-build-agent:latest
```

Arguments:

* `AZP_TOKEN`: the personal access token from Azure DevOps.
* `AZP_AGENT_NAME`: display name of the agent, as it will be registered in Azure DevOps.
* `TARGETARCH`: Can be 'linux-x64', 'linux-arm64', 'linux-arm', 'rhel.6-x64'.

Optionally, to accept only one build and quit after, you can add the optional `--once

```bash
$ docker run --name azure-build-agent \
    ... removed for brevity ...
    --once
```
