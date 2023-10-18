# Docker LSPHP

Docker LSPHP (LSAPI + PHP) is a collection of optimized and extensible container images for running PHP applications in
production.

All images are based on our [Docker Runtime](https://github.com/sitepilot/docker-runtime) image, an optimized and
extensible Ubuntu container image.

## Usage

This repository creates several variations of Docker images, enabling you to select precisely what you require. Just
utilize this image naming pattern in any of your projects:

```bash
ghcr.io/sitepilot/lsphp-{{variation-name}}:{{lsphp-version}}
```

For example, if you wish to run **LSPHP 8.2** with **OpenLiteSpeed**, use the following image name:

```bash
ghcr.io/sitepilot/lsphp-ols:8.1
```

## Customize an image

To customize an image and avoid potential breaking changes in your container builds, use the following image naming
pattern in your Dockerfile:

```Dockerfile
FROM ghcr.io/sitepilot/lsphp-{{variation-name}}:{{runtime-version}}-{{lsphp-version}}
```

For example, if you wish to customize the **LSPHP 8.2** with **OpenLiteSpeed** image, which is built upon
the [Runtime V1](https://github.com/sitepilot/docker-runtime/tree/1.x) image (Ubuntu 22.04 LTS), include the
following `FROM` line in your Dockerfile:

```Dockerfile
FROM ghcr.io/sitepilot/lsphp-ols:v1-8.2
```

## Variations

The following Docker image variations are available:

* LSPHP CLI - `ghcr.io/sitepilot/lsphp-cli:{{version}}`
* LSPHP & OpenLiteSpeed - `ghcr.io/sitepilot/lsphp-ols:{{version}}`

## Versions

The following LSPHP versions variations are available:

* LSPHP 7.4
* LSPHP 8.0
* LSPHP 8.1
* LSPHP 8.2

You can find a list of installed extensions for each LSPHP version [here](./src/packages).

## Environment

The following environment variables are available to modify the configuration of an image:

| Name                      | Value   | LSPHP-OLS | LSPHP-CLI |
|---------------------------|---------|-----------|-----------|
| `PHP_DATE_TIMEZONE`       | `UTC`   | ✅         | ✅         |
| `PHP_MEMORY_LIMIT`        | `256M`  | ✅         | ✅         |
| `PHP_MAX_EXECUTION_TIME`  | `300`   | ✅         | ✅         |
| `PHP_MAX_INPUT_VARS`      | `10000` | ✅         | ✅         |
| `PHP_POST_MAX_SIZE`       | `100M`  | ✅         |           |
| `PHP_UPLOAD_MAX_FILESIZE` | `100M`  | ✅         |           |
| `OLS_PUBLIC_DIR`          | -       | ✅         |           |
