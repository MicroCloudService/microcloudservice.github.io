# Create a custom role and add 'Azure role assignments' to have the permission in your subscription to find, start & stop resources

You can deploy the Custom role using the link.
[![Deploy To Azure](https://raw.githubusercontent.com/MicroCloudService/microcloudservice.github.io/main/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicroCloudService%2Fmicrocloudservice.github.io%2Fmain%2FManage_My_Resources_Operator.json)

In order for the application to have the permission in your subscription to find, start & stop resources, the user assigned identity 'ManageMy-Resources' must be assigned a subscription level role. 

We recommend using a custom role with a minimal set of permissions.

# Create custom role
Select the managed resource group (this can be found in the overview section of the managed application).

Select the user assigned identity named 'ManageMy-Resources'

Select 'Azure role assignments'

Add a role assignment at the subscription level granting either the Contributor role or 'Manage My Resources Operator' custom role

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
# OR :

# Add as Contributor: 

Suitable for testing the product, but grants the application far more rights than it requires.

Manage My Resources Operator: prefered option, assigns the minimum set of rights required to discover, stop and start resources

Documentation for these roles can be found in https://learn.microsoft.com/en-us/azure/role-based-access-control/resource-provider-operations.

# Next Steps 

Either assign the Contributor or the Custom Role created called 'Manage My Resources Operator' to the Managed Identity named 'ManageMy-Resources'.

You can assign the Custom roleto the Managed Identity using the link.
[![Deploy To Azure](https://raw.githubusercontent.com/MicroCloudService/microcloudservice.github.io/main/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicroCloudService%2Fmicrocloudservice.github.io%2Fmain%2FManage_My_Resources_Permissions.json)

Select the managed resource group (this can be found in the overview section of the managed application).

Select the user assigned identity named 'ManageMy-Resources'

Select 'Azure role assignments'

Add a role assignment at the subscription level granting either the Contributor role or 'Manage My Resources Operator' custom role


