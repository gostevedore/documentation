---
title: Image
date: "2021-04-13"
weight: 20
---

{{<toc>}}
{{< hint warning >}}
TODO
{{< /hint >}}

## General Description
An `image` it is Docker image representation. It holds the


## Keywords reference for image configuration

|Keyword|Type|Description|Value|
|---|---|---|---|
|**builder**| string/yaml | It identify which [builder]({{<ref "/getting-started/concepts/#builder" >}}) to use to create the image. It could be either an string, for [global builders]({{<ref "/reference-guide/builder/#global-builder">}}) or, in case of [in-line builders]({{<ref "/reference-guide/builder/#in-line-builder">}}), a yaml.<br>When builder is not defined, is used a default builder which does not performs any action. | - |
|**children**| yaml | Children is the list of images which starts from the current image | - |
|**name**| string | It is the image name | By default its value is defined as the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}}) name key |
|**namespace**| string| It is the docker registry namespace | - |
|**persistent_vars**| yaml | It defines a list of variables that will be passed to builder in ordre to create the Docker image. That variables are also inherited as `persistent_vars` by all its children | Its value is a key-value structure where the key is an string and its value a yaml object| - |
|**registry**| string | It is the docker registry host |
|**tags**| list | It is an extra tags list to be created for the image | - |
|**vars**| map | It defines a list of variables that will be passed to builder in ordre to create a Docker image | Its value is a key-value structure where the key is an string and its value a yaml object| - |
|**registry**| string | It is the docker registry host |
|**version**| string | It is the image main tag | By default its value is defined as the [image_tree]({{<ref "/getting-started/concepts/#image-tree">}}) version key |



{{< highlight golang "linenos=table" >}}
Builder        interface{}            `yaml:"builder"`
Children       map[string][]string    `yaml:"children"`
Childs         map[string][]string    `yaml:"childs"`
Name           string                 `yaml:"name"`
Namespace      string                 `yaml:"namespace"`
PersistentVars map[string]interface{} `yaml:"persistent_vars"`
Registry       string                 `yaml:"registry"`
Tags           []string               `yaml:"tags"`
Type           string                 `yaml:"type"`
Vars           map[string]interface{} `yaml:"vars"`
Version        string                 `yaml:"version"`
{{< /highlight>}}


Parser will use the SemVer struct to generate the template, and all SemVer attributes could be used to define each template.
{{< highlight golang "linenos=table" >}}
   // SemVer is a sematinc version representation
   type SemVer struct {
   	Major      string
   	Minor      string
   	Patch      string
   	PreRelease string
   	Build      string
   }
{{< /highlight>}}

Template variables available:
{{.Name}}
{{.Parent}}
{{.Version}}