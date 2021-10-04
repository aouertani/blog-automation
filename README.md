# blog-automation

A set of ansible roles and playbooks to automatically deploy [blog](https://github.com/aouertani/blog/tree/docker) application and its ecosystem in Google Cloud Platform (GKE).
Each role manages a different componenet and can be used separately:

* common: Checks if kubectl, gcloud and helm are installed first and then authenticates to GKE using a service account key file and sets the current context.
* cert-manager: Deploys cert-manager in cert-manager namespace.
* ingress-controller: Deploys ingress-controller in cert-manager name space and creates let's encrypt staging and production issuers.
* blog: Deploys juice shop application in juice-shop namepace.
* prometheus: Deploys prometheus in monitoring namespace.
* grafana: Deploys grafana in monitoring namespace. Creates a prometheus data source and a user.

Each role is described with more details on its README.md file.

```bash
.
├── LICENSE
├── README.md
├── blog.yml
└── roles
    ├── blog
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    ├── cert-manager
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    ├── common
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   ├── main.yml
    │   │   └── prerequisites.yml
    │   ├── templates
    ├── grafana
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   │   └── values.yaml
    ├── ingress-controller
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    └── prometheus
        ├── README.md
        ├── defaults
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        ├── templates
        │   ├── promethues-nfs-pv.yaml
        │   ├── promethues-nfs-pvc.yaml
        │   └── values.yaml
        └── vars
            └── main.yml
```

# Prerequisites
* ansible, helm, kubectl and gcloud should be installed in the machine that runs the playbooks
* A GCP project and a running GKE cluster 
* GOOGLE_APPLICATION_CREDENTIALS should contain the path to a service account key file: [instructions here](https://cloud.google.com/docs/authentication/production#automatically)
* A cloud DNS for the applicat example.com
* Install MySQL operator: https://github.com/mysql/mysql-operator
* Create a DB schema named laravel

# Deploy blog in GKE
To deploy the application and its ecosystem:

* roles/common/defaults/main.yml should look like this:
```yaml
---
# defaults file for common
service_account_key_file: "{{ lookup('env', 'GOOGLE_APPLICATION_CREDENTIALS') }}"

cluster_id: cluster-5
cluster_zone: europe-central2
gcp_project_id: curious-furnace-316611
```

* Once the required vars are configured, run the main playbook:
```bash
ansible-playbook blog.yml
```
* Any role can be executed separately by specifying its tag:
```bash
ansible-playbook --tags=role-<role_name>  juice-shop.yml
```
* When the whole roles are successfully run the output must look like this:
```bash
TASK [Juice Shop application URL] ********************************************************************************************************************************
task path: ..../juice-shop-automation/juice-shop.yml:21
ok: [localhost] => {
    "msg": "Blog application URL: https://example.com"
}
```

# Todos:
* Rebuild the image of the app to scrape data and export them to prometheus
* Create dashboard in grafana using those data to monitor the app
* expose grafana with to be accessible from outside the cluster
* add a role to mange the DNS zone 
* add a role to automate the install and expose mysql operator
* Manage secrets in gcp secrets (vault)

