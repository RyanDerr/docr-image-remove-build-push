# docker-hub-build-push

## Description
This workflow is designed to build the docker image of the application and push to a user's
Docker Hub Registry specified tagged with datetime and latest.

**Note**: This application uses the **DockerHub Access Token** to authenticate and authorize to push to repository on
your behalf.

## Variables

- docker-image-name
    - Desc: The name of the Docker image & repository to push (e.g. my-app)
    - Type: string
    - Required: true

## Secrets

- docker-username
    - Desc: Username of Docker Hub account to push 
    - Required: true
- docker-token
    - Desc: Access token of Docker Hub account to push
    - Required: true

## Example Workflow

```
name: Build and Push Docker Image To DockerHub Repository

on:
  push:
    branches:
      - main


jobs:
  build-and-push:
    uses: RyanDerr/reusable-workflow-templates/.github/workflows/docker-hub-build-push.yml@main
    with:
      docker-image-name: sample-image
    secrets: 
      docker-username: ${{ secrets.DOCKER_USERNAME }}
      docker-token: ${{ secrets.DOCKER_TOKEN }}
```