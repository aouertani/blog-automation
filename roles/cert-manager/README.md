Role Name: cert-manager
=========

This role deploys cert manager using its helm chart

Requirements
------------

This role does not have any dependencies and can be run separately.

Role Variables
--------------

```yaml
---
# Helm chart version
cert_manager_chart_version: "v1.3.1"

# Helm release name
cert_manager_release_name: "cert-manager"

# Helm repository name
cert_manager_repo_name: "jetstack"

# Helm chart name
cert_manager_chart_name: "{{ cert_manager_repo_name }}/{{ cert_manager_release_name }}"

# Helm chart URL
cert_manager_chart_url: "https://charts.jetstack.io"

# Kubernetes namespace where cert-manager resources should be installed
cert_manager_namespace: "cert-manager"

# The following table contains the configurable parameters of the cert-manager
cert_manager_values:
   installCRDs: true
   prometheus.servicemonitor.enabled: true
```

Dependencies
------------

This role needs community.kubernetes collection. It can be set in roles/cert-manager/meta/main.yml
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
         - role: cert-manager
      tags: role-cert-manager
