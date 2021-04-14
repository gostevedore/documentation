---
title: Image
date: "2021-04-14"
weight: 20
---

{{<toc>}}

## General Description
`stevedore`represents the Docker image definition by `image`, then an `image`, on `stevedore` context, holds information to required to build the image.
Note that an `image` by itself it has no sense, it must belong to [image_tree]({{<ref "/getting-started/concepts/#image-tree">}}).

## Keywords reference for image configuration

|Keyword|Type|Description|Value|
|---|---|---|---|
|**builder**| string/yaml | Identify which [builder]({{<ref "/getting-started/concepts/#builder" >}}) to use in order to create the image. It could be either an string, for [global builders]({{<ref "/reference-guide/builder/#global-builder">}}) or, in case of [in-line builders]({{<ref "/reference-guide/builder/#in-line-builder">}}), a yaml.<br>When builder is not defined, is used a default builder which does not performs any action. | - |
|**children**| map | Children is a map where each key is the image name of an [image_tree]({{<ref "/getting-started/concepts/#image-tree">}})'s image and the value is a list of image versions | - |
|**name**| string | It is the image name | By default its value is defined as the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}})'s image name key |
|**namespace**| string| It is the docker registry namespace | - |
|**persistent_vars**| map | It defines a list of variables that will be passed to builder in ordre to create the Docker image. That variables are also inherited as `persistent_vars` by all its children | Its value is a key-value structure where the key is an string and its value a yaml object| - |
|**registry**| string | It is the docker registry host |
|**tags**| list | It is an extra tags list to be created for the image | - |
|**vars**| map | It defines a list of variables that will be passed to builder in ordre to create a Docker image | Its value is a key-value structure where the key is an string and its value a yaml object| - |
|**registry**| string | It is the docker registry host |
|**version**| string | It is the image main tag | By default its value is defined as the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}})'s image version key |

- Example
{{<highlight Yaml "linenos=table">}}
  name: ubuntu
  version: 18.04
  builder:
    driver: docker
    options:
    	context:
  	  path: ./ubuntu
  namespace: stable
  registry: myregistry.example.com
  tags:
    - latest
  persistent_vars:
    ubuntu_version: 18.04
  vars:
    os_base: rootfs.tar
  children:
    app1:
      - "1.2.3"
      - "*"
    app2:
      - "0.3.1"
      - "*"
{{</highlight>}}


## Templating image attributs

When you define an `image` you could set it attributs with data comming from the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}}).
The available

|Name|Template|Description|
|---|---|---|
| **name** | {{ .Name }} | It gives the image's name on the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}}) |
| **version** | {{ .Version }} | It gives the image's version on the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}}) |
| **parent** | {{ .Parent }} | It let you to get the template attributes from the image's parent on the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}})|

{{<highlight Yaml "linenos=table">}}
  name: ubuntu
  version: "{{ .Version }}"
  builder:
    driver: docker
    options:
    	context:
  	  path: ./ubuntu
  namespace: stable
  registry: myregistry.example.com
  tags:
    - "{{ .Version }}-{{ .Parent.Version }}"
  persistent_vars:
    ubuntu_version: "{{ .Version }}"
{{</highlight>}}
