Role Name: prometheus
=========

This role deploys prometheus using its helm chart

Requirements
------------

This role does not have any dependencies and can be run separately.

Role Variables
--------------

```yaml
---
# Helm release name
prometheus_release_name: "prometheus"

# Helm repository name
prometheus_repo_name: "prometheus-community"

# Helm chart name
prometheus_chart_name: "{{ prometheus_repo_name }}/{{ prometheus_release_name }}"

# Helm chart URL
prometheus_chart_url: "https://prometheus-community.github.io/helm-charts"

# Kubernetes namespace where prometheus resources should be installed
prometheus_namespace: "monitoring"

# The following table contains the configurable parameters of the prometheus
prometheus_values:
   server.persistentVolume.size: 20Gi
   server.retention: 10d
```

Dependencies
------------

This role needs community.kubernetes collection. It can be set in roles/prometheus/meta/main.yml
```yaml
---
  dependencies:
    "community.kubernetes": "*"
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: prometheus
      tags: role-prometheus
