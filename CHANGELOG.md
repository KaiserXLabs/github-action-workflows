# Changelog

## [2.2.0](https://github.com/KaiserXLabs/github-action-workflows/compare/v2.1.1...v2.2.0) (2022-07-28)


### Features

* **ci:** add major and minor version tagging when releasing ([#8](https://github.com/KaiserXLabs/github-action-workflows/issues/8)) ([4652691](https://github.com/KaiserXLabs/github-action-workflows/commit/4652691d7f4731c9d6d1bf235acd4337c9091a0c))

## [2.1.1](https://github.com/KaiserXLabs/github-action-workflows/compare/v2.1.0...v2.1.1) (2022-07-27)


### Bug Fixes

* output values not set ([#6](https://github.com/KaiserXLabs/github-action-workflows/issues/6)) ([a23866a](https://github.com/KaiserXLabs/github-action-workflows/commit/a23866a5ad922440a6837287d0a5c89ce562570c))

## [2.1.0](https://github.com/KaiserXLabs/github-action-workflows/compare/v2.0.0...v2.1.0) (2022-07-26)


### Features

* **helm:** add `chart_tag` output ([#4](https://github.com/KaiserXLabs/github-action-workflows/issues/4)) ([9cdc42b](https://github.com/KaiserXLabs/github-action-workflows/commit/9cdc42b48e585769a1f089b56bde2a5fa45b6191))

## [2.0.0](https://github.com/KaiserXLabs/github-action-workflows/compare/v1.0.0...v2.0.0) (2022-07-25)


### ⚠ BREAKING CHANGES

* container images are only tagged with the determined image_tag

### Features

* add image_tag output to build and push docker image ([#2](https://github.com/KaiserXLabs/github-action-workflows/issues/2)) ([7c40fab](https://github.com/KaiserXLabs/github-action-workflows/commit/7c40fabff8066cab0a135ffb882b63c0a744c504))
* **ci:** add auto versioning to readme ([8c5cdc5](https://github.com/KaiserXLabs/github-action-workflows/commit/8c5cdc5a3f2124fcb2fd308b473e1af3464d43e0))
* container images are only tagged with the determined image_tag ([7c40fab](https://github.com/KaiserXLabs/github-action-workflows/commit/7c40fabff8066cab0a135ffb882b63c0a744c504))

## 1.0.0 (2022-07-14)


### Features

* **ci:** add simple release please ([a660248](https://github.com/KaiserXLabs/github-action-workflows/commit/a6602480af15652ccc3c5e44f0cc3408dd75e11d))
* **helm:** add optional helm_chart_root_path input ([2865c4e](https://github.com/KaiserXLabs/github-action-workflows/commit/2865c4e2c7d616ae4efcbfd2d7ab577ee6130e9a))


### Bug Fixes

* **helm:** default registry_url_path ([7c8c2d6](https://github.com/KaiserXLabs/github-action-workflows/commit/7c8c2d6000a380e15b80a0a6f153adde3960961f))
* workflow files location ([ec2b2b1](https://github.com/KaiserXLabs/github-action-workflows/commit/ec2b2b15986eac711ef65c7317af03d0294ecbb9))
