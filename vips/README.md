# libvips builder

This builder will create `.deb` packages for any released [`libvips`](https://github.com/libvips/libvips) version.

## Prerequisites

You need to have Docker installed for this builder to run.

## Usage

Simple run `make` in this directory. This will create a Docker container, install every build dependency in it, and then build `libvips`.

Once `libvips` is build, it will create a `.deb` package using `fpm` and copy it from the container to the `./dist` directory.

## Options

You can specify various arguments to the docker build. For this, set the `DOCKER_BUILD_ARGS` variable when running make.

For example: `make DOCKER_BUILD_ARGS="MAKE_OPTIONS=-j3 VIPS_VERSION=8.7.2"`

Available options:


| Variable name | Default value | Meaning |
| --- | --- | --- |
| VIPS_VERSION | `8.7.2` | The `libvips` version to compile. It is fetched from the [`libvips` releases page](https://github.com/libvips/libvips/releases). |
| DEV_DEPS | `libjpeg62-turbo-dev libpng-dev libwebp-dev libgif-dev libtiff-dev libpango1.0-dev liborc-0.4-dev liblcms2-dev libexif-dev zlib1g-dev` | Those packages will be installed before building VIPS. This allows you to add or remove support for input/output formats in the final build. If a package installed, `libvips` will be build with support for this format. You can find a [list of optional `libvips` dependencies in its README](https://github.com/libvips/libvips#optional-dependencies). |
| MAKE_OPTIONS | - | Specify custom options for `make` here. For example, `-j4` to speed up the build if you have multiple cores available in your Docker environment. |
| DEBIAN_VERSION | `stable` | The debian version to use. The builder will use the `debian:${DEBIAN_VERSION}-slim` docker base image. |
| MAINTAINER | `Renaud Chaput <renchap@gmail.com>` | The maintainer field in the resulting package |
