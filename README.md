# aws-eapamq-demo

This repository contains reference configurations that can create the needed resources on AWS and deploy on them a cross-region clustered Jboss EAP service with the JMS remoting to a cross-region AMQ broker cluster. The demo relies on ansible [middleware automation collections](https://ansible-middleware.github.io/) for wildfly/EAP and activemq/AMQ Broker.

The final architecture looks like:

![Architecture diagram](./scenario.svg)


## Prerequisites

* Red Hat Enterprise Linux (RHEL) 8 controller host (or Fedora works too); preferably a bastion ec2 instance
* Red Hat Ansible >= 2.11 / python >= 3.9
* python dependencies installed on the controller host `pip install -r requirements.txt`
* AWS-CLI installed on the controller host
* an AWS account capable of:
  * vpc:*
  * ec2:*
  * route53:*
  * efs:* (only if setting up backup amq brokers with shared filesystem)
* an ssh keypair installed in AWS to use for creating EC2 instances
* if exposing the loadbalancer publicly, the TLS certificate and key in the files/ directory, named _public_fqdn_.key and _public_fqdn_.crt


### Install Ansible Dependencies

`ansible-galaxy collection install -r requirements.yml`


## Running on AWS cloud

### Infrastracture

1. Make sure ansible and AWS account are operational:

`ansible-inventory --list`

This should return empty before running the infra.

2. Execute the main play:
```
ansible-playbook infra.yml
```
Note: open `ansible.cfg` config file and edit `remote_user` and `remote_privkey` accordingly to your configuration.

3. Now inspect the inventory:

`$ ansible-inventory --list`

```
    "all": {
        "children": [
            "amq",
            "aws_ec2",
            "eap",
            "loadbalancer",
            "site1",
            "site2",
            "ungrouped",
            "us_east_1",
            "us_west_2"
        ]
    },
    "amq": {
        "hosts": [
            "site1-amq1",
            "site1-amq2",
            "site2-amq1",
            "site2-amq2"
        ]
    },
    "eap": {
        "hosts": [
            "site1-eap1",
            "site1-eap2",
            "site2-eap1",
            "site2-eap2"
        ]
    },
    "loadbalancer": {
        "hosts": [
            "site1-loadbalancer",
            "site2-loadbalancer"
        ]
    },
    "site1": {
        "hosts": [
            "site1-amq1",
            "site1-amq2",
            "site1-eap1",
            "site1-eap2",
            "site1-loadbalancer"
        ]
    },
    "site2": {
        "hosts": [
            "site2-amq1",
            "site2-amq2",
            "site2-eap1",
            "site2-eap2",
            "site2-loadbalancer"
        ]
    },

[...]
```

We use a dynamic ansible inventory that relies on AWS tagging to build the node list and determine configurations, available at `inventory/myaws_ec2.yml`; and a default ansible configuration at: `./ansible.cfg`;
by default, ansible ssh connectivity uses EC2 instances public IP addresses, but, if you intend to run on a separate EC2 bastion (recommended), we advice the edit the dynamic inventory like so:

```
[...]
compose:
  ansible_host: private_ip_address
```

in order to use instance's private IP address. This saves time, bandwidth and money.


### Service deployment

1. Create a var-file containing your RHN credentials:
```
$ cat rhn-creds.yml
rhn_username: '<username>'
rhn_password: '<password>'
```

2. Execute the full deployment play

```
ansible-playbook -e @rhn-creds.yml deploy.yml
```

### Verify deployed demo

1) Open your browser to: https://<your_loadbalacer>/iot-example/ for the consumer UI
2) `curl https://<your_loadbalacer>/iot-example/DeviceServletClient?msg=500&interval=50` to produce 500 messages with 50 msec delay
3) watch the consumer UI populate with data
4) (optional) check metrics grafana 


## License

Apache License v2.0 or later

See [LICENCE](LICENSE) to view the full text.


## Authors


* Guido Grazioli <ggraziol@redhat.com>
* Shaaf Syed <sshaaf@redhat.com>
* and the rest of Middleware Automation team
