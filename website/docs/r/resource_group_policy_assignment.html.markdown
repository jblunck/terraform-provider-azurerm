---
subcategory: "Policy"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_resource_group_policy_assignment"
description: |-
  Manages a Resource Group Policy Assignment.
---

# azurerm_resource_group_policy_assignment

Manages a Resource Group Policy Assignment.

## Example Usage

```hcl
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_policy_definition" "example" {
  name        = "only-deploy-in-westeurope"
  policy_type = "Custom"
  mode        = "All"

  policy_rule = <<POLICY_RULE
	{
    "if": {
      "not": {
        "field": "location",
        "equals": "westeurope"
      }
    },
    "then": {
      "effect": "Deny"
    }
  }
POLICY_RULE
}

resource "azurerm_resource_group_policy_assignment" "example" {
  name                 = "example"
  resource_group_id    = azurerm_resource_group.example.id
  policy_definition_id = azurerm_policy_definition.example.id

  parameters = <<PARAMS
    {
      "tagName": {
        "value": "Business Unit"
      },
      "tagValue": {
        "value": "BU"
      }
    }
PARAMS
}
```

## Arguments Reference

The following arguments are supported:

* `name` - (Required) The name which should be used for this Policy Assignment. Changing this forces a new Policy Assignment to be created.

* `policy_definition_id` - (Required) The ID of the Policy Definition or Policy Definition Set. Changing this forces a new Policy Assignment to be created.

* `resource_group_id` - (Required) The ID of the Resource Group where this Policy Assignment should be created. Changing this forces a new Policy Assignment to be created.

---

* `description` - (Optional) A description which should be used for this Policy Assignment.

* `display_name` - (Optional) The Display Name for this Policy Assignment.

* `enforce` - (Optional) Specifies if this Policy should be enforced or not?

* `identity` - (Optional) An `identity` block as defined below.

-> **Note:** The `location` field must also be specified when `identity` is specified.

* `location` - (Optional) The Azure Region where the Policy Assignment should exist. Changing this forces a new Policy Assignment to be created.

* `metadata` - (Optional) A JSON mapping of any Metadata for this Policy.

* `non_compliance_message` - (Optional) One or more `non_compliance_message` blocks as defined below.

* `not_scopes` - (Optional) Specifies a list of Resource Scopes (for example a Subscription, or a Resource Group) within this Management Group which are excluded from this Policy.

* `parameters` - (Optional) A JSON mapping of any Parameters for this Policy.

---

A `identity` block supports the following:

* `type` - (Optional) The Type of Managed Identity which should be added to this Policy Definition. The only possible value is `SystemAssigned`.

---

A `non_compliance_message` block supports the following:

* `content` - (Required) The non-compliance message text. When assigning policy sets (initiatives), unless `policy_definition_reference_id` is specified then this message will be the default for all policies.

* `policy_definition_reference_id` - (Optional) When assigning policy sets (initiatives), this is the ID of the policy definition that the non-compliance message applies to.

## Attributes Reference

In addition to the Arguments listed above - the following Attributes are exported:

* `id` - The ID of the Resource Group Policy Assignment.

---

The `identity` block exports the following:

* `principal_id` - The Principal ID of the Policy Assignment for this Resource Group.

* `tenant_id` - The Tenant ID of the Policy Assignment for this Resource Group.

## Timeouts

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:

* `create` - (Defaults to 30 minutes) Used when creating the Policy Assignment for this Resource Group.
* `read` - (Defaults to 5 minutes) Used when retrieving the Policy Assignment for this Resource Group.
* `update` - (Defaults to 30 minutes) Used when updating the Policy Assignment for this Resource Group.
* `delete` - (Defaults to 30 minutes) Used when deleting the Policy Assignment for this Resource Group.

## Import

Resource Group Policy Assignments can be imported using the `resource id`, e.g.

```shell
terraform import azurerm_resource_group_policy_assignment.example /subscriptions/00000000-0000-0000-000000000000/resourceGroups/group1/providers/Microsoft.Authorization/policyAssignments/assignment1
```
