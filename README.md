# awscli

[![License](https://img.shields.io/badge/license-New%20BSD-blue.svg?style=flat)](https://raw.githubusercontent.com/saucelabs-ansible/awscli/master/LICENSE)
[![Build Status](https://travis-ci.org/saucelabs-ansible/awscli.svg?branch=master)](https://travis-ci.org/saucelabs-ansible/awscli)

[![Platform](http://img.shields.io/badge/platform-ubuntu-dd4814.svg?style=flat)](#)

[![Project Stats](https://www.openhub.net/p/saucelabs-ansible-awscli/widgets/project_thin_badge.gif)](https://www.openhub.net/p/saucelabs-ansible-awscli/)

Ansible role to install the [AWS command line interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) and
configure the credentials and profiles.


## Tests

| Family | Distribution | Version | Test Status |
|:-:|:-:|:-:|:-:|
| Debian | Ubuntu  | Precise | [![x86](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |
| Debian | Ubuntu  | Trusty  | [![x86](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) [![x86_64](http://img.shields.io/badge/x86_64-passed-006400.svg?style=flat)](#) |


## Requirements

- ansible >= 1.9.4


## Role Variables

- **debug**: flag to run debug tasks.
- **awscli_configuration**: dictionary containing a series of AWS profile configuration options.
- **awscli_profiles**: dictionary containing a series of AWS profile credentials.
- **awscli_users**: dictionary containing a series of user accounts and
                    their profiles and configuration.
- **awscli_version**: the version to be installed.
- **awscli_virtualenv**: in case the tool is to be installed on a virtualenv,
                         use this parameter to specify the path.

Unless stated otherwise
a default value is provided for each of the variables mentioned above
in `defaults/main.yml`.


## Dependencies

- [saucelabs-ansible.pip](https://github.com/saucelabs-ansible/pip)


## Playbooks

Example:

    - hosts: servers
      vars:
        debug: yes

      roles:
        - role: awscli
          awscli_profiles:
            default:
              aws_access_key_id: AKIAIOSFODNN7EXAMPLE
              aws_secret_access_key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
            user2:
              aws_access_key_id: AKIAI44QH8DHBEXAMPLE
              aws_secret_access_key: je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
          awscli_configuration:
            default:
              region: us-west-2
              output: json
            conf_east1_text:
              region: us-east-1
              output: text
          awscli_users:
            ec2-user:
              profiles:
                - default
              configuration:
                - default
            vagrant:
              profiles:
                - default
                - user2
              configuration:
                default: default
                user2: conf_east1_text
           tags: [ awscli ]

This would generate the following configuration files:

    # ~ec2-user/.aws/credentials
    [default]
    aws_access_key_id=AKIAIOSFODNN7EXAMPLE
    aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

    # ~ec2-user/.aws/config
    [default]
    region=us-west-2
    output=json


    # ~vagrant/.aws/credentials
    [default]
    aws_access_key_id=AKIAIOSFODNN7EXAMPLE
    aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

    [user2]
    aws_access_key_id=AKIAI44QH8DHBEXAMPLE
    aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY

    # ~vagrant/.aws/config
    [default]
    region=us-west-2
    output=json

    [profile user2]
    region=us-east-1
    output=text


## Tags

- **configuration**: configuration tasks.
- **debug**: task to debug role variables.
- **installation**: installation tasks.
- **validation**: task to validate role variables.


## Test

To run the tests you will need to install:

- [tox](https://tox.readthedocs.org/)
- [vagrant](https://www.vagrantup.com/)

To run all tests against all pre-defined OS/distributions * ansible versions:

```
$ tox
```

To run tests for `trusty64`:

```
$ cd tests
$ bash test_idempotence.sh --box trusty64.vagrant.dev
# log file will be stores under tests/log
```

To perform debugging on a specific environment:

```
$ cd tests
$ vagrant up trusty64.vagrant.dev

# to provision using the test.yml playbook (as many time as you need)
$ vagrant provision trusty64.vagrant.dev

# to access the Vagrant box
$ vagrant ssh trusty64.vagrant.dev
```


## Links

- [AWS command line interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)
- [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)


## Author Information

- [steenzout](https://github.com/steenzout/)
