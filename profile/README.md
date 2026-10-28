<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://conn3x.org/brand/conn3x-logo-dark-bg.svg">
    <img alt="Conn3x" src="https://conn3x.org/brand/conn3x-logo-light-bg.svg" width="280">
  </picture>
</p>

<p align="center">
  <a href="https://conn3x.org">Website</a> ·
  <a href="https://docs.conn3x.org/get-started">Get started</a> ·
  <a href="https://conn3x.org/circuits">Circuits</a> ·
  <a href="https://conn3x.org/blog">Blog</a> ·
  <a href="https://github.com/conn3xorg/conn3x/discussions">Discussions</a>
</p>

Setting up a server usually takes two tools. Terraform or OpenTofu creates the machine, Ansible configures it, and an inventory file and a pile of glue scripts hold the two together. [Conn3x](https://github.com/conn3xorg/conn3x) does the whole job from one binary, with one plan and one state. It's built on OpenTofu, so the providers and modules you already use keep working.

```hcl
circuit "web" {
  hosts = { for name, s in digitalocean_droplet.web : name => s.ipv4_address }

  ssh {
    user             = "root"
    private_key_file = "~/.ssh/id_ed25519"
  }

  step "nginx" {
    package {
      name = "nginx"
    }
  }
}
```

Run `conn3x plan` and you see the droplets it will create and the packages, files and services it will change on them, together. Run it again next week and it shows you what drifted.

## Start here

- **[conn3x](https://github.com/conn3xorg/conn3x)**: the binary. Linux and macOS, signed releases, `brew install conn3xorg/tap/conn3x`.
- **[Try it locally](https://docs.conn3x.org/tutorials/try-locally)**: a container stands in for a server, so you need no cloud account.
- **[Coming from Ansible](https://docs.conn3x.org/guides/migrate-from-ansible)** or **[from Terraform](https://docs.conn3x.org/guides/migrate-from-terraform)**: what maps to what.

## Circuits

A circuit is a reusable setup for a set of hosts, versioned like any OpenTofu module. Add one `module` block, point it at your hosts, and plan. Each one here is tested on Ubuntu 24.04, Debian 13, Fedora 45 and Rocky Linux 10 on every change.

| | |
| --- | --- |
| [circuit-k3s](https://github.com/conn3xorg/circuit-k3s) | k3s servers and agents, HA with embedded etcd, Raspberry Pi OS |
| [circuit-kubeadm](https://github.com/conn3xorg/circuit-kubeadm) | Upstream Kubernetes with kubeadm and containerd, HA control planes |
| [circuit-docker](https://github.com/conn3xorg/circuit-docker) | Docker Engine from Docker's repository, Compose stacks from your config |
| [circuit-monitoring](https://github.com/conn3xorg/circuit-monitoring) | Prometheus, Grafana and cAdvisor as one stack |
| [circuit-firewall](https://github.com/conn3xorg/circuit-firewall) | Default-deny ufw or firewalld per host, keeping SSH open |
| [circuit-baseline](https://github.com/conn3xorg/circuit-baseline) | Time zone, chrony, network sysctls and key-only SSH |
| [circuit-users](https://github.com/conn3xorg/circuit-users) | Users, groups, SSH keys and sudo from one map |
| [circuit-nginx](https://github.com/conn3xorg/circuit-nginx) | nginx with a site, a reload handler and an open firewall port |
| [circuit-node-exporter](https://github.com/conn3xorg/circuit-node-exporter) | Prometheus node_exporter at a pinned, checksummed version |
| [circuit-caddy](https://github.com/conn3xorg/circuit-caddy) | Caddy as a web server or reverse proxy, from a map of sites |
| [circuit-postgres](https://github.com/conn3xorg/circuit-postgres) | PostgreSQL with settings, access rules, users and databases |
| [circuit-podman](https://github.com/conn3xorg/circuit-podman) | Podman, with your containers run as systemd services through Quadlet |
| [circuit-tailscale](https://github.com/conn3xorg/circuit-tailscale) | Tailscale, joined to your tailnet, with routes and exit nodes |
| [circuit-fail2ban](https://github.com/conn3xorg/circuit-fail2ban) | Bans repeated SSH login failures, plus jails of your own |
| [circuit-auto-updates](https://github.com/conn3xorg/circuit-auto-updates) | Automatic package updates with a reboot window |

Wrote one of your own? [Share it](https://docs.conn3x.org/guides/share-a-circuit) and we'll list it on [conn3x.org/circuits](https://conn3x.org/circuits).

## Get involved

Questions and ideas go to [Discussions](https://github.com/conn3xorg/conn3x/discussions). If you'd like to send a fix, issues labelled [good first issue](https://github.com/conn3xorg/conn3x/labels/good%20first%20issue) and [help wanted](https://github.com/conn3xorg/conn3x/labels/help%20wanted) are a good place to start, and [CONTRIBUTING.md](https://github.com/conn3xorg/conn3x/blob/main/CONTRIBUTING.md) covers the rest. Found a security problem? Email security@conn3x.org rather than opening an issue.
