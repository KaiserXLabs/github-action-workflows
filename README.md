# (Reusable) Github Action Workflows

This repository contains reusable (callable) [github action workflows](https://github.com/features/actions) that help to reduce boilerplate YAML code that is needed to create and publish K8s artifacts like docker images and helm charts.

## Build and publish docker images to container registry

Workflow name: `KaiserXLabs/github-action-workflows/.github/workflows/build-and-push-docker-image.yml@main`

Usage:

```yaml
name: "Push to main"

on:
  push:
    branches:
      - main

jobs:
  release-please:
    runs-on: ubuntu-latest
    outputs:
      release_created: ${{ steps.release.outputs.release_created || false }}
      version_tag: "${{ steps.release.outputs.release_created && format('{0}.{1}.{2}',steps.release.outputs.major,steps.release.outputs.minor,steps.release.outputs.patch) || steps.package-json-version.outputs.value }}"
    steps:
      - uses: google-github-actions/release-please-action@v3
        id: release
        with:
          release-type: node
          package-name: my-service-name

      - name: Checkout code
        uses: actions/checkout@v3

      - name: grab package json version
        id: package-json-version
        uses: ashley-taylor/read-json-property-action@v1.0
        with:
          path: package.json
          property: version

  buildAndPushDockerImage:
    name: "Build and push docker image"
    needs: release-please
    uses: KaiserXLabs/github-action-workflows/.github/workflows/build-and-push-docker-image.yml@main
    with:
      repository_name: my-service-name
      registry_name: my-docker-registry.example.io
      release_created: ${{ needs.release-please.outputs.release_created == 'true' }}
      version_tag: ${{ needs.release-please.outputs.version_tag }}
    secrets:
      registry_username: ${{ secrets.REGISTRY_USERNAME }}
      registry_password: ${{ secrets.REGISTRY_PASSWORD }}
```
