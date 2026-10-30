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

A circuit is a reusable setup for a set of hosts, versioned like any OpenTofu module. Add one `module` block, point it at your hosts, and plan. Each one's page says what it was tested on; most run on Ubuntu 24.04, Debian 13, Fedora 45 and Rocky Linux 10 in CI on every change, and each has its own thread in [Discussions](https://github.com/conn3xorg/conn3x/discussions/categories/circuits).

There are 72 official circuits so far:

- **Web and proxies:** [nginx](https://docs.conn3x.org/circuits/nginx), [Apache httpd](https://docs.conn3x.org/circuits/apache), [Caddy](https://docs.conn3x.org/circuits/caddy), [HAProxy](https://docs.conn3x.org/circuits/haproxy), [Traefik](https://docs.conn3x.org/circuits/traefik), [certbot](https://docs.conn3x.org/circuits/certbot)
- **Databases and caches:** [PostgreSQL](https://docs.conn3x.org/circuits/postgres), [MariaDB](https://docs.conn3x.org/circuits/mariadb), [MySQL](https://docs.conn3x.org/circuits/mysql), [SQL Server](https://docs.conn3x.org/circuits/mssql), [CouchDB](https://docs.conn3x.org/circuits/couchdb), [Valkey](https://docs.conn3x.org/circuits/valkey), [OpenSearch](https://docs.conn3x.org/circuits/opensearch)
- **Monitoring:** [node_exporter](https://docs.conn3x.org/circuits/node-exporter), [Prometheus](https://docs.conn3x.org/circuits/prometheus), [Grafana](https://docs.conn3x.org/circuits/grafana), [Monitoring stack](https://docs.conn3x.org/circuits/monitoring), [Netdata](https://docs.conn3x.org/circuits/netdata), [OpenTelemetry Collector](https://docs.conn3x.org/circuits/otelcol), [Zabbix](https://docs.conn3x.org/circuits/zabbix)
- **Containers and Kubernetes:** [Docker](https://docs.conn3x.org/circuits/docker), [Podman](https://docs.conn3x.org/circuits/podman), [k3s](https://docs.conn3x.org/circuits/k3s), [kubeadm](https://docs.conn3x.org/circuits/kubeadm), [MicroShift](https://docs.conn3x.org/circuits/microshift)
- **DNS:** [Pi-hole](https://docs.conn3x.org/circuits/pihole), [AdGuard Home](https://docs.conn3x.org/circuits/adguardhome), [Unbound](https://docs.conn3x.org/circuits/unbound), [BIND](https://docs.conn3x.org/circuits/bind)
- **Networking and VPN:** [WireGuard](https://docs.conn3x.org/circuits/wireguard), [Tailscale](https://docs.conn3x.org/circuits/tailscale), [keepalived](https://docs.conn3x.org/circuits/keepalived)
- **Security and compliance:** [Baseline](https://docs.conn3x.org/circuits/baseline), [Hardening](https://docs.conn3x.org/circuits/hardening), [Firewall](https://docs.conn3x.org/circuits/firewall), [fail2ban](https://docs.conn3x.org/circuits/fail2ban), [Automatic updates](https://docs.conn3x.org/circuits/auto-updates), [OpenSCAP](https://docs.conn3x.org/circuits/openscap), [auditd](https://docs.conn3x.org/circuits/auditd)
- **Storage and backup:** [Samba](https://docs.conn3x.org/circuits/samba), [NFS](https://docs.conn3x.org/circuits/nfs), [restic](https://docs.conn3x.org/circuits/restic)
- **Identity and secrets:** [Users](https://docs.conn3x.org/circuits/users), [LLDAP](https://docs.conn3x.org/circuits/lldap), [OpenLDAP](https://docs.conn3x.org/circuits/openldap), [FreeIPA](https://docs.conn3x.org/circuits/freeipa), [Keycloak](https://docs.conn3x.org/circuits/keycloak), [OpenBao](https://docs.conn3x.org/circuits/openbao), [Infisical](https://docs.conn3x.org/circuits/infisical)
- **AI:** [Ollama](https://docs.conn3x.org/circuits/ollama), [Qdrant](https://docs.conn3x.org/circuits/qdrant), [Open WebUI](https://docs.conn3x.org/circuits/openwebui), [LiteLLM](https://docs.conn3x.org/circuits/litellm), [Presidio](https://docs.conn3x.org/circuits/presidio), [Langfuse](https://docs.conn3x.org/circuits/langfuse), [Agent box](https://docs.conn3x.org/circuits/agentbox)
- **Platforms:** [Proxmox VE](https://docs.conn3x.org/circuits/proxmox), [Forgejo](https://docs.conn3x.org/circuits/forgejo), [GitLab CE](https://docs.conn3x.org/circuits/gitlab), [Cockpit](https://docs.conn3x.org/circuits/cockpit), [n8n](https://docs.conn3x.org/circuits/n8n)
- **Build and runtimes:** [Java](https://docs.conn3x.org/circuits/java), [PHP-FPM](https://docs.conn3x.org/circuits/php), [Jenkins](https://docs.conn3x.org/circuits/jenkins), [GitHub Actions runner](https://docs.conn3x.org/circuits/github-runner), [GitLab Runner](https://docs.conn3x.org/circuits/gitlab-runner)
- **Communication and media:** [Matrix Synapse](https://docs.conn3x.org/circuits/matrix), [Jellyfin](https://docs.conn3x.org/circuits/jellyfin)
- **Detection and response:** [Suricata](https://docs.conn3x.org/circuits/suricata), [Zeek](https://docs.conn3x.org/circuits/zeek), [osquery](https://docs.conn3x.org/circuits/osquery), [Velociraptor](https://docs.conn3x.org/circuits/velociraptor)

Wrote one of your own? [Share it](https://docs.conn3x.org/guides/share-a-circuit) and we'll list it on [conn3x.org/circuits](https://conn3x.org/circuits).

## Get involved

Questions and ideas go to [Discussions](https://github.com/conn3xorg/conn3x/discussions). If you'd like to send a fix, issues labelled [good first issue](https://github.com/search?q=org%3Aconn3xorg+label%3A%22good+first+issue%22+state%3Aopen&type=issues) and [help wanted](https://github.com/search?q=org%3Aconn3xorg+label%3A%22help+wanted%22+state%3Aopen&type=issues), across Conn3x and every circuit, are a good place to start, and [CONTRIBUTING.md](https://github.com/conn3xorg/conn3x/blob/main/CONTRIBUTING.md) covers the rest. Found a security problem? Email security@conn3x.org rather than opening an issue.
