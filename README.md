Restore Jenkins Configuration from SCM
======================================

This role can be used to save or restore Jenkins configuration from SCM.

Requirements
------------

* Ansible 2.4
* Jenkins with the `scm-sync-configuration` plugin.
* Git installed and configured.

Parameters
----------

* `JENKINS_URL`: URL where Jenkins is available.
* `JENKINS_ADMIN_USERNAME`, `JENKINS_ADMIN_PASSWORD`: username and password for Jenkins admin user.
* `JENKINS_SCMSYNC_URL`: repository where Jenkins configuration is stored.
  Right now only Git repositories are supported with the following limitations:
  - Only HTTPS access to the repository is supported. Credentials should be provided as follows: https://username:password_or_token@git.example.com/jenkins-configuration.git
  - Only default branch can be used.
* `JENKINS_SCMSYNC_MESSAGE_PATTERN`: pattern for commit messages.
* `JENKINS_SCMSYNC_ADDITIONAL_INCLUDES`: list of files to additionally include to the repository.
  Credentials and secret files are already included in `jenkins_scmsync_default_includes` variable.

Example
-------

```yaml
- hosts: jenkins
  vars:
    JENKINS_PORT: 8080
    JENKINS_URL: "http://{{ inventory_hostname }}:{{ JENKINS_PORT }}"
    JENKINS_ADMIN_USERNAME: admin
    JENKINS_ADMIN_PASSWORD: securePassword
    JENKINS_PLUGINS:
      - scm-sync-configuration

    JENKINS_SCMSYNC_URL: https://user:pass@git.example.com/jenkins-conf.git
    JENKINS_SCMSYNC_ADDITIONAL_INCLUDES:
      - custom/*
      - com.dabsquared.gitlabjenkins.connection.GitLabConnectionConfig.xml
      - org.jenkins.plugins.lockableresources.LockableResourcesManager.xml
      - xml-config-files.xml

  roles:
    - role: jenkins
    - role: valerius257.jenkins-restore-configuration
```
