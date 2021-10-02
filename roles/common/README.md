Role Name: common
=========

This role:
* checks if kubectl, gcloud and helm are installed
* authenticates to GKE using a service account key file and sets the current context.

Requirements
------------

This role does not have any dependencies and can be run separately.

Role Variables
--------------

```yaml
---
cluster_id: cluster-5
cluster_zone: europe-central2
gcp_project_id: curious-furnace-316611

service_account_key_file: "{{ lookup('env', 'GOOGLE_APPLICATION_CREDENTIALS') }}"
```

Dependencies
------------

This role needs GOOGLE_APPLICATION_CREDENTIALS to be set. It should point to a service account key file, instructions to generate the file are explained here [here](https://cloud.google.com/docs/authentication/production#automatically)


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: cert-common
      tags: role-common
