---
title: Builder
weight: 20
---

{{<toc>}}

## General Description
A builder holds those parameters required by a driver to proceed with the Docker's image building request, such the build context or its Dockerfile location, for instance. A builder is defnied by [`YAML`](https://en.wikipedia.org/wiki/YAML) data structure, following the specifications described below, on [Keywords reference for builders configuration]({{<ref "/reference-guide/builder/#keywords-reference-for-builder-configuration">}}).

Builders can be defined as `global`, and could be used by any image, in that case the builder must be placed under the `builders` block on [Stevedore configuration]({{<ref "/getting-started/configuration">}}), or can be also defined inside the image definition, and it is known as `in-line` builder.

### Global builder
Global builders are those builders that are defined under the `builders` block on [Stevedore configuration]({{<ref "/getting-started/configuration">}}) and can be used by any image on the images tree. 

To include a `global` builder on [Stevedore configuration]({{<ref "/getting-started/configuration">}}), it must be edited the file specified in [builder_path]({{<ref "/getting-started/configuration/#builder_path">}}) configuration parameter, and place there the new definition, under the `builders`.

For example, when `builder_path` is set as below:
{{<highlight Yaml "linenos=table">}}
builder_path: stevedore_builders.yaml
{{</highlight>}}

It must be edited the file `stevedore_builders.yaml` to include there the new builder. On next example are defined builder1, builder2 and builder3.
{{<highlight Yaml "linenos=table">}}
builders:
    builder1:
        driver: docker
        options:
            context:
                path: ./my-apps-golang
    builder2:
        driver: docker
        options:
            context:
                path: ./my-apps-phyton
    builder2:
        driver: ansible-playbook
        options:
            playbook: my-apps-base/site.yml
            inventory: my-apps-base/all
{{</highlight>}}

The [builder_path]({{<ref "/getting-started/configuration/#builder_path">}}) value could be also achieved through Stevedore CLI.
```bash
$ stevedore get configuration
PARAMETER                       VALUE
tree_path                       stevedore.yaml
builder_path                    stevedore.yaml
log_path                        /dev/null
num_workers                     4
push_images                     true
build_on_cascade                false
docker_registry_credentials_dir ~/.config/stevedore/credentials
semantic_version_tags_enabled   false
semantic_version_tags_templates [{{ .Major }}.{{ .Minor }}.{{ .Patch }}]
```

### In-line builder
When is needed to build an image using an adhoc driver configuration, and to simplify the Stevedore configuration, the builder could be defined inside the [image configuration]({{<ref "reference-guide/image/">}}), in that case the builder is known as an `in-line` builder. 
In-line builders are also defined following the [Keywords reference for builders configuration]({{<ref "/reference-guide/builder/#keywords-reference-for-builder-configuration">}}).

{{< highlight Yaml "linenos=table" >}}
my-image-base:
    "0.0.1":
        registry: my-registry.my-domain.cat 
        namespace: library
        builder:
            driver: docker
            options:
                context:
                    path: my-image-base
{{< /highlight >}}

## Keywords reference for builder configuration

On next table are described those keywords attributes that could be included on a builder definition:
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**name**|*string*|Name of the builder<br><font color="#AA0088">*optional*</font>|When a builder is defined as `global` its value is the yaml object's `key` where the builder is being defined. In case that a builder is defined such an `in-line` builder, its value is the same as the image's name|
|**driver**|*string*|Driver to be used to build the image<br><font color="#AA0088">*mandatory*</font>|The allowed values are:<br> - **ansible-playbook**<br> - **docker**|
|**options**|*yaml object*|Options holds those parameters required by the driver<br><font color="#AA0088">*mandatory*</font>|Each driver requires its own configuration parameters.<br>Refer to [options reference]({{<ref "/reference-guide/builder/#options">}}) for detailed description|
|**variables_mapping**|*yaml object*|Maps a relationship from image’s attributs  to driver variables/parameteres/arguments<br><font color="#AA0088">*optional*</font>|Each driver requires its own variables mapping.<br>Refer to [variables mapping]({{<ref "/reference-guide/builder/#variables-mapping-reference">}})|

### Options reference
On the tabs below is defined the options reference for each driver.
{{<tabs "builder-options">}}

{{<tab "docker">}}
### docker
Here is described builder's configuration options for `docker` driver.

#### Builder options
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**context**|*yaml object*|Is the Docker build's context where are placed the set fo files required to build and image. Context could be located either on a local path, `path` context, or in a git respository, `git` context.<br><font color="#AA0088">*mandatory*</font>|Each context type has its own specifications [path]({{<ref "reference-guide/builder/#path-context">}}) [git]({{<ref "reference-guide/builder/#git-context">}})|
|**dockerfile**|*string*|`Dockerfiles`'s location path. The path is relative to context root<br><font color="#AA0088">*optional*</font>|By default, is used the `Dockerfile` located at context root|
---
##### Path context
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|path||||
- Example
{{<highlight Yaml "linenos=table">}}
    code:
        driver: docker
        options:
            context:
                path: .
{{</highlight>}}

##### Git context 
Git
    repository
    reference
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|git||||
|repository||||
|reference||||
- Example
{{<highlight Yaml "linenos=table">}}
    code:
        driver: docker
        options:
            context:
                git: 
                    repository: https://github.com/apenella/simple-go-helloworld.git
{{</highlight>}}
{{</tab>}}

{{<tab "ansible-playbook">}} 
### ansible-playbook 
Here is described builder's configuration options for `ansible-playbook` driver.

#### Builder options
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**playbook**|*string*|Is the playbook file location path<br><font color="#AA0088">*mandatory*</font>||
|**inventory**|*string*|<br><font color="#AA0088">*mandatory*</font>||

#### Example
{{<highlight Yaml "linenos=table">}}
    infrastructure:
        driver: ansible-playbook
        options:
            inventory: inventory/all
            playbook: build_applications.yml
{{</highlight>}}
{{</tab>}}

{{</tabs>}}


### Variables mapping reference

{{<tabs "builder-varmap">}}
{{<tab "docker">}}
|Key name|Description|Default Value|
|---|---|---|
|**image_builder_name_key**|               // Not comming from build's command flag|image_builder_name|
|**image_builder_tag_key**|                // Not comming from build's command flag|image_builder_tag|
|**image_builder_registry_namespace_key**| // Not comming from build's command flag|image_builder_registry_namespace|
|**image_builder_registry_host_key**|      // Not comming from build's command flag|image_builder_registry_host|
|**image_builder_label_key**||image_builder_label|
|**image_from_name_key**||image_from_name|
|**image_from_tag_key**||image_from_tag|
|**image_from_registry_namespace_key**||image_from_registry_namespace|
|**image_from_registry_host_key**||image_from_registry_host|
|**image_name_key**||image_name|
|**image_tag_key**||image_tag|
|**image_extra_tags_key**||image_extra_tags|
|**image_registry_namespace_key**||image_registry_namespace|
|**image_registry_host_key**||image_registry_host|
|**push_image_key**||push_image|
{{</tab>}}

{{<tab "ansible-playbook">}}
|Key name|Description|Default Value|
|---|---|---|
|**image_builder_name_key**|               // Not comming from build's command flag|image_builder_name|
|**image_builder_tag_key**|                // Not comming from build's command flag|image_builder_tag|
|**image_builder_registry_namespace_key**| // Not comming from build's command flag|image_builder_registry_namespace|
|**image_builder_registry_host_key**|      // Not comming from build's command flag|image_builder_registry_host|
|**image_builder_label_key**||image_builder_label|
|**image_from_name_key**||image_from_name|
|**image_from_tag_key**||image_from_tag|
|**image_from_registry_namespace_key**||image_from_registry_namespace|
|**image_from_registry_host_key**||image_from_registry_host|
|**image_name_key**||image_name|
|**image_tag_key**||image_tag|
|**image_extra_tags_key**||image_extra_tags|
|**image_registry_namespace_key**||image_registry_namespace|
|**image_registry_host_key**||image_registry_host|
|**push_image_key**||push_image|
{{</tab>}}

{{</tabs>}}

## Builder definition
Builder definition must match to golang struct defined below. Options structure depends on each driver.
{{<highlight golang "linenos=table">}}
    type Builder struct {
        Name        string                 `yaml:"name"`
        Driver      string                 `yaml:"driver"`
        Options     map[string]interface{} `yaml:"options"`
        VarMapping  varsmap.Varsmap        `yaml:"variables_mapping"`
    }
{{</highlight>}}

Set of builders must match to golang struct defined below
{{<highlight golang "linenos=table">}}
    type Builders struct {
        Builders map[string]*Builder `yaml:"builders"`
    }
{{</highlight>}}