uclalib_role_k3s
=========

Ansible role to install the [k3s](https://k3s.io/) server component on a RHEL 8 Stream system 

Requirements
------------

This role assumes the following conditions in your environment:
  * RHEL 8 Stream OS

Usage of k3s requires disabling/stopping any host-based firewalls installed the server, e.g.:
  * `iptables`
  * `nftables`
  * `firewalld`

This role stops and disables firewalld, allows incoming ssh connections, and disables all other incoming connections. k3s itself manages further required ports.

Rules for restricting access to the host should be managed by an external firewall and/or the Kubernetes CNI (e.g. Calico)

Role Variables
--------------

Reference `defaults/main.yml` to review default values.

Installation-related variables (you can typically leave these set to their default values):

  * `k3s_channel` - Defines a channel (stable, latest, testing) to install. 
  * `k3s_config_path` - Defines the directory holding the k3s installation configuration file
  * `k3s_install_script_url` - Defines the URL to obtain the k3s installation script
  * `k3s_version` - Defines the version of k3s to install (check version numbers on the k3s [releases](https://github.com/k3s-io/k3s/releases) page). Overrides `k3s_channel`.

k3s installation configuration options:

  * `k3s_exec_options` - Defines the list of k3s server configuration options to use during installation
    * Configuration options are available at [K3s Server Configuration Reference](https://rancher.com/docs/k3s/latest/en/installation/install-options/server-config/). 
    * Example usage:
      ```
      k3s_exec_options: |
        write-kubeconfig-mode: "0644"
        disable: "traefik,metrics-server"
      ```

Tags
----

Use the following tags to run specific role functions:

  * `k3s-iptables` - set up host based firewall
  * `k3s-setup` - set-up prerequisites for k3s install
  * `k3s-install` - perform install/upgrade of k3s server
  * `k3s-traefik` - customize traefik configuration
  * `k3s-update` - update k3s to latest version in defined channel (default is `stable` channel)
    * NOTE: must explicitly call this tag to run the update tasks

Dependencies
------------

None

Example Playbook
----------------

An example complete playbook that uses redis as a cache would like something like the following:

```
---

- name: uclalib_k3s.yml
  become: yes
  become_method: sudo
  hosts: all

  roles:
    - { role: uclalib_role_k3s }
