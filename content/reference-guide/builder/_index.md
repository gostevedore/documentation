---
title: Builder
weight: 20
---

{{<toc>}}

## General Description
A builder holds those parameters required by a driver to proceed with the Docker's image building request, such the build context or its Dockerfile location, for instance. A builder is defnied by [`YAML`](https://en.wikipedia.org/wiki/YAML) data structure, following the specifications described on [Keywords reference for builders configuration]({{<ref "/reference-guide/builder/#keywords-reference-for-builder-configuration">}}).

Builders can be defined as `global`, which could be used by any image, and in that case the builder must be placed under the `builders` block on [Stevedore configuration]({{<ref "/getting-started/configuration">}}), or can be defined inside the image definition, that are known as `in-line` builders.

### Global builder
Global builders are those builders that are defined under the `builders` block on [Stevedore configuration]({{<ref "/getting-started/configuration">}}) and can be used by any image on the images tree. 

To include a `global` builder on [Stevedore configuration]({{<ref "/getting-started/configuration">}}), it must be edited the file specified in [builder_path]({{<ref "/getting-started/configuration/#builder_path">}}) configuration parameter, and place there the new definition, under the `builders` block.

For example, when `builder_path` is set as below:
{{<highlight Yaml "linenos=table">}}
builder_path: stevedore_builders.yaml
{{</highlight>}}

The file `stevedore_builders.yaml` must be edited to include there the new builder definition. On next example are defined the global builders `builder1`, `builder2` and `builder3`.
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

The [builder_path]({{<ref "/getting-started/configuration/#builder_path">}}) value could be also achieved through Stevedore CLI command (`stevedore get configuration`).
```bash
$ stevedore get configuration
PARAMETER                           VALUE
tree_path                           stevedore.yaml
builder_path                        stevedore.yaml
log_path                            /dev/null
num_workers                         4
push_images                         true
build_on_cascade                    false
docker_registry_credentials_dir     ~/.config/stevedore/credentials
semantic_version_tags_enabled       false
semantic_version_tags_templates     [{{ .Major }}.{{ .Minor }}.{{ .Patch }}]
```

### In-line builder
When is needed to build an image using an adhoc driver configuration, and to simplify Stevedore configuration, the builder could be defined inside the [image configuration]({{<ref "reference-guide/image/">}}). In that case the builder is known as an `in-line` builder. 
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

On next table are described the high-level keywords attributes required to define a builder:
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**name**|*string*|Name of the builder<br><font color="#AA0088">*optional*</font>|When is created a `global` builder, its name is set by default as the yaml object's `key`, which holds the builder definition. In case that the builder is defined such an `in-line` builder, its name is set as the image's name.|
|**driver**|*string*|Driver to be used to build the image<br><font color="#AA0088">*mandatory*</font>|The allowed values are:<br> - **ansible-playbook**<br> - **docker**|
|**options**|*yaml object*|Options holds those parameters required by the driver<br><font color="#AA0088">*mandatory*</font>|Each driver requires its own configuration parameters.<br>Refer to [options reference]({{<ref "/reference-guide/builder/#options">}}) for detailed description.|
|**variables_mapping**|*yaml object*|Stevedore injects to drivers a bunch of parameters, such the image name or image tags. Varaibles mapping is a `key-value` structure where you can override the name of those variables that stevedore injects to drivers.<br><font color="#AA0088">*optional*</font>|Each driver requires its own variables mapping.<br>Refer to [variables mapping]({{<ref "/reference-guide/builder/#variables-mapping-reference">}}) for detailed description.|

### Options reference
On the tabs below is described how to define the options yaml object for each driver.
{{<tabs "builder-options">}}

{{<tab "docker">}}
### docker
Here are described builder's configuration options for `docker` driver.

#### Builder options
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**context**|*yaml object*|It is the Docker build's context where are placed the set of files required to build an image. Context could be located either on a local path, known as `path` context, or in a git respository, named `git` context.<br><font color="#AA0088">*mandatory*</font>|Each context type has its own specifications.<br>Refer [here]({{<ref "reference-guide/builder/#path-context">}}) for the `path` context details.<br>Refer [here]({{<ref "reference-guide/builder/#git-context">}}) for the `git` context details.|
|**dockerfile**|*string*|`Dockerfiles`'s location path. The path is relative to context root<br><font color="#AA0088">*optional*</font>|By default, is used the `Dockerfile` located at context root|
---
##### Path context
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**path**|string|It specifies where to find the files required to build the image<br><font color="#AA0088">*mandatory*</font>|-|
- Example
{{<highlight Yaml "linenos=table">}}
code:
  driver: docker
  options:
  context:
    path: .
  dockerfile: build/Dockerfile
{{</highlight>}}

##### Git context 
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|git|*yaml object*|It is the keyword under which are defined the git repository details<br><font color="#AA0088">*mandatory*</font>|-|
|repository|string|Repository specifies the git repository to find the files to build the image. `repository` keyword is placed under `git`.<br><font color="#AA0088">*mandatory*</font>|-|
|reference|string|Repository reference required to build the image. `reference` keyword is placed under `git`.<br><font color="#AA0088">*optional*</font>|By default, is used [`refs/heads/master`](https://pkg.go.dev/github.com/go-git/go-git/v5@v5.0.0/plumbing#ReferenceName) reference, that belongs to [go-git](https://github.com/go-git/go-git) project.|
- Example
{{<highlight Yaml "linenos=table">}}
  code:
    driver: docker
    options:
      context:
        git: 
          repository: https://github.com/apenella/simple-go-helloworld.git
          reference: v0.0.0
      dockerfile: build/Dockerfile
{{</highlight>}}
{{</tab>}}

{{<tab "ansible-playbook">}} 
### ansible-playbook 
Here are described builder's configuration options for `ansible-playbook` driver.

#### Builder options
|Keyword|Type|Description|Value|
|---|:---:|---|---|
|**playbook**|*string*|It is the playbook file location path<br><font color="#AA0088">*mandatory*</font>|-|
|**inventory**|*string*|It is the ansible-playbook inventory to be used<br><font color="#AA0088">*mandatory*</font>|It could be either an inventory file or and adhoc inventory, such `127.0.0.1,`|

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

`stevedore` injects through drivers a bunch of parameters to image building process. Among those parameters you could find the image from is based the built image or which tags are needed to be created. Those paramaters are defined on a `key-value` structure known as `variables mapping`.
Internally, `stevedore` refers to each parameter by its key on `variables mapping` structure and you could override their values on the builder definition. 

`variables mapping` are passed such build arguments on `docker` driver or extra variables on `ansible-playbook` driver.
Since each driver behaves on its own way, not all drivers uses the same set of `variables mapping`.

To avoid any confusion you must understand which is the difference between a `variables mapping` key name, key value and an argument value.
- **key name**: It is used internally by `stevedore` to identify the argument's name to use during the image building process
- **key value**: It is the argument name and could be override on [builder]({{<ref "/getting-started/concepts/#builder">}}) definition
- **argument value**: It is the value that `stevedore` gives to the argument before injecting it to image building process. That value comes from [image tree]({{<ref "/getting-started/concepts/#image-tree">}}) or CLI flags.

Below are defined the variables mapping for each driver.

{{<tabs "builder-varmap">}}
{{<tab "docker">}}
Docker driver passes `variables mapping` to docker engine as build arguments and each variable can be consumed as an [ARG](https://docs.docker.com/engine/reference/builder/#arg) inside the Dockerfile definition.

|Key name|Description|Default Value|
|---|---|---|
|**image_from_name_key**|Its value is referred to Dockerfile's argument name used to define the parent's image name.<br>The argument value is set such the parent's image name, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_name|
|**image_from_registry_host_key**|Its value is referred to Dockerfile's argument name used to define the parent's registry host name.<br>The argument value is set such the parent's registry, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_registry_host|
|**image_from_registry_namespace_key**|Its value is referred to Dockerfile's argument name used to define the parent's registry namespace.<br>The argument value is set such the parent's namespace, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_registry_namespace|
|**image_from_tag_key**|Its value is referred to Dockerfile's argument name used to define the parent's image tag.<br>The argument value is set such the parent's version, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_tag|

On the example below is defined a Dockerfile which uses the arguments passed from `stevedore`'s docker driver.
{{<highlight shell "linenos=table">}}
ARG image_from_registry_host
ARG image_from_registry_namespace
ARG image_from_name
ARG image_from_tag
FROM ${image_from_registry_host}/${image_from_registry_namespace}/${image_from_name}:${image_from_tag}
RUN ...

{{</highlight>}}


{{</tab>}}

{{<tab "ansible-playbook">}}
`ansible-playbook` driver passes each `variables mapping` entry as a runtime variable, also known an [extra-vars](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#defining-variables-at-runtime)
|Key name|Description|Default Value|
|---|---|---|
|**image_builder_label_key**|When you create an image using the `ansible-playbook` driver, that argument value is the intermediate container's name, which is used during the provisioning process.<br>[Here]({{<ref "/reference-guide/driver/ansible-playbook/">}}) you could read how `ansible-playbook` works.|image_builder_label|
|**image_from_name_key**|Its value is referred to Dockerfile's argument name used to define the parent's image name.<br>The argument value is set such the parent's image name, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_name|
|**image_from_registry_host_key**|Its value is referred to Dockerfile's argument name used to define the parent's registry host name.<br>The argument value is set such the parent's registry, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_registry_host|
|**image_from_registry_namespace_key**|Its value is referred to Dockerfile's argument name used to define the parent's registry namespace.<br>The argument value is set such the parent's namespace, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_registry_namespace|
|**image_from_tag_key**|Its value is referred to Dockerfile's argument name used to define the parent's image tag.<br>The argument value is set such the parent's version, defined on the [image tree]({{<ref "/getting-started/concepts/#image-tree">}})|image_from_tag|
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
    Name    string         `yaml:"name"`
    Driver    string         `yaml:"driver"`
    Options   map[string]interface{} `yaml:"options"`
    VarMapping  varsmap.Varsmap    `yaml:"variables_mapping"`
  }
{{</highlight>}}

Set of builders must match to golang struct defined below
{{<highlight golang "linenos=table">}}
  type Builders struct {
    Builders map[string]*Builder `yaml:"builders"`
  }
{{</highlight>}}