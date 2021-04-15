---
title: Images Tree
date: "2021-04-14"
weight: 20
---

## General Description
`images tree` contains the relationship among a set of [images]({{<ref "/reference-guide/image/">}}) and it does by defining a map of maps data structure following the next rules:

1. Images tree is defined in a [`YAML`](https://en.wikipedia.org/wiki/YAML) structure.
2. The main key that contains the tree definition is named `images_tree`, which value is a map of maps data structure.
3. Under `images_tree` are placed all the images' names that belong to images tree. Image's name is the first level keys.
4. The second level keys identifies the image versions and are placed under each image name key.
5. Image version's value is the [image]({{<ref "/reference-guide/image/">}}) definition for that specific version.

{{<hint info>}}
In case an image name or image version contains a `.` (dot), it is needed to quote it to avoid the yaml parser to fails.
{{</hint>}}


- Example

On that example you could see such as image's names key `ubuntu`, `php-fpm` and `php-cli`. Taking a look to `ubuntu` image, you could see the versions `18.04` and `20.04`.

{{<highlight Yaml "linenos=table">}}
images_tree:
  ubuntu:
    "18.04":
      builder: global-infr-builder
      children:
      - php-fpm:
        - "7.3"
    "20.04":
      builder: global-infr-builder
      children:
      - php-fpm:
        - "7.4"
      - php-cli:
        - "7.4"
  php-fpm:
    "7.3":
      builder:
        driver: docker
        context:
          path: php-fpm
        vars:
          version: "{{ .Version }}"
    "7.4":
      builder:
        driver: docker
        context:
          path: php-fpm
        vars:
          version: "{{ .Version }}"
  php-cli:
    "7.4":
      builder:
        driver: docker
        context:
          path: php-fpm
        vars:
          version: "{{ .Version }}"
{{</highlight>}}

<!-- #
# An images must be defined based on the golang struct defined below:
# 
#     type Image struct {
#         Name      string                 #u0060yaml:"name"#u0060
#         Registry  string                 #u0060yaml:"registry"#u0060
#         Builder   interface{}            #u0060yaml:"builder"#u0060
#         Namespace string                 #u0060yaml:"namespace"#u0060
#         Version   string                 #u0060yaml:"version"#u0060
#         Tags      []string               #u0060yaml:"tags"#u0060
#         Vars      map[string]interface{} #u0060yaml:"vars"#u0060
#         Children  map[string][]string    #u0060yaml:"childs"#u0060
#     }
# 
#  An images tree define the relationship among a set of images and must match to golang struct defined below:
# 
#     type ImagesTree struct {
#         Images map[string]map[string]*image.Image #u0060yaml:"images_tree"#u0060
#     }
# 
#  examples:
#    1) Define an images tree that has one image named 'ubuntu' which has two versions, '20.04' and '18.04'.
#       The image '20.04' has two children: 'php-fpm' and 'php-cli', both versions defined are '7.4' while the version '18.04' has only one child: 'php-fpm' and version '7.3'
#       
#       images_tree:
#         ubuntu:
#           18.04:
#             builder: infrastructure
#             children:
#             - php-fpm:
#               - 7.3
#           20.04:
#             builder: infrastructure
#             children:
#             - php-fpm:
#               - 7.4
#             - php-cli:
#               - 7.4
#         php-fpm:
#           7.3:
#             builder: infrastructure
#             builder:
#               driver: docker
#               context:
#                 path: php-fpm
#               vars:
#                 version: 7.3
#           7.4:
#             builder: infrastructure
#             builder:
#               driver: docker
#               context:
#                 path: php-fpm
#               vars:
#                 version: 7.4
#         php-cli:
#           7.4:
#             builder: infrastructure
#             builder:
#               driver: docker
#               context:
#                 path: php-fpm
#               vars:
#                 version: 7.4 -->