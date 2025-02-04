I have retrieved the necessary information to create a `README.md` file for the `MaciekCiurla/CopilotTests` repository. Here's a draft:

```markdown
# Terraform Modules for Palo Alto Networks Software Firewalls on Azure Cloud

![GitHub release (latest by date)](https://img.shields.io/github/v/release/PaloAltoNetworks/terraform-azurerm-swfw-modules?style=flat-square)
![GitHub](https://img.shields.io/github/license/PaloAltoNetworks/terraform-modules-swfw-ci-workflows?style=flat-square)
![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/PaloAltoNetworks/terraform-azurerm-swfw-modules/release_ci.yml?style=flat-square)
![GitHub issues](https://img.shields.io/github/issues/PaloAltoNetworks/terraform-azurerm-swfw-modules?style=flat-square)
![GitHub pull requests](https://img.shields.io/github/issues-pr/PaloAltoNetworks/terraform-azurerm-swfw-modules?style=flat-square)
![Terraform registry downloads total](https://img.shields.io/badge/dynamic/json?color=green&label=downloads%20total&query=data.attributes.total&url=https%3A%2F%2Fregistry.terraform.io%2Fv2%2Fmodule[...])
![Terraform registry download month](https://img.shields.io/badge/dynamic/json?color=green&label=downloads%20this%20month&query=data.attributes.month&url=https%3A%2F%2Fregistry.terraform.io%2Fv2%2F[...])

## Overview

A set of modules for using **Palo Alto Networks Software Firewalls** to provide control and protection to your applications running on Azure Cloud. It deploys Software Firewalls and configures aspects such as virtual networks, subnets, network security groups, storage accounts, service principals, Panorama virtual machine instances, and more.

The design is heavily based on the [Reference Architecture Guide for Azure](https://pandocs.tech/fw/115p-prime).

For copyright and license see the LICENSE file.

## Structure

This repository has the following directory structure:

* `modules` - Contains several standalone, reusable, production-grade Terraform modules, each individually documented.
* `examples` - Shows examples of different ways to combine the modules contained in the `modules` directory. Note that **this code should NOT be used directly in production** as it might contain examples of sensitive data that normally should not be kept in a repository.

## Security

Modules hosted in this repository require sensitive data to work, such as passwords and firewall bootstrap options. We do not provide a mechanism to safely store this information.

Examples provided here are a form of documentation and are meant to help you understand how to use the modules. They were not written with security in mind. Before using this code for purposes other than training, follow all Terraform and your organization's security best practices.

## Compatibility

The compatibility with Terraform is defined individually per each module. In general, expect the earliest compatible Terraform version to be 1.0.0 across most of the modules.

## Versioning

These modules follow the principles of [Semantic Versioning](http://semver.org/). You can find each new release, along with the changelog, on the GitHub [Releases](https://github.com/PaloAltoNetworks/terraform-azurerm-swfw-modules/releases) page.

## Getting Help

If you have found a bug, please report it by creating a new issue on the [GitHub issue page](https://github.com/PaloAltoNetworks/terraform-azurerm-swfw-modules/issues).

For consulting support, please contact services-sales@paloaltonetworks.com or your Palo Alto Networks account manager.

## Contributing

Contributions are welcome and greatly appreciated! Every little bit helps, and credit will always be given. Please follow our [contributing guide](https://github.com/PaloAltoNetworks/terraform-best-practices/blob/main/CONTRIBUTING.md).

## Prerequisites

- _(In case of non-cloud shell deployment)_ Credentials and (optionally) tools required to authenticate against Azure Cloud. See [AzureRM provider documentation for details](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs#authenticating-to-azure).
- [Supported](#requirements) version of [Terraform](https://developer.hashicorp.com/terraform/downloads).
- Acceptance of Palo Alto NGFW images license in a subscription if not done already ([see this note](../../modules/vmseries/README.md#accept-azure-marketplace-terms)).

## Usage

### Deployment Steps

1. Checkout the code locally (if you haven't done so yet).
2. Copy the [`example.tfvars`](./example.tfvars) file, rename it to `terraform.tfvars`, and adjust it to your needs (take a closer look at the `TODO` markers).
3. _(Optional)_ Authenticate to AzureRM, switch to the Subscription of your choice.
4. Provide `subscription_id` either by creating an environment variable named `ARM_SUBSCRIPTION_ID` with Subscription ID as the value in your shell (recommended option) or by setting the value of `subscription_id` variable within your `tfvars` file (discouraged option, we don't recommend putting the Subscription ID in clear text inside the code).
5. Initialize the Terraform module:
   ```bash
   terraform init
   ```
6. _(Optional)_ Plan your infrastructure to see what will be actually deployed:
   ```bash
   terraform plan
   ```
7. Deploy the infrastructure (you will have to confirm it by typing in `yes`):
   ```bash
   terraform apply
   ```
   The deployment takes a couple of minutes. At the end, you should see a summary similar to:
   ```console
   Apply complete! Resources: 53 added, 0 changed, 0 destroyed.
   ```

### Post-deployment

- Retrieve the initial credentials for the firewalls:
  - For username:
    ```bash
    terraform output usernames
    ```
  - For password:
    ```bash
    terraform output passwords
    ```
- The management public IP addresses are available in the `vmseries_mgmt_ips`:
  ```bash
  terraform output vmseries_mgmt_ips
  ```
- Login to the devices using either CLI (SSH client required) or Web UI (HTTPS).

## Cleanup

To remove the deployed infrastructure, run:
```sh
terraform destroy
```

## Reference

### Requirements

- `terraform`, version: >= 1.5, < 2.0

### Providers

- `random`
- `azurerm`
- `local`

### Modules

Name | Version | Source | Description
--- | --- | --- | ---
`vnet` | - | ../../modules/vnet | 
`vnet_peering` | - | ../../modules/vnet_peering | 
`public_ip` | - | ../../modules/public_ip | 
`natgw` | - | ../../modules/natgw | 
`load_balancer` | - | ../../modules/loadbalancer | 
`appgw` | - | ../../modules/appgw | 
`ngfw_metrics` | - | ../../modules/ngfw_metrics | 
`bootstrap` | - | ../../modules/bootstrap | 
`vmseries` | - | ../../modules/vmseries | 
`test_infrastructure` | - | ../../modules/test_infrastructure | 

### Resources

- `availability_set` (managed)
- `resource_group` (managed)
- `file` (managed)
- `password` (managed)
- `resource_group` (data)

### Required Inputs

Name | Type | Description
--- | --- | ---
`subscription_id` | `string` | Azure Subscription ID is a required argument since AzureRM provider v4.
`resource_group_name` | `string` | Name of the Resource Group.
`region` | `string` | The Azure region to use.
`vnets` | `map` | A map defining VNETs.

### Optional Inputs

Name | Type | Description
--- | --- | ---
`name_prefix` | `string` | A prefix that will be added to all created resources.
`create_resource_group` | `bool` | When set to `true` it will cause a Resource Group creation.
`tags` | `map` | Map of tags to assign to the created resources.
`vnet_peerings` | `map` | A map defining VNET peerings.
`public_ips` | `object` | A map defining Public IP Addresses and Prefixes.
`natgws` | `map` | A map defining NAT Gateways.
`load_balancers` | `map` | A map containing configuration for all (both private and public) Load Balancers.
`appgws` | `map` | A map defining all Application Gateways in the current deployment.
`ngfw_metrics` | `object` | A map controlling metrics-related resources.
`bootstrap_storages` | `map` | A map defining Azure Storage Accounts used to host file shares for bootstrapping NGFWs.
`vmseries_universal` | `object` | A map defining common settings for all created VM-Series instances.
`vmseries` | `map` | A map defining Azure Virtual Machines based on Palo Alto Networks Next Generation Firewall image.
`test_infrastructure` | `map` | A map defining test infrastructure including test VMs and Azure Bastion hosts.

### Outputs

Name | Description
--- | ---
`usernames` | Initial administrative username to use for VM-Series.
`passwords` | Initial administrative password to use for VM-Series.
`natgw_public_ips` | Nat Gateways Public IP resources.
`metrics_instrumentation_keys` | The Instrumentation Key of the created instance(s) of Azure Application Insights.
`lb_frontend_ips` | IP Addresses of the load balancers.
`vmseries_mgmt_ips` | IP addresses for the VM-Series management interface.
`bootstrap_storage_urls` | 
`test_vms_usernames` | Initial administrative username to use for test VMs.
`test_vms_passwords` | Initial administrative password to use for test VMs.
`test_vms_ips` | IP Addresses of the test VMs.
`test_lb_frontend_ips` | IP Addresses of the test load balancers.

### Required Inputs details

#### subscription_id

Azure Subscription ID is a required argument since AzureRM provider v4.

**Note!** \
Instead of putting the Subscription ID directly in the code, it's recommended to use an environment variable. Create an environment variable named `ARM_SUBSCRIPTION_ID` with your Subscription ID as the value and leave this variable set to `null`.

Type: string

#### resource_group_name

Name of the Resource Group.

Type: string

#### region

The Azure region to use.

Type: string

#### vnets

A map defining VNETs.

For detailed documentation on each property refer to [module documentation](../../modules/vnet/README.md)

- `create_virtual_network`  - (`bool`, optional, defaults to `true`) When set to `true` will create a VNET, `false` will source an existing VNET.
- `name`                    - (`string`, required) A name of a VNET. In case `create_virtual_network = false` this should be a full resource name, including prefixes.
- `resource_group_name`     - (`string`, optional, defaults to current RG) A name of an existing Resource Group in which the VNET will reside or is sourced from.
- `address_space`           - (`list`, required when `create_virtual_network = false`) A list of CIDRs for a newly created VNET.
- `dns_servers`             - (`list`, optional, defaults to module defaults) A list of IP addresses of custom DNS servers (by default Azure DNS is used).
- `vnet_encryption`         - (`string`, optional, defaults to module default) Enables Azure Virtual Network Encryption when set, only possible value at the moment is `AllowUnencrypted`. When set to `null`, the feature is disabled.
- `ddos_protection_plan_id` - (`string`, optional, defaults to `null`) ID of an existing Azure Network DDOS Protection Plan to be associated with the VNET.
- `network_security_groups` - (`map`, optional) Map of Network Security Groups to create, for details see [VNET module documentation](../../modules/vnet/README.md#network_security_groups).
- `route_tables`            - (`map`, optional) Map of Route Tables to create, for details see [VNET module documentation](../../modules/vnet/README.md#route_tables).
- `subnets`                 - (`map`, optional) Map of Subnets to create or source, for details see [VNET module documentation](../../modules/vnet/README.md#subnets).

Type: 

```hcl
map(object({
    create_virtual_network  = optional(bool, true)
    name                    = string
    resource_group_name     = optional(string)
    address_space           = optional(list(string))
    dns_servers             = optional(list(string))
    vnet_encryption         = optional(string)
    ddos_protection_plan_id = optional(string)
    network_security_groups = optional(map(object({
      name = string
      rules = optional(map(object({
        name                         = string
        priority                     = number
        direction                    = string
        access                       = string
        protocol                     = string
        source_port_range            = optional(string)
        source_port_ranges           = optional(list(string))
        destination_port range       = optional(string)
        destination_port_ranges      = optional(list(string))
        source_address_prefix        = optional(string)
        source_address_prefixes      = optional(list(string))
        destination_address_prefix   = optional(string)
        destination_address_prefixes = optional(list(string))
      })), {})
    })), {})
    route_tables = optional(map(object({
      name                          = string
      bgp_route_propagation_enabled = optional(bool)
      routes = map(object({
        name                = string
        address_prefix      = string
        next_hop_type       = string
        next_hop_ip_address = optional(string)
      }))
    })), {})
    subnets = optional(map(object({
      create                          = optional(bool, true)
      name                            = string
      address_prefixes                = optional(list(string), [])
      network_security_group_key      = optional(string)
      route_table_key                 = optional(string)
      enable_storage_service_endpoint = optional(bool)
      enable_cloudngfw_delegation     = optional(bool)
    })), {})
  }))
```

### Optional Inputs details

#### name_prefix

A prefix that will be added to all created resources. There is no default delimiter applied between the prefix and the resource name. Please include the delimiter in the actual prefix.

Example:
```hcl
name_prefix = "test-"
```

**Note!** \
This prefix is not applied to existing resources. If you plan to reuse, for example, a VNET, please specify its full name, even if it is also prefixed with the same value as the one in this property.

Type: string

Default value: ``

#### create_resource_group

When set to `true` it will cause a Resource Group creation. Name of the newly specified RG is controlled by `resource_group_name`.

When set to `false` the `resource_group_name` parameter is used to specify a name of an existing Resource Group.

Type: bool

Default value: `true`

#### tags

Map of tags to assign to the created resources.

Type: map(string)

Default value: `map[]`

#### vnet_peerings

A map defining VNET peerings.

Following properties are supported:
- `local_vnet_name`            - (`string`, required) Name of the local VNET.
- `local_resource_group_name`  - (`string`, optional) Name of the resource group, in which local VNET exists.
- `remote_vnet_name`           - (`string`, required) Name of the remote VNET.
- `remote_resource_group_name` - (`string`, optional) Name of the resource group, in which remote VNET exists.

Type: 

```hcl
map(object({
    local_vnet_name            = string
    local_resource_group_name  = optional(string)
    remote_vnet_name           = string
    remote_resource_group_name = optional(string)
  }))
```

Default value: `map[]`

#### public_ips

A map defining Public IP Addresses and Prefixes.

Following properties are available:

- `public_ip_addresses` - (`map`, optional) Map of objects describing Public IP Addresses, please refer to [module documentation](../../modules/public_ip/README.md#public_ip_addresses) for available properties.
- `public_ip_prefixes`  - (`map`, optional) Map of objects describing Public IP Prefixes, please refer to [module documentation](../../modules/public_ip/README.md#public_ip_prefixes) for available properties.

Type: 

```hcl
object({
    public_ip_addresses = optional(map(object({
      create                     = bool
      name                       = string
      resource_group_name        = optional(string)
      zones                      = optional(list(string))
      domain_name_label          = optional(string)
      idle_timeout_in_minutes    = optional(number)
      prefix_name                = optional(string)
      prefix_resource_group_name = optional(string)
    })), {})
    public_ip_prefixes = optional(map(object({
      create              = bool
      name                = string
      resource_group_name = optional(string)
      zones               = optional(list(string))
      length              = optional(number)
    })), {})
  })
```

Default value: `map[]`

#### natgws

A map defining NAT Gateways. 

Please note that a NAT Gateway is a zonal resource, which means it's always placed in a zone (even when you do not specify one explicitly). Please refer to Microsoft documentation for notes on NAT Gateway's zonal resiliency. For detailed documentation on each property, refer to [module documentation](../../modules/natgw/README.md).

Following properties are supported:
- `name`                - (`string`, required) A name of a NAT Gateway. In case `create_natgw = false` this should be a full resource name, including prefixes.
- `vnet_key`            - (`string`, required) A name (key value) of a VNET defined in `var.vnets` that hosts a subnet this NAT Gateway will be assigned to.
- `subnet_keys`         - (`list(string)`, required) A list of subnets (key values) the NAT Gateway will be assigned to, defined in `var.vnets` for a VNET described by `vnet_name`.
- `create_natgw`        - (`bool`, optional, defaults to `true`) Create (`true`) or source an existing NAT Gateway (`false`), created or sourced: the NAT Gateway will be assigned to a subnet created by the `vnet` module.
- `resource_group_name` - (`string`, optional) Name of a Resource Group hosting the NAT Gateway (newly created or the existing one).
- `zone`                - (`string`, optional) An Availability Zone in which the NAT Gateway will be placed. When skipped, Azure will pick a zone.
- `idle_timeout`        - (`number`, optional, defaults to 4) Connection IDLE timeout in minutes, for newly created resources.
- `public_ip`           - (`object`, optional) An object defining a public IP resource attached to the NAT Gateway.
-
