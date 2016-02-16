docker-plex
===========

[![Build Status](https://img.shields.io/travis/marvinpinto/ansible-role-docker-plex/master.svg?style=flat-square)](https://travis-ci.org/marvinpinto/ansible-role-docker-plex)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-docker--plex-blue.svg?style=flat-square)](https://galaxy.ansible.com/marvinpinto/docker-plex)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.txt)

Ansible Galaxy role to manage and run a [plex](https://plex.tv) docker
container.

This role wires together the plex [docker
container](https://hub.docker.com/r/linuxserver/plex) created by
[linuxserver](https://github.com/linuxserver/docker-plex), along with various
boilerplate to get things going.


Requirements
------------

This role has been tested on Ubuntu 14.04 and will likely only work on an
Ubuntu-like system. You will also need a functioning docker environment and a
recent-is version of `docker-py` for this role to work.

If you have neither and would like ansible to set this up for you, have a look
at the [marvinpinto.docker](https://galaxy.ansible.com/marvinpinto/docker)
Galaxy role.


Role Variables
--------------

```yaml
# Docker container name
docker_plex_container_name: 'plex'

# Plex version. Options are:
# - specific version number e.g. '0.9.12.4.1192-9a47d21'
# - 'plexpass'
# - 'latest'
docker_plex_version: 'latest'

# Directory that will be used as the root of all plex-related configuration &
# data.
#
# Assuming 'docker_plex_mounted_directory' is set to /tmp/plex_mount, then the
# following directories will be automatically created if they don't already
# exist:
#
# /tmp/plex_mount/transcode
# /tmp/plex_mount/library
# /tmp/plex_mount/tv
# /tmp/plex_mount/movies
docker_plex_mounted_directory: '/tmp/plex_mount'
```


Examples
--------

Install this module from Ansible Galaxy into the './roles' directory:
```bash
ansible-galaxy install marvinpinto.docker-plex -p ./roles
```

Use it in a playbook as follows:
```yaml
- hosts: '127.0.0.1'
  roles:
    - role: 'marvinpinto.docker-plex'
      become: true
```


Mounted Directory
-----------------

The reasoning behind storing all related configuration in the
`docker_plex_mounted_directory` root directory is because a person now has the
ability to manage all the configuration + data outside of Ansible.

This becomes especially useful when said mounted directory resides on a
separate filesystem (EBS, USB disk, etc).


Initial Setup & Configuration
-----------------------------

Since Plex only allows administrative actions to be initiated from a local
subnet, you will likely need to create an SSH tunnel to your host machine for
the initial setup. Note that this **only needed for the initial setup**.

https://support.plex.tv/hc/en-us/articles/200288586-Installation

In its general form, it looks something like this:

```bash
ssh ip.address.of.server -L 8888:localhost:32400
```

Then browsing over to [http://localhost:8888/web](http://localhost:8888/web)
should allow you to configure your Plex app.

In a vagrant environment, that ssh command looks something like:

```bash
ssh \
  -p <Port> \
  -i <IdentityFile> \
  -L 8888:localhost:32400 \
  <User>@<HostName>
```

Where `Port`, `IdentityFile`, `User`, and `HostName` can all be found by
running:

```bash
vagrant ssh-config
```

More information available on the Plex [Installation
Docs](https://support.plex.tv/hc/en-us/articles/200288586-Installation).
