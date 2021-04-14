---
title: Driver
date: "2020-01-31"
weight: 20
---

## General Description
A driver is a tool that already knows how to build docker images. `stevedore` could be seen as an intermediate between docker end-user and those tools which could build docker images. Based on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}}), the [builder]({{<ref "/getting-started/concepts/#builder">}}) options and the [stevedore cli]({{<ref "/reference-guide/cli">}}) command flags, `stevedore` prepares all required parameters that defines the Docker image and passes them to driver.

All drivers receives parameters defined on builder [variables-mapping]({{<ref "/reference-guide/builder/#variables-mapping-reference">}}), image [vars]({{<ref "/reference-guide/image/#keywords-reference-for-image-configuration">}}) and image [persistent_vars]({{<ref "/reference-guide/image/#keywords-reference-for-image-configuration">}})

Stevedore supports the drivers listed below. Follow its links to know more about each driver.
{{<toc-tree>}}
