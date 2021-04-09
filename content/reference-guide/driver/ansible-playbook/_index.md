---
title: Ansible Playbook
date: "2020-01-31"
weight: -20
---

{{< hint warning >}}
TODO
{{< /hint >}}

{{<toc>}}

## Driver overview
To create a docker image using `ansible-playbook` driver, is required to have an ansible playbook that could create docker images before start doing anything. 
`stevedore` executes an `ansible-playbook` command, for that reason is required `ansible` to be already installed on the host where it runs.

`stevedore` prepares all the parameters that defines the image to be built and then execute the `ansible-playbook` command through [go-ansible](https://github.com/apenella/go-ansible) library. The parameters are passed to `ansible-playbook` command as `extra-vars`, the most precedence variables on ansible, it means that overrides any other value previously defined on the playbook.

## Playbook at a glance

1. Start a docker container which is used as builder base from the required image from
2. Provision and configure the builder base container
3. Commit the builder base container

### Playbook tips and tricks
- To create a playbook for building docker images purpose could be used [docker collection](https://galaxy.ansible.com/community/docker)

- Stevedore host must follow the collection [requirements](https://docs.ansible.com/ansible/latest/scenario_guides/guide_docker.html#requirements)

- Use [docker_container](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html#ansible-collections-community-docker-docker-container-module) module to create a docker container that is

### Example