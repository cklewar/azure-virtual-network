# AZURE-MARKETPLACE-AGREEMENT

This repository consists of Terraform templates to create Azure Marketplace agreement.

## Usage

- Clone this repo with: `git clone --recurse-submodules https://github.com/cklewar/azure-marketplace-agreement`
- Enter repository directory with: `cd azure-marketplace-agreement`
- Obtain F5XC API certificate file from Console and save it to `cert` directory
- Pick and choose from below examples and add mandatory input data and copy data into file `main.tf.example`.
- Rename file __main.tf.example__ to __main.tf__ with: `rename main.tf.example main.tf`
- Initialize with: `terraform init`
- Apply with: `terraform apply -auto-approve` or destroy with: `terraform destroy -auto-approve`

### Example Output

```bash
test123
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # module.f5xc_azure_marketplace_agreement_app_stack.azurerm_marketplace_agreement.f5xc_azure_ce[0] will be created
  + resource "azurerm_marketplace_agreement" "f5xc_azure_ce" {
      + id                  = (known after apply)
      + license_text_link   = (known after apply)
      + offer               = "entcloud_voltmesh_voltstack_node"
      + plan                = "freeplan_entcloud_voltmesh_voltstack_node"
      + privacy_policy_link = (known after apply)
      + publisher           = "volterraedgeservices"
    }

  # module.f5xc_azure_marketplace_agreement_multi_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0] will be created
  + resource "azurerm_marketplace_agreement" "f5xc_azure_ce" {
      + id                  = (known after apply)
      + license_text_link   = (known after apply)
      + offer               = "entcloud_voltmesh_voltstack_node"
      + plan                = "freeplan_entcloud_voltmesh_voltstack_node_multinic"
      + privacy_policy_link = (known after apply)
      + publisher           = "volterraedgeservices"
    }

  # module.f5xc_azure_marketplace_agreement_single_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0] will be created
  + resource "azurerm_marketplace_agreement" "f5xc_azure_ce" {
      + id                  = (known after apply)
      + license_text_link   = (known after apply)
      + offer               = "volterra-node"
      + plan                = "volterra-node"
      + privacy_policy_link = (known after apply)
      + publisher           = "volterraedgeservices"
    }

Plan: 3 to add, 0 to change, 0 to destroy.
module.f5xc_azure_marketplace_agreement_app_stack.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Creating...
module.f5xc_azure_marketplace_agreement_multi_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Creating...
module.f5xc_azure_marketplace_agreement_single_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Creating...
module.f5xc_azure_marketplace_agreement_app_stack.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Still creating... [10s elapsed]
module.f5xc_azure_marketplace_agreement_multi_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Still creating... [10s elapsed]
module.f5xc_azure_marketplace_agreement_single_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Still creating... [10s elapsed]
module.f5xc_azure_marketplace_agreement_multi_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Creation complete after 12s [id=/subscriptions/e9cbbd48-704d-4dfa-bf62-60edda755a66/providers/Microsoft.MarketplaceOrdering/agreements/volterraedgeservices/offers/entcloud_voltmesh_voltstack_node/plans/freeplan_entcloud_voltmesh_voltstack_node_multinic]
module.f5xc_azure_marketplace_agreement_app_stack.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Creation complete after 13s [id=/subscriptions/e9cbbd48-704d-4dfa-bf62-60edda755a66/providers/Microsoft.MarketplaceOrdering/agreements/volterraedgeservices/offers/entcloud_voltmesh_voltstack_node/plans/freeplan_entcloud_voltmesh_voltstack_node]
module.f5xc_azure_marketplace_agreement_single_nic.azurerm_marketplace_agreement.f5xc_azure_ce[0]: Creation complete after 17s [id=/subscriptions/e9cbbd48-704d-4dfa-bf62-60edda755a66/providers/Microsoft.MarketplaceOrdering/agreements/volterraedgeservices/offers/volterra-node/plans/volterra-node]
Releasing state lock. This may take a few moments...

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```

## Create Azure Marketplace Agreement

First time only state import needed. Make sure to adapt module names:

```bash
terraform import module.f5xc_azure_marketplace_agreement_single_nic.azurerm_marketplace_agreement.f5xc_azure_ce_singel_nic /subscriptions/e9cbbd48-704d-4dfa-bf62-60edda755a66/providers/Microsoft.MarketplaceOrdering/agreements/volterraedgeservices/offers/volterra-node/plans/volterra-node
terraform import module.f5xc_azure_marketplace_agreement_multi_nic.azurerm_marketplace_agreement.f5xc_azure_ce /subscriptions/e9cbbd48-704d-4dfa-bf62-60edda755a66/providers/Microsoft.MarketplaceOrdering/agreements/volterraedgeservices/offers/entcloud_voltmesh_voltstack_node/plans/freeplan_entcloud_voltmesh_voltstack_node_multinic
terraform import module.f5xc_azure_marketplace_agreement_app_stack.azurerm_marketplace_agreement.f5xc_azure_ce /subscriptions/e9cbbd48-704d-4dfa-bf62-60edda755a66/providers/Microsoft.MarketplaceOrdering/agreements/volterraedgeservices/offers/entcloud_voltmesh_voltstack_node/plans/freeplan_entcloud_voltmesh_voltstack_node
```

```hcl
variable "azure_client_id" {
  type    = string
}

variable "azure_client_secret" {
  type    = string
}

variable "azure_tenant_id" {
  type    = string
}

variable "azure_subscription_id" {
  type    = string
}

module "f5xc_azure_marketplace_agreement_single_nic" {
  source                           = "./modules/azure/agreement"
  azure_client_id                  = var.azure_client_id
  azure_client_secret              = var.azure_client_secret
  azure_tenant_id                  = var.azure_tenant_id
  azure_subscription_id            = var.azure_subscription_id
  f5xc_azure_ce_gw_type            = "single_nic"
}

module "f5xc_azure_marketplace_agreement_multi_nic" {
  source                           = "./modules/azure/agreement"
  azure_client_id                  = var.azure_client_id
  azure_client_secret              = var.azure_client_secret
  azure_tenant_id                  = var.azure_tenant_id
  azure_subscription_id            = var.azure_subscription_id
  f5xc_azure_ce_gw_type            = "multi_nic"
}

module "f5xc_azure_marketplace_agreement_app_stack" {
  source                           = "./modules/azure/agreement"
  azure_client_id                  = var.azure_client_id
  azure_client_secret              = var.azure_client_secret
  azure_tenant_id                  = var.azure_tenant_id
  azure_subscription_id            = var.azure_subscription_id
  f5xc_azure_ce_gw_type            = "app_stack"
}
```