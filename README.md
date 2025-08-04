# Ansible Role: Prometheus Release Uploader

This Ansible role automates the process of downloading Prometheus component release artifacts from GitHub and uploading them to a Nexus3 repository.

Supports both official `prometheus` and `prometheus-community` components, including platform-specific artifacts like `windows_exporter`.

---

## ✅ Features

- Downloads `.tar.gz` or `.zip` release artifacts and checksums from GitHub
- Uploads files to Nexus3 using `curl` (avoids Ansible's binary issues)
- Supports multiple Prometheus components with independent versioning
- Reusable, extensible, and easy to maintain
- Supports Linux and Windows exporters

---

## 🔧 Role Variables

### Required structure (defined in `defaults/main.yml`)

```yaml
component_sources:
  alertmanager: prometheus/alertmanager
  apache_exporter: Lusitaniae/apache_exporter
  node_exporter: prometheus/node_exporter
  prometheus: prometheus/prometheus
  pushgateway: prometheus/pushgateway
  blackbox_exporter: prometheus/blackbox_exporter
  mysqld_exporter: prometheus/mysqld_exporter
  snmp_exporter: prometheus/snmp_exporter
  memcached_exporter: prometheus/memcached_exporter
  graphite_exporter: prometheus/graphite_exporter
  statsd_exporter: prometheus/statsd_exporter
  systemd_exporter: prometheus-community/systemd_exporter
  windows_exporter: prometheus-community/windows_exporter
  postgres_exporter: prometheus-community/postgres_exporter

prometheus_components:
  - alertmanager
  - apache_exporter
  - node_exporter
  - prometheus
  - pushgateway
  - blackbox_exporter
  - mysqld_exporter
  - snmp_exporter
  - statsd_exporter
  - memcached_exporter
  - graphite_exporter
  - systemd_exporter
  - windows_exporter
  - postgres_exporter

# Version per component (example: map versions as needed)
prometheus_versions:
  alertmanager: "0.28.1"
  apache_exporter: "1.0.10"
  node_exporter: "1.9.1"
  prometheus: "2.53.5"
  pushgateway: "1.11.1"
  blackbox_exporter: "0.27.0"
  mysqld_exporter: "0.17.2"
  snmp_exporter: "0.29.0"
  memcached_exporter: "0.15.3"
  systemd_exporter: "0.7.0"
  windows_exporter: "0.25.1"
  graphite_exporter: "0.16.0"
  postgres_exporter: "0.17.1"
  statsd_exporter: "0.28.0"

nexus_base_url: "http://localhost:8081/repository/prometheus"
nexus_user: "admin"
nexus_password: "passwd" # save in the vault for when used in PROD
download_dir: "/tmp/prometheus_artifacts"
```

## 📦 Supported Artifacts
Component	GitHub Repo	Formats
alertmanager	prometheus/alertmanager	.tar.gz
node_exporter	prometheus/node_exporter	.tar.gz
prometheus	prometheus/prometheus	.tar.gz
systemd_exporter	prometheus-community/systemd_exporter	.tar.gz
windows_exporter	prometheus-community/windows_exporter	.zip

## 🚀 Example Playbook
```yaml
- name: Upload Prometheus releases to Nexus
  hosts: localhost
  gather_facts: no
  roles:
    - role: ansible-role-prometheus-upload-releases
```
## 🛠️ Requirements
- Nexus3 repository with support for raw file uploads via PUT
- Internet access to download GitHub releases
- curl installed on the host running the playbook

## 📈 Roadmap Ideas
- GitHub API integration to fetch latest versions dynamically
- SHA256 checksum validation before upload
- Multi-arch artifact handling (arm64, etc.)
- Role packaging for Ansible Galaxy

🧑‍💻 Author
---
[Ibrahim Musayev](https://github.com/codehunter-py)