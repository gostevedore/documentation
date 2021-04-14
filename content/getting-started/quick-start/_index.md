---
title: Quick start
date: "2021-04-14"
weight: 20
---

{{<toc>}}

## Download and install stevedore

{{< hint info >}}
stevedore has been developed and tested on amd64 architecture and Linux os, for that reason the only available tarball is for amd64-Linux tarball.
In case you are running another arch or os, you could build it from source.
{{< /hint >}}

### Install from tarball

1. Achieve *Stevedore*'s tarball
```
curl -sL https://github.com/gostevedore/stevedore/releases/download/v0.10.2/stevedore_0.10.2_Linux-x86_64.tar.gz > stevedore_0.10.2_Linux-x86_64.tar.gz
```

2. Untar the package
```
tar xzfv stevedore_0.10.2_Linux-x86_64.tar.gz
```

3. You can create a symlink to *stevedore*'s binary
```
ln -sf ${PWD}/stevedore /usr/local/bin/stevedore
```

### Install from source

1. Clone *stevedore*'s repository
```
git clone https://github.com/gostevedore/stevedore.git
```

2. Build *stevedore*'s code
```
make build
```
You can also run `make tar` to generate a tarball, under `dist` folder, containing the binary.

#### Use Goreleaser to create an snapshot
In case you want to use [goreleaser] to generate an snapshot you can run `make snapshot`.

## Initialize the stevedore project
```
stevedore init
```
**Usage:**
```
Create stevedore configuration file

Usage:
  stevedore init [flags]

Aliases:
  init

Flags:
  -C, --build-on-cascade                             On build, start children images building once an image build is finished. Its default value is 'false'
  -b, --builder-path-file string                     Builders location path. Its default value is 'stevedore.yaml'
  -d, --credentials-dir string                       Location path to store docker registry credentials. Its default value is 'credentials'
  -p, --credentials-password credentials-regristry   Docker registry password. It is ignored unless credentials-regristry value is defined
  -r, --credentials-registry-host string             Docker registry host to register credentials
  -u, --credentials-username credentials-regristry   Docker registry username. It is ignored unless credentials-regristry value is defined
  -s, --enable-semver-tags                           Generate extra tags when the main image tags is semver 2.0.0 compliance. Its default value is 'false'
      --force                                        Force to create configuration file when the file already exists
  -h, --help                                         help for init
  -l, --log-path-file string                         Log file location path. Its default value is '/dev/null'
  -P, --no-push-images                               On build, push images automatically after it finishes. Its default value is 'true'
  -w, --num-workers int                              It defines the number of workers to build images which corresponds to the number of images that can be build concurrently. Its default value is '4' (default -1)
  -T, --semver-tags-template strings                 List of templates which define those extra tags to generate when 'semantic_version_tags_enabled' is enabled. Its default value is '{{ .Major }}.{{ .Minor }}.{{ .Patch }}'
  -t, --tree-path-file string                        Images tree location path. Its default value is 'stevedore.yaml'

Global Flags:
  -c, --config string   Configuration file location path
```

## Load credentials, in case you need
```
stevedore create credentials
```
**Usage:**
```
Create stevedore docker registry credentials

Usage:
  stevedore create credentials [flags]

Aliases:
  credentials, auth

Flags:
  -d, --credentials-dir string   Location path to store docker registry credentials (default "/home/aleix/.config/stevedore/credentials")
  -h, --help                     help for credentials
  -p, --password string          Docker registry password
  -r, --registry-host string     Docker registry host to register credentials
  -u, --username string          Docker registry username

Global Flags:
  -c, --config string   Configuration file location path
```

## Define builders
Refer to [builders]({{<ref "/reference-guide/builder/">}}) reference guide

## Define the images tree
Refer to [images tree]({{<ref "/reference-guide/images-tree/">}}) reference guide

## Start building
```
stevedore build
```
**Usage:**
```
Build images

Usage:
  stevedore build <image> [flags]

Flags:
  -b, --builder-name string            Intermediate builder's container name [only applies to ansible-playbook builders]
  -C, --cascade                        Build images on cascade. Children's image build is started once the image build finishes
  -d, --cascade-depth int              Number images levels to build when build on cascade is executed (default -1)
  -L, --connection-local               Use local connection for ansible [only applies to ansible-playbook builders]
      --debug                          Enable debug mode to show build options
  -D, --dry-run                        Run a dry-run build
  -S, --enable-semver-tags             Generate a set of tags for the image based on the semantic version tree when main version is semver 2.0.0 compliance
  -h, --help                           help for build
  -I, --image-from string              Image (FROM) parent's name
  -N, --image-from-namespace string    Image (FROM) parent's registry namespace
  -R, --image-from-registry string     Image (FROM) parent's registry host
  -V, --image-from-version string      Image (FROM) parent's version
  -i, --image-name string              Image name- It overrides image tree image name
  -v, --image-version strings          Image versions to be built. One or more image versions could be built
  -H, --inventory string               Specify inventory hosts' path or comma separated list of hosts [only applies to Ansible builders]
  -l, --limit string                   Further limit selected hosts to an additional pattern [only applies to Ansible builders]
  -n, --namespace string               Image's registry namespace where image will be stored
  -P, --no-push                        Do not push the image to registry once it is built
  -w, --num-workers int                Number of workers to execute builds
  -r, --registry string                Image's registry host where image will be stored
  -T, --semver-tags-template strings   List templates to generate tags following semantic version expression
  -s, --set strings                    Set variables to use during the build. The format of each variable must be <key>=<value>
  -p, --set-persistent strings         Set persistent variables to use during the build. A persistent variable will be available on child image during its build and could not be overwrite. The format of each variable must be <key>=<value>
  -t, --tag strings                    Give an extra tag for the docker image

Global Flags:
  -c, --config string   Configuration file location path

```

[goreleaser]: https://goreleaser.com/