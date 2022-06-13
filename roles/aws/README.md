AWS role
========

Requirements
------------

The role handles create instances and configures then on AWS cloud.

<!--start argument_specs-->
Role Defaults
-------------

* No defaults


Role Variables
--------------

| Variable | Description | Default |
|:---------|:------------|:--------|
|`virtualnetwork_name`| Name of the network interface. | `ansible-middileware-vn`|
|`subnet_name`| Name of the subnet network interface. | `ansible-middileware-subnet`|
|`securitygroup_name`| Unique name for the rule. | `ansible-middileware-sg`|
|`admin_username`| Azure VM username. | `azureuser`|
|`admin_password`| Azure VM password. | `Password12345`|
|`offer`| The image used to build the VM. | `RHEL`|
|`publisher`| VM image publisher. | `RedHat`|
|`sku`| VM image SKU. | `8-LVM`|
|`image_version`| Verison of the Image. | `latest`|
|`vm_size`| A valid Azure VM size value. Choices vary depending on the subscription and location. `Standard_D2s_v3`|
|`publicIP`| Name of the Public IP.. | `ansible-middileware-ip`|
|`public_ip_address_name`| Name of the Public IP address name. | `ansible-middileware-public-ip`|
|`domain_name`| The customizable portion of the FQDN assigned to public IP address. This is an explicit setting. If no value is provided, any existing value will be removed on an existing public IP. | `rhsso`|
|`privatednszone`| Name of the private DNS zone. | `internal.ansiblemiddleware.com`|
|`vn_peering_name`| Name of the VM Peering. | `eastus-centralus-peering`|
|`allow_virtual_network_access`| Allows VMs in the remote VNet to access all VMs in the local VNet. | `true`|
|`allow_forwarded_traffic`| Allows forwarded traffic from the VMs in the remote VNet. | `true`|
|`traffic_manager_name`| Name of the Traffic Manager profile. | `rhsso-ansible-middleware`|
|`traffic_manager_resource_group`| Name of a resource group where the Traffic Manager profile exists or will be created. | `None`|
|`profile_status`| The status of the Traffic Manager profile. | `Enabled`|
|`traffic_manager_relative_name`| The relative DNS name provided by this Traffic Manager profile. | `rhsso-ansible-middleware`|
|`ttl`| The DNS Time-To-Live (TTL), in seconds. | `60`|
|`protocol`| The protocol (HTTP, HTTPS or TCP) used to probe for endpoint health. | `HTTPS`|
|`port`| The TCP port used to probe for endpoint health. | `443`|
|`path`| The path relative to the endpoint domain name used to probe for endpoint health. | `/`|
|`timeout_in_seconds`| The monitor timeout for endpoints in this profile.. | `10`|
|`interval_in_seconds`| The monitor interval for endpoints in this profile. | `30`|
|`tolerated_number_of_failures`| The number of consecutive failed health check before declaring an endpoint in this profile Degraded after the next failed health check. | `3`|
|`relative_name`| The relative DNS name provided by this Traffic Manager profile. | `rhsso`|
|`record_type`| The type of record set to create or delete. | `CNAME`|
|`entry`| Primary data value for all record types. | `rhsso-ansible-middleware.trafficmanager.net`|
|`dns_name`| Name of the public DNS. | `None`|
|`loadbalancer_ip`| Provide the loadbalancer ip to associate the CNAME. | `loadbalancer0-ip`|
|`resource_groups`| Dictionary to get the details of resource groups. | `None`|
|`vm`| Dictionary to get the details of VM created for the intance" | `None`|
|`db`| Dictionary to get the details of Database. | `None`|
|`db_sku_name`| The name of the SKU, typically, tier + family + cores, for example B_Gen4_1, GP_Gen5_8. | `B_Gen5_2`|
|`db_sku_tier`| The tier of the particular SKU. | `Basic`|
|`db_storage_mb`| The size code, to be interpreted by resource as appropriate. | `5120`|
|`db_enforce_ssl`| Enable SSL enforcement. | `true`|
|`db_version`| Maria DB server version. | `10.2`|
|`db_admin_username`| Maria DB username. | `keycloak-user`|
|`db_admin_password`| Maria DB password. | `redhat1!`|
|`managed_mariadb`| Choice of using managed MariaDB. | `true`|
|`db_vm`| List of VM's for unmanaged maria db. | `None`|



<!--end argument_specs-->

Dependencies
------------


Example Playbook
----------------

License
-------

GPL2

Author Information
------------------

* [Harsha Cherukuri](https://github.com/hcherukuri)
