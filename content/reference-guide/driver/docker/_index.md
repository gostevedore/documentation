---
title: Docker
date: "2021-04-13"
weight: -20
---

## Driver overview
To create a docker image using `docker` driver it used [go client for the Docker Engine API](https://pkg.go.dev/github.com/docker/docker/client?utm_source=godoc#section-documentation). Instead of using the Docker API directly, it is used through [go-docker-builder](https://github.com/apenella/go-docker-builder) which wraps some use cases, such as context tar generation.

