# (Reusable) Github Action Workflows

This repository contains reusable (callable) [github action workflows](https://github.com/features/actions) that help to reduce boilerplate YAML code that is needed to create and publish K8s artifacts like docker images and helm charts.

## Build and publish docker images

Builds a `Dockerfile` and pushes the built container to the passed repository at the passed container registry.

### Workflow name

<!-- x-release-please-start-version -->

`KaiserXLabs/github-action-workflows/.github/workflows/build-and-push-docker-image.yml@v1.0.0`

<!-- x-release-please-end -->

### Inputs

| Input                     | Type    | Required                       | Description                                                                                                            |
| ------------------------- | ------- | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `registry_name`           | string  | yes                            | The name of the container registry, for example `ghcr.io/kaiserxlabs`                                                  |
| `repository_name`         | string  | yes                            | The name of the repository at the container registry, for example `hello-world`                                        |
| `release_created`         | boolean | yes                            | Whether this workflow should push a pre-release using the short SHA as image tag or push a release using `version_tag` |
| `version_tag`             | string  | only if release_created is yes | The version tag if `release_created` is `true`                                                                         |
| `dockerfile_path`         | string  | no                             | Optional `Dockerfile` file path relative to the repository root. Default value is `./Dockerfile`                       |
| `dockerfile_context_path` | string  | no                             | Optional context path relative to the repository root. Default value is `.`                                            |

### Secrets

| Input               | Required | Description                           |
| ------------------- | -------- | ------------------------------------- |
| `registry_username` | yes      | The container registry login user     |
| `registry_password` | yes      | The container registey login password |

### Outputs

| Input       | Always | Description                                                                                       |
| ----------- | ------ | ------------------------------------------------------------------------------------------------- |
| `image_tag` | yes    | Either the short SHA if `release_created` is `false` or the `version_tag` (see [Inputs](#inputs)) |

### Usage

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
    uses: KaiserXLabs/github-action-workflows/.github/workflows/build-and-push-docker-image.yml@v1.0.0 # x-release-please-version
    with:
      repository_name: hello-world
      registry_name: ghcr.io/${{ github.repository_owner }}
      release_created: ${{ needs.release-please.outputs.release_created == 'true' }}
      version_tag: ${{ needs.release-please.outputs.version_tag }}
      version_tag: ${{ needs.release-please.outputs.version_tag }}
      dockerfile_path: hello-world/Dockerfile
      dockerfile_context_path: hello-world
    secrets:
      registry_username: ${{ secrets.REGISTRY_USERNAME }}
      registry_password: ${{ secrets.REGISTRY_PASSWORD }}
```

## Package and publish helm charts

### Workflow name

<!-- x-release-please-start-version -->

`KaiserXLabs/github-action-workflows/.github/workflows/package-and-push-helm-chart.yml@main`

<!-- x-release-please-end -->

### Usage

```yaml
name: "Push to main"

on:
  push:
    branches:
      - main

jobs:
  release-please:
    # see above what this job is doing in detail

  packageAndPushHelmChart:
    name: "Build and push helm chart"
    needs: release-please
    uses: KaiserXLabs/github-action-workflows/.github/workflows/package-and-push-helm-chart.yml@v1.0.0 # x-release-please-version
    with:
      registry_name: my-docker-registry.example.io # required
      release_created: ${{ needs.release-please.outputs.release_created == 'true' }} # required
      version_tag: ${{ needs.release-please.outputs.version_tag }} # required
      registry_url_path: /my-helm-charts # optional, default: /helm
      image_tag_value_property_path: image.tag # optional, default: global.image.tag
      helm_chart_root_path: ./my-helm-chart # optional, default: ./charts

    secrets:
      registry_username: ${{ secrets.REGISTRY_USERNAME }}
      registry_password: ${{ secrets.REGISTRY_PASSWORD }}
```
