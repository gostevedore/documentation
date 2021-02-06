---
title: Concepts
date: "2020-01-31"
weight: 20
---

On that section there is a brief description of the main Stevedore components or term that appears a long that documentation.

{{<toc>}}

### Build
The process that generates a Docker image is named the build.

### Builder
A Builder holds those parameters required by a driver to proceed with the building request, such the build context or your Dockerfile location, for instance. 

You can define as many builders as you need for each driver. For example, suppose that you have a dozen of microservices written in Golang and another bunch in Python. All your Golang microservices are built in the same way, then you can define one builder to create all the golang's microservices images. Regarding Python microservices you can also do the same. Finally, all your microservices can be built defining two builders.

A Builder could be defined as `global`, in that case it can be used by any image, and must be placed under the `builders` definition on your configuration. 
On the snipped below there are defined two global builders: `code`, at line 2 and `infrastructure`, at line 7.
{{< highlight Yaml "linenos=table" >}}
builders:
    code:
        driver: docker
        options:
            context:
                path: .
    infrastructure:
        driver: ansible-playbook
        options:
            inventory: inventory/all
            playbook: build_applications.yml
{{< /highlight >}}

You could also define the builder inside the image definition, and it is known as `in-line` builder.
On that other snipped is defined an in-line builder:
{{< highlight Yaml "linenos=table" >}}
  simple-go-helloworld:
    "0.0.1":
      version: "{{ .Version }}"
      builder:
        driver: docker
        options:
            context:
                git: https://github.com/apenella/simple-go-helloworld.git
        variables_mapping:
          image_name_key: image
          image_tag_key: tag
{{< /highlight >}}

### Credentials
A credential is an username-password tuple which could be used by Stevedore to authenticate in your behalf to a Docker registry.

### Driver
Stevedore does not implement its own way to build docker images but it does through existing tools created for that purpose. Driver is the component in charge to collect the details to build an image and make the request for build to one of those tools.

There are currently two kinds of drivers: 
- **docker** that uses Docker API. When a builder is defined to use a docker driver, it must provide at least the context, and the Dockerfile location when it is not found on context's root. For further information see the [reference guide]({{<ref "/reference-guide/driver/docker">}}).
- **ansible-playbook** that execute ansible playbooks. When a builder is defined to use an ansible-playbook driver, it must provide the playbook location and also where is placed the inventory. For further information see the [reference guide]({{<ref "/reference-guide/driver/ansible-playbook">}}).

### Image
An image defines what you want to build, how to build and the relationship with other images. An image makes no sense by itself and must be placed on an 
[image tree]({{<ref "/getting-started/concepts/#image-tree">}}) before use it. For further information see the [reference guide]({{<ref "/reference-guide/image">}}).

{{< highlight Yaml "linenos=table" >}}
my-image-base:
    "0.0.1":
        registry: my-registry.my-domain.cat 
        namespace: library
        tags:
        - latest 
        children:
            my-ms1:
            - stable
            - devel
            my-ms2:
            - stable
        builder:
            driver: docker
            options:
                context:
                    path: my-image-base
{{< /highlight >}}

### Image tree
On Stevedore's startup are loaded all images that can be built, and those images are loaded from the image tree. Therefore, the image tree contains all those images which you can deal with, through by Stevedore. 

On the snipped below there is shown an image tree where are defined three images: `my-image-base`, at line 2, `my-ms1`,at line 21, and `my-ms2`, at line 32.
{{< highlight Yaml "linenos=table" >}}
images_tree:
    my-image-base:
        "stable":
            registry: my-registry.my-domain.cat 
            namespace: library
            tags:
            - latest 
            children:
                my-ms1:
                - "0.0.1"
                - devel
                - "*"
                my-ms2:
                - "0.1.5"
                - "*"
            builder:
                driver: docker
                options:
                    context:
                        path: my-image-base
    my-ms1:
        "0.0.1":
            registry: my-registry.my-domain.cat 
            namespace: library
        devel:
            registry: my-registry.my-domain.cat 
            namespace: library
        "*":
            registry: my-registry.my-domain.cat 
            namespace: library
            version: "{{ .Version }}"
    my-ms2:
        "0.1.5":
            registry: my-registry.my-domain.cat 
            namespace: library
        "*":
            registry: my-registry.my-domain.cat 
            namespace: library
            version: "{{ .Version }}"
{{< /highlight >}}

### Promote
Promote an image, on Stevedore context, means to push an image to another Docker registre or to another location on the same Docker registry.

### Semver tag
When a tag is [semver 2.0.0](https://semver.org/) compliance is known as semver tag.
