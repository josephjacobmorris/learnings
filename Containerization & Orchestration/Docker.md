# Docker

## Introduction

## Building Docker Images

### Caching of image layers

In Dockerfile suppose the file didnt change but its dependencies such as a copied file is changed docker is not able to identify that which results in it using the old file from the cache and the docker image wouldnt have the required behavior as we expect.


<https://stackoverflow.com/questions/75604031/docker-cache-causes-false-positive-release>