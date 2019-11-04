Ansible Role: Gogs Git Server on OpenShift
[![Build Status](https://travis-ci.org/siamaksade/ansible-openshift-gogs.svg?branch=master)](https://travis-ci.org/siamaksade/ansible-openshift-gogs)
=========

Ansible Role for deploying Gogs Git Server on OpenShift. This role creates an admin 
account, a user account and also if configured would generate the specified number of user 
accounts for Gogs.


Role Variables
------------

| Variable                  | Default Value   | Description   |
|---------------------------|-----------------|---------------|
|`gogs_service_name`        | gogs            | Gogs service name on OpenShift  |
|`gogs_image_version`       | 0.11.29         | Gogs image version as available on [Docker Hub](https://hub.docker.com/r/openshiftdemos/gogs/tags/) |
|`gogs_route`               | gogs-{{ project_name }}.127.0.0.1.nip.io | **Required**. Gogs hostname to be configure |
|`gogs_admin_user`          | gogs            | Admin account username |
|`gogs_admin_password`      | gogs            | Admin account password |
|`gogs_user`                | developer       | User account username |
|`gogs_password`            | developer       | User account password |
|`gogs_generate_user_count` | 0               | Number of users accounts to generate with the user account password |
|`gogs_generate_user_format`| user%02d        | [printf style format](https://en.wikipedia.org/wiki/Printf_format_string) to use for generating user accounts |
|`gogs_database_version`    | 9.6             | Postgresql version used for gogs persistent template |
|`max_mem`                  | 2Gi             | Max memory allocated to Gogs container |
|`min_mem`                  | 512Mi           | Min memory allocated to Gogs container |
|`max_cpu`                  | 1               | Max cpu allocated to Gogs container |
|`min_cpu`                  | 200m            | Min cpu allocated to Gogs container |
|`clean_deploy`             | false           | Deploy a fresh Gogs and delete the existing one if any |
|`project_name`             | gogs            | OpenShift project name for the Gogs container  |
|`project_display_name`     | Gogs            | OpenShift project display name for the Gogs container  |
|`project_desc`             | Gogs Git Server | OpenShift project description for the Gogs container |
|`project_admin`            | -               | If set, the user to be assigned as project admin |
|`project_annotations`      | -               | OpenShift project annotations for the Gogs container |
|`openshift_cli`            | oc              | OpenShift CLI command and arguments (e.g. auth)       | 

OpenShift Version Compatibility
------------
When listing this role in `requirements.yml`, make sure to pin the version of the role via one of the tags:

```
- src: siamaksade.openshift_gogs
  version: 1.1.0
```  

The following tables shows the version combinations that are tested and verified:

| Role Version      | OpenShift Version |
|-------------------|-------------------|
| 1.0.x   | 3.7.x   |
| 1.1.x   | 3.9.x, 3.10.x, 3.11.x |
| 1.2.x   | 4.1.x, 4.2.x |

Note that if a version combination is not listed above, it does **NOT** mean that the latest role version 
won't work on that OpensShift version. The above table is merely the combinations that we have tested and verified.


Example Playbook
------------

```
name: Example Playbook
hosts: localhost
tasks:
- import_role:
    name: siamaksade.openshift_gogs
  vars:
    gogs_route: "gogs-cicd-project.apps.myopenshift.com"
    project_name: "cicd-project"
    gogs_generate_user_count: "50"
    openshift_cli: "oc --server http://master:8443"
```
