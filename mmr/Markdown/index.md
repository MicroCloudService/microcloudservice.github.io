# Manage My Resources (MMR)

Start / stop tagged azure resources on a schedule, or on demmand. Integrated into Azure Portal, and runs in your subscription.

Supported resources:

| Resource                  | Stop Action | Start Acton |
| ------------------------- | ----------- | ----------- |
| Bastion                   | delete      | create      |
| Virtual Machine           | deallocate  | power on    |
| Virtual Machine Scale Set | deallocate  | power on    |
| Azure Kubernetes Service  | stop        | start       |

We are continuing to work on this list, if you have something we don't support please get in touch.

## Coming Soon

1. SQL Managed Instance
2. Cosmos DB

## More Information

* [User Guide](./UserGuide.html)
* [Installation](./Installation.html)
