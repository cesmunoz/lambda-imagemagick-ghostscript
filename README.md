# Lambda AWS - ImageMagick Ghostscript

Scripts to compile ImageMagick utilities for AWS Lambda instances powered by Amazon Linux 2.x, such as the `nodejs10.x` runtime, and the updated 2018.03 Amazon Linux 1 runtimes.

Amazon Linux 2 instances for Lambda no longer contain system utilities, so `convert`, `mogrify`, `identify` from the [ImageMagick](https://imagemagick.org) and `gs` from [Ghostscript](https://www.ghostscript.com/) package are no longer available.

## Usage

## Prerequisites

- Docker desktop
- Unix Make environment
- AWS command line utilities (just for deployment)

## Compiling the code

- start Docker services
- `make clean`
- `make build result`
- `make all`

There are two `make` scripts in this project.

- [`Makefile`](Makefile) is intended to run on the build system, and just starts a Docker container matching the AWS Linux 2 environment for Lambda runtimes to compile ImageMagick using the second script.
- [`Makefile_layer`](Makefile_layer) is the script that will run inside the container, and actually compile binaries.

The output will be in the `result` dir.

### Configuring the build

By default, this compiles a version expecting to run as a Lambda layer from `/opt`. You can change the expected location by providing a `TARGET` variable when invoking `make`.

The default Docker image used is `lambci/lambda-base-2:build`. To use a different base, provide a `DOCKER_IMAGE` variable when invoking `make`.

Modify the versions of libraries for ImageMagick or Ghostscript directly in [`Makefile_layer`](Makefile_layer).

### Bundled libraries

This is not a full-blown ImageMagick setup you can expect on a regular Linux box, it's a slimmed down version to save space that works with the most common formats. You can add more formats by including another library into the build process in [`Makefile_layer`](Makefile_layerñ).

These libraries are currently bundled:

- libpng
- libtiff
- libjpeg
- openjpeg2
- libwebp

## Deploying to AWS as a layer

Run the following command to deploy the compiled result as a layer in your AWS account.

```
make deploy DEPLOYMENT_BUCKET=<YOUR BUCKET NAME>
```

### configuring the deployment

By default, this uses imagemagick-layer as the stack name. Provide a `STACK_NAME` variable when
calling `make deploy` to use an alternative name.

## Info on scripts

For more information, check out:

- https://imagemagick.org/script/install-source.php
- http://www.linuxfromscratch.org/blfs/view/cvs/general/imagemagick.html
- https://www.ghostscript.com/doc/current/Install.htm

## Author

Cesar Muñoz

## License

- These scripts: [MIT](https://opensource.org/licenses/MIT)
- ImageMagick: https://imagemagick.org/script/license.php
- Ghostscript: https://www.ghostscript.com/license.html
- Contained libraries all have separate licenses, check the respective web sites for more information
