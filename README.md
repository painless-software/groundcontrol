Groundcontrol
=============

> Painless Infrastructure for Small and Medium Size Businesses

Groundcontrol provides an infrastructure to manage the infrastructure for your office, IoT and home environment.

- Fully automatic setup of desktop and server machines
- Integrated infrastructure monitoring and management
- Complete tool chain for infrastructure as code
- Automatic backups of your critical data (local or offsite)
- Automatic user data synchronization for desktop machines (offline availability)

Core Components
---------------

- Infrastructure management frontend ([The Foreman](https://theforeman.org/))
- Virtualization infrastructure ([libvirt](https://libvirt.org/))
- User and host identity management incl. DHCP/router ([FreeIPA](https://www.freeipa.org/))
- Version control & CI/CD ([GitLab Libre](https://about.gitlab.com/products/))
- Data storage, automatic backup & synching ([FreeNAS/ZFS](http://www.freenas.org/), [Syncthing](https://syncthing.net/))

Getting Started
---------------

### Prerequisites

- [Vagrant](https://www.vagrantup.com/downloads.html)
- [Ansible](http://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#latest-releases-via-pip) -
  _you can use the prepared `Pipfile` with [Pipenv](https://docs.pipenv.org/) for your convenience_

### Launching

```bash
$ vagrant up  # this will take several minutes
```

Vagrant will install plugins and Ansible roles required by the setup
automatically as needed. You may need to run the command twice the very first
time if plugins are installed and a Vagrant error shows.
