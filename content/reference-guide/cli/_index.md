---
title: Stevedore CLI
date: "2020-01-31"
weight: 20
---

{{<toc>}}

## **build**
Stevedore command to build images

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
Stevedore command to generate shell completions. [Here](https://github.com/spf13/cobra/blob/master/shell_completions.md) you could find more details about completion

```
Usage:
  stevedore completion [flags]

Flags:
  -h, --help   help for completion

Global Flags:
  -c, --config string   Configuration file location path
```

## **create**
Stevedore command to create configuration files

```
Usage:
  stevedore create [flags]
  stevedore create [command]

Aliases:
  create, generate

Available Commands:
  configuration Create stevedore configuration file
  credentials   Create stevedore docker registry credentials
```

### create configuration
Stevedore subcommand to initialize a project with a configuration file and a credential file

```

Usage:
  stevedore create configuration [flags]

Aliases:
  configuration, config

Flags:
  -C, --build-on-cascade                             On build, start children images building once an image build is finished. Its default value is 'false'
  -b, --builder-path-file string                     Builders location path. Its default value is 'stevedore.yaml'
  -d, --credentials-dir string                       Location path to store docker registry credentials. Its default value is 'credentials'
  -p, --credentials-password credentials-regristry   Docker registry password. It is ignored unless credentials-regristry value is defined
  -r, --credentials-registry-host string             Docker registry host to register credentials
  -u, --credentials-username credentials-regristry   Docker registry username. It is ignored unless credentials-regristry value is defined
  -s, --enable-semver-tags                           Generate extra tags when the main image tags is semver 2.0.0 compliance. Its default value is 'false'
      --force                                        Force to create configuration file when the file already exists
  -h, --help                                         help for configuration
  -l, --log-path-file string                         Log file location path. Its default value is '/dev/null'
  -P, --no-push-images                               On build, push images automatically after it finishes. Its default value is 'true'
  -w, --num-workers int                              It defines the number of workers to build images which corresponds to the number of images that can be build concurrently. Its default value is '4' (default -1)
  -T, --semver-tags-template strings                 List of templates which define those extra tags to generate when 'semantic_version_tags_enabled' is enabled. Its default value is '{{ .Major }}.{{ .Minor }}.{{ .Patch }}'
  -t, --tree-path-file string                        Images tree location path. Its default value is 'stevedore.yaml'

Global Flags:
  -c, --config string   Configuration file location path
```

### create credentials
Stevedore subcommand to create a credentials file

```
Usage:
  stevedore create credentials [flags]

Aliases:
  credentials, auth

Flags:
  -d, --credentials-dir string   Location path to store docker registry credentials (default "/etc/stevedore/credentials")
  -h, --help                     help for credentials
  -p, --password string          Docker registry password
  -r, --registry-host string     Docker registry host to register credentials
  -u, --username string          Docker registry username

Global Flags:
  -c, --config string   Configuration file location path
```

## **get**
Stevedore command to get items information

```
Usage:
  stevedore get [flags]
  stevedore get [command]

Aliases:
  get, list

Available Commands:
  builders      get builders return all builders defined
  configuration get configuration
  credentials   get credentials return all credentials defined
  images        get images return all images defined

Flags:
  -h, --help   help for get

Global Flags:
  -c, --config string   Configuration file location path

Use "stevedore get [command] --help" for more information about a command.
```

### get builders
Stevedore subcommand to get builders information

```
Usage:
  stevedore get builders [flags]

Aliases:
  builders, builder

Flags:
  -h, --help   help for builders

Global Flags:
  -c, --config string   Configuration file location path
```

### get configuration
Stevedore subcommand to get configuration information

```
Usage:
  stevedore get configuration [flags]

Aliases:
  configuration, config, conf, cfg

Flags:
  -h, --help   help for configuration

Global Flags:
  -c, --config string   Configuration file location path
```

### get credentials
Stevedore subcommand to get credentials information

```
Usage:
  stevedore get credentials [flags]

Aliases:
  credentials, auth, auths, credential

Flags:
  -h, --help   help for credentials
  -w, --wide   Show wide docker registry credentials information

Global Flags:
  -c, --config string   Configuration file location path
```

### get images
Stevedore subcommand to get images information

```
Usage:
  stevedore get images [flags]

Aliases:
  images, images

Flags:
  -h, --help   help for images
  -t, --tree   Return the output as a tree

Global Flags:
  -c, --config string   Configuration file location path
```

## **init**
Stevedore init command is a shortcut for '[stevedore create configuration]({{<ref "/reference-guide/cli/#create-configuration">}})'

## **promote**
Stevedore command to promote, publish or copy images to a docker registry or namespace

```
Usage:
  stevedore promote [flags]

Aliases:
  promote, publish

Flags:
  -D, --dry-run                          Dry run show the promote parameters
  -S, --enable-semver-tags               Generate extra tags based on semantic version tree when main version is semver 2.0.0 compliance
  -h, --help                             help for promote
  -i, --promote-image-name string        Name for the image to be promoted
  -n, --promote-image-namespace string   Registry's mamespace for the image to be promoted
  -r, --promote-image-registry string    Registry's host for the image to be promoted
  -t, --promote-image-tag strings        Extra tag for the image to be promoted
  -R, --remove-promote-tags              Remove remoted tags from local docker host
  -T, --semver-tags-template strings     List templates to generate tags following semantic version expression

Global Flags:
  -c, --config string   Configuration file location path
```

## **version**
Stevedore command to print the binary release version
