uclalib_role_k3s
=========

Ansible role to install the [k3s](https://k3s.io/) server component on a RHEL system 

Requirements
------------

This role assumes the following conditions in your environment:
  * RHEL 7 or 8 OS

Usage of k3s requires disabling/stopping any host-based firewalls installed the server, e.g.:
  * `iptables`
  * `nftables`
  * `firewalld`

Rules for restricting access to the host should be managed by an external firewall and/or the Kubernetes CNI (e.g. Calico)

Role Variables
--------------

Reference `defaults/main.yml` to review default values.

Installation-related variables (you can typically leave these set to their default values):

  * `k3s_install_script_url` - Defines the URL to obtain the k3s installation script
  * `k3s_install_script` - Defines the path to the downloaded k3s installation script
  * `k3s_version` - Defines the version of k3s to install (check version numbers on the k3s [releases](https://github.com/k3s-io/k3s/releases) page)
  * `k3s_config_path` - Defines the directory holding the k3s installation configuration file

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

  * `k3s-setup` - set-up prerequisites for k3s install
  * `k3s-install` - perform install/upgrade of k3s server

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
