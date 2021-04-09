---
title: Stevedore CLI
date: "2020-01-31"
weight: 20
---

## **build**
Build images

```
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

## **completion**
Generates completion from multiple shell. [Here](https://github.com/spf13/cobra/blob/master/shell_completions.md) you could find more details about completion

```
To load completion run

    . <(stevedore completion)
    
    To configure your bash shell to load completions for each session add   to your bashrc
    # ~/.bashrc or ~/.profile
    . <(stevedore completion)

Usage:
  stevedore completion [flags]

Flags:
  -h, --help   help for completion

Global Flags:
  -c, --config string   Configuration file location path
```

## **create**
create      Create stevedore elements

## **get**
get         get could return serveral items

## **help**
help        Help about any command

## **init**
init        Create stevedore configuration file

## **promote**
promote

## **version**
version     Print Stevedore version number

