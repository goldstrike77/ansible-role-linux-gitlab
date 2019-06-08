![](https://img.shields.io/badge/Ansible-GitLab-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_gitlab.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [GitLab Versions](#GitLab-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
This Ansible role installs GitLab on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### GitLab versions

The following list of supported the GitLab releases:

* GitLab 11.11+

## Role variables
### Main parameters
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `gitlab_client_max_body_size`: Specify the max attachment file size.
* `gitlab_data_path`: This directory is used to store Git data.
* `gitlab_domain`: Defines domain name.
* `gitlab_edition`: 'ce' (Community Edition) or 'ee' (Enterprise Edition)
* `gitlab_enable_https`: Communicate with GitLab instance over a secure connection.
* `gitlab_root_password`:Default root password.
* `gitlab_selinux`: SELinux security policy.
* `gitlab_time_zone`: Defines global timezone. 
* `gitlab_version`: Specify the GitLab version.

##### Listen port
* `gitlab_port_arg.http`:HTTP listen port.
* `gitlab_port_arg.https`: HTTPs listen port.
* `gitlab_port_arg.exporter`: Exporter listen port.

##### Service Mesh
* `environments`: Define the service environment.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

##### Redis parameters
* `gitlab_redis_dept`: A boolean value, whether installs external Redis.
* `gitlab_redis_host`:  Redis server address.
* `gitlab_redis_maxmemory`: A memory usage limit to the specified amount in MB.
* `gitlab_redis_path`: Specify the Redis data directory.
* `gitlab_redis_port`: Redis server port.
* `gitlab_redis_requirepass`: Authorization clients password.
* `gitlab_redis_ssl`: Enables or disables Redis SSL encryption.

##### PostgreSQL parameters
* `gitlab_pgsql_dept`: A boolean value, whether installs external PostgreSQL.
* `gitlab_pgsql_host`: PostgreSQL instance communication address.
* `gitlab_pgsql_mailto`:PostgreSQL report mail recipient.
* `gitlab_pgsql_path`: Specify the PostgreSQL data directory.
* `gitlab_pgsql_port`: PostgreSQL instance communication ports.
* `gitlab_pgsql_sa_pass`: PostgreSQL root account password.
* `gitlab_pgsql_bu_dbs_arg`: GitLab Database Variables.

##### Email parameters
* `gitlab_email_arg`: Define mail parameters.

##### SMTP parameters
* `gitlab_smtp_arg`: Define SMTP server parameters

## Dependencies
- Ansible versions > 2.6 are supported.
- [PostgreSQL](https://github.com/goldstrike77/ansible-role-linux-postgresql.git)
- [Redis](https://github.com/goldstrike77/ansible-role-linux-redis.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10' gitlab_version='11.11.2'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-gitlab
           gitlab_version: '11.11.2'

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    gitlab_client_max_body_size: '250m'
    gitlab_data_path: '/data'
    gitlab_domain: 'gitlab.example.com'
    gitlab_edition: 'ce' # 'ce'
    gitlab_enable_https: true
    gitlab_root_password: 'changeme'
    gitlab_selinux: false
    gitlab_time_zone: 'Asia/Shanghai'
    gitlab_version: '11.11.2'
    gitlab_port_arg:
      http: '80'
      https: '443'
      exporter: '9168'
    gitlab_redis_dept: true
    gitlab_redis_host: '127.0.0.1'
    gitlab_redis_maxmemory: '128'
    gitlab_redis_path: '{{ gitlab_data_path }}'
    gitlab_redis_port: '6379'
    gitlab_redis_requirepass: 'changeme'
    gitlab_redis_ssl: false
    gitlab_pgsql_dept: true
    gitlab_pgsql_host: '127.0.0.1'
    gitlab_pgsql_mailto: 'somebody@example.com'
    gitlab_pgsql_path: '{{ gitlab_data_path }}'
    gitlab_pgsql_port: '5432'
    gitlab_pgsql_sa_pass: 'changeme'
    gitlab_pgsql_bu_dbs_arg:
      - dbs: 'gitlabhq_production'
        user: 'gitlab'
        pass: 'changeme'
        priv: 'ALL'
        role: 'SUPERUSER'
    gitlab_email_arg:
      enabled: true
      from: 'gitlab@example.com'
      display_name: 'GitLab example'
      reply_to: 'do-not-reply@example.com'
      subject_suffix: ''
    gitlab_smtp_arg:
      enable: true
      address: 'localhost'
      port: 25
      user_name: 'smtp user'
      password: 'smtp password'
      domain: 'localhost'
      authentication: 'login'
      tls: false
      openssl_verify_mode: 'none'
      enable_starttls_auto: false
      ssl: false
      force_ssl: false
    environments: 'SIT'
    consul_public_register: false
    consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
    consul_public_clients: 'localhost'
    consul_public_http_port: '8500'

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
