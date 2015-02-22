# ruby-bundle-install-docker

Use this tool to create a base docker image which have your required gems installed.

The idea is by having a docker image that is based on another docker image that have gems pre-installed. This will then make your docker build runs much faster during `bundle install` (every gems are already pre-installed).

When the base image is getting stale, simply re-run the tool to regenerate the base image.

## Usage

1. Create the base image by issuing the following command
  ```
  ./bin/base_docker_image <template file> <base image> <path to you Gemfile/Gemfile.lock> <image name> <image tag>
  ```
  ie.

  ```
  ./bin/base_docker_image '' '' ~/my-cool-ruby-project/ my-cool-ruby-project base
  ```
  * Template file is defaulted to [templates/dockerfile_base](templates/dockerfile_base)
  * Base image is defaulted to 'ruby:latest', put any ruby docker image you may wish here (bundler is assumed to be available)
  * Your Gemfile and Gemfile.lock should be located as specified by the 3rd param 
  * Note the default template runs in production mode

1. Now use the newly created base image in your "actual" Dockerfile
  ```
  FROM my-cool-ruby-project:base

  ADD . /app
  RUN bundle install
  ...
  ```
1. When the base image get stale (many gems needs to be installed), recreate the base image again by doing no 1.

## Other Usage

* The template docker file is overridable, which means you can use this tool to cache anything that your project need. Please ensure that the operation is idempotent.

Still WIP
