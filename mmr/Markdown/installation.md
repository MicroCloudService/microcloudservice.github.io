# Installation

Manage My Resources (MMR) consists of a managed application, and a managed resorce group 'mrg-managmyresources-{unique number}' containing the scheduling components. These components include a User Assigned Identity named 'ManageMy-Resources' that must be granted permissions to find, start & stop resources.

To integrate the user interface into Azure Portal MMR uses a custom resource provider. The locations a custom resource provider can be deployed to are limited.

Steps:

1. Install the application from the marketplace
2. Grant the managed application permission to manage your scheduled resources
3. Add a Schedule (see the [User Guide](./UserGuide.md))
4. Add tags to the resources that belong to the Schedule
5. Use the All Resources view in MMR to verify the Schedule has discovered the expected resources
6. Trigger the Schedule and verify it's working as expected

## Permissions

In order for the application to have the permission in your subscription to find, start & stop resources, the user assigned identity 'ManageMy-Resources' must be assinged a subscription level role. We recommend using a custom role with a minimal set of permissions.

1. Optional: create custom role
2. Select the managed resource group (this can be found in the overview section of the managed application).
3. Select the user assigned identity named 'ManageMy-Resources'
4. Select 'Azure role assignments'
5. Add a role assignment at the subscription level granting either the Contributor role or 'Manage My Resources Operator' custom role
    * Contributor: easy win, suitable for testing the product, but grants the application far more rights than it requires
    * Manage My Resources Operator: prefered option, assigns the minimum set of rights required to discover, stop and start resources

### Custom Role

Open [Azure Portal](https://portal.azure.com/#view/Microsoft_Azure_Billing/SubscriptionsBlade) > Subscription > Access control (IAM) > Add > Add custom role

Enter:

__Custom role name:__ Manage My Resources Operator  
__Description:__ Lets the managed application \'Manage My Resources\' discover, start and stop tagged resources.  
__Baseline permissions:__ start from sratch  
Under __JSON__ tab select edit and add these permissions to the actions array:

```text
"Microsoft.Compute/virtualMachines/read",
"Microsoft.Compute/virtualMachines/instanceView/read",
"Microsoft.Compute/virtualMachines/deallocate/action",
"Microsoft.Compute/virtualMachines/start/action",
"Microsoft.Compute/virtualMachineScaleSets/read",
"Microsoft.Compute/virtualMachineScaleSets/deallocate/action",
"Microsoft.Compute/virtualMachineScaleSets/start/action",
"Microsoft.ContainerService/managedClusters/read",
"Microsoft.ContainerService/managedClusters/start/action",
"Microsoft.ContainerService/managedClusters/stop/action",
"Microsoft.Network/bastionHosts/read",
"Microsoft.Network/bastionHosts/write",
"Microsoft.Network/bastionHosts/delete",
"Microsoft.Network/virtualNetworks/subnets/join/action",
"Microsoft.Network/publicIPAddresses/join/action"
```

Documentation for these roles can be found in <https://learn.microsoft.com/en-us/azure/role-based-access-control/resource-provider-operations>.

In production you may only want to manage Bastions, in that senario you could just grant the bastion specifc permissions.

## Marketplace App Deployment Issues

Tagging (and other) policies can prevent a customer from deploying a marketplace app. Work arounds are:

1. Policies affecting managed application

    Add an exemption on the problem policies to the scoped to the resource group that the managed application will be deployed to. The excemption can be further refined to target the 'Microsoft.Solutions/applications' resource type.
    The resources created by the managed appliation don't seem to be affected by the policies when this exemption in enabled.

2. Policies affecting managed resource group

    Unfortunately we have no way of targetting these types of resource groups, a solution is to override the policy effect and set to 'Audit'.

3. Tagging Policies

    While our deployment supports tagging the resources deployed to the managed resource group, Marketplace applications do not support tagging the managed application itself. If creating and exemption as per option 1 is not possbile then deploying the managed application using bicep or terraform will allow the setting of tags on the managed application.

## Automation

The managed application and schedules can be deployed via code.

### Managed Application

```bicep
TODO bicep example here
```

### Schedule

```bicep
TODO bicep example here
```

#### Custom Role

```bicep
targetScope = 'subscription'

@description('Friendly name of the role definition')
param roleName string = 'Manage My Resources Operator'

@description('Detailed description of the role definition')
param roleDescription string = 'Lets the managed application \'Manage My Resources\' discover, start and stop tagged resources.'

var roleDefName = guid(subscription().id, roleName)

resource roleDef 'Microsoft.Authorization/roleDefinitions@2022-04-01' = {
  name: roleDefName
  properties: {
    roleName: roleName
    description: roleDescription
    type: 'customRole'
    permissions: [
      {
        actions: [
          'Microsoft.Compute/virtualMachines/read'
          'Microsoft.Compute/virtualMachines/instanceView/read'
          'Microsoft.Compute/virtualMachines/deallocate/action'
          'Microsoft.Compute/virtualMachines/start/action'

          'Microsoft.Compute/virtualMachineScaleSets/read'
          'Microsoft.Compute/virtualMachineScaleSets/deallocate/action'
          'Microsoft.Compute/virtualMachineScaleSets/start/action'

          // Kubernetes
          'Microsoft.ContainerService/managedClusters/read'
          'Microsoft.ContainerService/managedClusters/start/action'
          'Microsoft.ContainerService/managedClusters/stop/action'

          // Bastion
          'Microsoft.Network/bastionHosts/read'
          'Microsoft.Network/bastionHosts/write'
          'Microsoft.Network/bastionHosts/delete'
          'Microsoft.Network/virtualNetworks/subnets/join/action'
          'Microsoft.Network/publicIPAddresses/join/action'
        ]        
      }
    ]
    assignableScopes: [
      subscription().id
    ]
  }
}
```
