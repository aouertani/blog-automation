Role Name: juice-shop
=========

This role juice shop application in juice-shop namepace.

Requirements
------------

This role needs the following roles to be run first:
* common
* cert-manager
* ingress-controller
* prometheus
* dns

Role Variables
--------------

```yaml
---
# Helm release name
juice_shop_release_name: "juice-shop"

# Helm repository name
juice_shop_repo_name: "juice-shop"


# Helm chart URL
juice_shop_repo_url: "https://github.com/aouertani/juice-shop-dep.git"

# Kubernetes namespace where juice-shop resources should be installed
juice_shop_namespace: "juice-shop"

# Path to clone juic-shop helm repository 
juice_shop_clone_repo: /tmp/demo_repo
```

Juice-Shop chart values in roles/juice-shop/templates/values.yaml used to configure the application.

Dependencies
------------

This role needs community.kubernetes collection. It can be set in roles/grafana/meta/main.yml
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
         - role: juice-shop
      tags: role-juice-shop
