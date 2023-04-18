# Digital Ocean Image Remove

Originally forked off of [docr-image-remove@v1](https://github.com/ripplr-io/docr-image-remove) by [ggraca](https://github.com/ggraca)

This action uses doctl to find and remove old images from Digital Ocean's Container Registry.

It gets the repository tags using `doctl registry repository lt` and orders them by the `UpdatedAt` attribute. 
By default, it will ignore the tag latest and the 10 most recent tags. It then deletes the older tags using `doctl registry repository dt`.

To Make sure shell script can run make sure to run `git update-index --chmod=+x [path]` on any executable script.

## Usage

Add this step to a job to automatically delete older images as part of a job:

    - name: Remove old images from Container Registry
      uses: RyanDerr/reusable-workflow-templates/.github/actions/docr-image-remove@main
      with:
        image-repository-name: image-repository # required
        image-storage-limit: 10 

## Inputs
- image-repository-name
  - Desc: Image repository name in the Container Registry
  - Required: True
- image-storage-limit
  - Desc: Number of recent images allowed in registry.
  - Required: False
  - Default: -1
    - No images will be deleted