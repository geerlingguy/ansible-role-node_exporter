# Ansible Role: Node exporter

[![CI](https://github.com/geerlingguy/ansible-role-node_exporter/actions/workflows/ci.yml/badge.svg)](https://github.com/geerlingguy/ansible-role-node_exporter/actions/workflows/ci.yml)

This role installs Prometheus' [Node exporter](https://github.com/prometheus/node_exporter) on Linux hosts, and configures a systemd unit file so the service can run and be controlled by systemd.

**Note**: If you're running in a Kubernetes cluster, you could run Node exporter as a DaemonSet in the cluster, instead of installing it on individual nodes.

## Requirements

N/A

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    node_exporter_version: 'latest'

The version of Node exporter to install. Available releases can be found on the [tags](https://github.com/prometheus/node_exporter/tags) listing in the Node exporter repository. When using a specific version, drop the `v` off the tag (i.e. `node_exporter_version: '0.18.1'`).

If you do not specify a variable, the default of 'latest' will be used. (This currently does not work in check mode.)

If you change the version, the `node_exporter` binary will be replaced with the updated version, and the service will be restarted.

    node_exporter_arch: "{{ {'i386': '386',
                    'x86_64': 'amd64',
                    'aarch64': 'arm64',
                    'armv7l': 'armv7',
                    'armv6l': 'armv6'}.get(ansible_architecture, 'amd64') }}"
    
    node_exporter_download_url: https://github.com/prometheus/node_exporter/releases/download/v{{ node_exporter_version }}/node_exporter-{{ node_exporter_version }}.linux-{{ node_exporter_arch }}.tar.gz

The architecture and download URL for Node exporter are being automatically detected and generated. If your architecture is not listed in the possible versions above, you may need to override the `node_exporter_arch` variable.

    node_exporter_bin_path: /usr/local/bin/node_exporter

The path where the `node_exporter` binary will be installed.

    node_exporter_host: 'localhost'
    node_exporter_port: 9100

Host and port on which node exporter will listen.

    node_exporter_options: ''

Any additional options to pass to `node_exporter` when it starts, e.g. `--no-collector.wifi` if you want to ignore any WiFi data.

    node_exporter_state: started
    node_exporter_enabled: true

Controls for the `node_exporter` service.

## Dependencies

None.

## Example Playbook

    - hosts: all
      roles:
        - role: geerlingguy.node_exporter

## License

MIT / BSD

## Author Information

This role was created in 2020 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
