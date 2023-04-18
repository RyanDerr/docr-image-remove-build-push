# Workflows

## Table Of Contents
- [Digital Ocean Container Registry Clean-Build-Push](#docr-clean-build-push)

## docr-clean-build-push

### Description
This workflow is designed to clean up (delete) older images after a provided limit of images is allowed to be stored in the Digital Ocean Container Registry
built on top of [docr-image-remove@v1](https://github.com/ripplr-io/docr-image-remove).
By default, the max limit is ten, though this value can be overwritten by the `image-store-limit` variable. One completed the workflow will then build the docker
image of the application and push to a Digital Ocean Container Registry specified tagged with datetime and latest.

**Note**: This application uses the **Digital Ocean API Token** to authenticate and authorize your Container Registry.

### Variables

- image-registry-name
    - Desc: The name of the digital ocean registry
    - Type: string
    - Required: true
- image-name
    - Desc: The name of the Docker image to push (e.g. my-app)
    - Type: string
    - Required: true
- image-store-limit
    - Desc: The maximum number of images to keep in the container registry (e.g. 10)
    - Type: number
    - Required: false
    - Default: -1
        - Does not remove any images
- docker-path
    - Desc: Relative path to Dockerfile within codebase
    - Type: string
    - Required: false
    - Default: .

### Secrets

- container-registry-token:
    - Desc: API token to authenticate and authorize Digital Ocean Container Registry
    - Required: true

### Example Workflow

```
name: Build and Push Docker Image To Digital Ocean Container Registry

on:
  push:
    branches:
      - main


jobs:
  clean_build_push:
    uses: RyanDerr/reusable-workflow-templates/.github/workflows/docr-clean-build-push.yml@main
    with:
      image-registry-name: sample
      image-name: sample-image
      image-store-limit: 5
    secrets: 
      container-registry-token: ${{ secrets.DIGITALOCEAN_TOKEN }}
```

### Notes

While this application does delete older images it will run garbage collection to purge unused data from the
Container Registry, this can cause locking allowing only read access of the registry until purge is complete.
This can take up to 15 mins and may cause future pipelines to fail until complete.