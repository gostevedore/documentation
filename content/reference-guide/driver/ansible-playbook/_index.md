---
title: Ansible Playbook
date: "2021-04-12"
weight: -20
---

{{<toc>}}

## Driver overview
To create a docker image using `ansible-playbook` driver, is required to have an ansible playbook that could create docker images before start doing anything. 
`stevedore` executes `ansible-playbook` command, for that reason is required `ansible` to be already installed on the host where it runs.

`stevedore` prepares all the parameters that defines the image to be built and uses [go-ansible](https://github.com/apenella/go-ansible) library to execute the `ansible-playbook` command. The parameters are passed to `ansible-playbook` command as `extra-vars`, the most precedence variables on ansible, it means that overrides any other value previously defined on the playbook.

## Playbook at a glance
To write a playbook to build docker images you could use [docker collection](https://galaxy.ansible.com/community/docker). There, are defined a set of modules such as [docker_container](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html#ansible-collections-community-docker-docker-container-module), that help to interact to Docker API.

Here are listed few steps of a rough playbook structure to build a Docker image:
1. Start a docker container which is used as builder base from the required image from
2. Provision and configure the builder base container
3. Commit the builder base container
