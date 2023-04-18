# docr-clean-build-push

## Description
This workflow is designed to clean up (delete) older images after a provided limit of images is allowed to be stored in the Digital Ocean Container Registry
built on top of [docr-image-remove@v1](https://github.com/ripplr-io/docr-image-remove).
By default, the max limit is ten, though this value can be overwritten by the `image-store-limit` variable. One completed the workflow will then build the docker
image of the application and push to a Digital Ocean Container Registry specified tagged with datetime and latest.

**Note**: This application uses the **Digital Ocean API Token** to authenticate and authorize your Container Registry.

## Variables

- image-registry-name
    - description: The name of the digital ocean registry
    - type: string
    - required: true
- image-name
    - description: The name of the Docker image to push (e.g. my-app)
    - type: string
    - required: true
- image-store-limit
    - description: The maximum number of images to keep in the container registry (e.g. 10)
    - type: number
    - required: false
    - default: -1
        - Does not remove any images
- docker-path
    - description: Relative path to Dockerfile within codebase
    - type: string
    - required: false
    - default: .

## Secrets

- container-registry-token:
    - description: API token to authenticate and authorize Digital Ocean Container Registry
    - required: true

## Example Workflow

```
name: Build and Push Docker Image To Digital Ocean Container Registry

on:
  push:
    branches:
      - main


jobs:
  clean_build_push:
    uses: RyanDerr/reusable-workflow-templates/.github/workflows/docr-clean-build-push/docr-clean-build-push.yml@main
    with:
      image-registry-name: sample
      image-name: sample-image
      image-store-limit: 5
    secrets: 
      container-registry-token: ${{ secrets.DIGITALOCEAN_TOKEN }}
```

## Notes

While this application does delete older images it will run garbage collection to purge unused data from the
Container Registry, this can cause locking allowing only read access of the registry until purge is complete.
This can take up to 15 mins and may cause future pipelines to fail until complete.