Role Name: ingress-controller
=========

This role deploys nginx ingress-controller using its helm chart in cert-manager name space and creates let's encrypt staging and production issuers.

Requirements
------------

This role does not have any dependencies and can be run separately.

Role Variables
--------------

```yaml
---
# Helm release name
ingress_controller_release_name: "ingress-nginx"

# Helm repository name
ingress_controller_repo_name: "ingress-nginx"

# Helm chart name
ingress_controller_chart_name: "{{ ingress_controller_repo_name }}/{{ ingress_controller_release_name }}"

# Helm chart URL
ingress_controller_chart_url: "https://kubernetes.github.io/ingress-nginx"

# Kubernetes namespace where cert-manager resources should be installed
ingress_controller_namespace: "cert-manager"

# The following table contains the configurable parameters of the cert-manager
#ingress_controller_values:
#  - key=value
cert_manager_le_clusterissuer_options:
  - name: letsencrypt-prod
    email: batman@gmail.com
    server: acme-v02
    private_key_secret_ref_name: letsencrypt-prod-account-key
    solvers_http01_ingress_class: "nginx"
  - name: letsencrypt-staging
    email: batman@gmail.com
    server: acme-staging-v02
    private_key_secret_ref_name: letsencrypt-staging-account-key
    solvers_http01_ingress_class: "nginx"

```

Dependencies
------------

This role needs cert-manager role to be run first and community.kubernetes collection. It can be set in roles/ingress-controller/meta/main.yml
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
         - role: ingress-controller
      tags: role-ingress-controller
