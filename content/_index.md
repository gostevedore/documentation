---
title: Documentation
date: "2020-01-31"
weight: 20
---

Stevedore, a way to manage and govern the Docker image's building process

### What is Stevedore?

Stevedore is a tool to build Docker images. Is not a Dockerfile's alternative, but how to use them to build your images. 
What stevedore does is accompany you during Docker image's building process when you manage a bunch of images, and it does through an **Stevedore** `driver`.

A `driver` is the component which is in charge of running the build. There are currently two kind of drivers: **Docker API** and **ansible-playbook**. You can configure its behavior defining `builder`.

### Why stevedore?

Stevedore is a useful tool when you need to manage a bunch of Docker images in a standardized way, such on a microservices architecture. It lets you to define how to build your Docker images and their parent-child relationship. It builds automatically the children images when parent ones are done. In case your are managing your tags using semver, it is possible to generate automatically several tags following the version tree, even you can configure which tags to create.
Stevedore could also store you private registry credentials and login to it behalf you during the CI/CD processes.

## Features

#### Automate
Define docker images relationship and automate its building process
{{< hint info >}}
 **_use case:_** You have an image shipped with a custom setup or user  and that image is configured as image `FROM` on all your microservices. Then every time you rebuild that image to include new security patches, all images that are depends to it are automatically built
{{< /hint >}}

#### Standardize
Standardize and reuse Docker's images definition
{{< hint info >}}
**_use case:_** You define all your microservices based on the same skeleton, with the same way of testing or building them.
Then, you could define one Dockerfile and reuse it to generate an image for any of those microservices
{{< /hint >}}

#### Semver tags
Generate automatically semver tags when main tag is semver 2.0.0 compliance
{{< hint info >}}
**_use case:_** Supose that you are tagging your images using semantic versioning. In that case you could generate automatically new tags based on semver tree

_example:_
_You have the tag `2.1.0`, then tags `2` and `2.1` are also generated._
{{< /hint >}}

#### Credentials
Centralized Docker registry's credentials store
{{< hint info >}}
**_use case:_** Supose that your Docker registry needs to be secured and should only be accessed by continuous delivery procedures. Then, `stevedore` could be authenticated on your behalf to push any created image to a Docker registry
{{< /hint >}}

#### Promote
Promote images to another Docker registry or to another registry namespace
{{< hint info >}}
**_use case:_** Your production environment only accept to pull images from specific Docker registry, and that docker registry is only used by your production environment. Supose that you have built and pushed an image to your staging Docker registry and you have passed all your end to end test. Then, you can promote the image from your staging Docker registry to your production one
{{< /hint >}}
