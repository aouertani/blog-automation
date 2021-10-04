Role Name: grafana
=========

This role:
* deploys grafana in monitoring namespace
* creates an ingress with letsencrypt including a valid certificate
* creates a prometheus data source
* create an admin user
* loads Juice Shop Status Dashboard from roles/grafana/files/demo-grafana-dashboard.json

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
grafana_release_name: "grafana"

# Helm repository name
grafana_repo_name: "grafana"

# Helm chart name
grafana_chart_name: "{{ grafana_repo_name }}/{{ grafana_release_name }}"

# Helm chart URL
grafana_chart_url: "https://grafana.github.io/helm-charts"

# Kubernetes namespace where grafana resources should be installed
grafana_namespace: "monitoring"

# Grafana ingress params 
letsencrypt: letsencrypt-prod
grafana_host: "{{ grafana_dns }}"
secret_name: garafana-cert
service_name: grafana
service_port: 80

# Prometheus data source details
data_source_name: Prometheus
data_source_type: prometheus
data_source_url: http://prometheus-server 

# Users credentials
# Admin user
admin_username: admin
# Created user info
user_name: "Bruce Wayne"
user_email: batman@gotham.city
user_login: batman
user_password: changme
```
Grafana chart values in roles/grafana/templates/values.yaml
```yaml
---
persistence:
  type: pvc
  enabled: true
  size: 10Gi

deploymentStrategy:
  type: Recreate
```
Dependencies
------------

This role needs community.kubernetes collection. It can be set in roles/grafana/meta/main.yml
```yaml
---
  dependencies:
    "community.kubernetes": "*"
    "community.grafana": "*"
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: grafana
      tags: role-grafana
