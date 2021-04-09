---
title: Driver
date: "2020-01-31"
weight: 20
---

## General Description
A driver is a tool that knows how to manage docker images. `stevedore` could be seen as an intermediate between a docker end-user and those tools which manages docker images, and based on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}}), the [builder]({{<ref "/getting-started/concepts/#builder">}}) and the [stevedore cli]({{<ref "/reference-guide/cli">}}) command flags, a set of parameters are prepared by `stevedore` and sent to driver. When a driver start to build a docker image based on those parameters.

All drivers receives parameters defined on builder [variables-mapping]({{<ref "/reference-guide/builder/#variables-mapping-reference">}}), image [vars]({{<ref "/reference-guide/image">}}) and image [persistent_vars]({{<ref "/reference-guide/image">}})

Stevedore support the drivers listed below, follow its links to know more about each driver.
{{<toc-tree>}}
