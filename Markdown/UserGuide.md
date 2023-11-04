# Manage My Resources (MMR)

MMR provides the following views:

Schedules: manage schedules and view the current state of a schedule.
Bastion: manage scheduled Bastions, start (create) and stop (delete) a Bastion on demand.
All Resources: list resources and the schedule they belong to.

## Schedules

Schedules are managed in the 'Schedules' tab located on the panel on the left.

Azure resources are managed according to how they are tagged. This is defined by the following properties.

### Schedule Properties

#### Tag Name

Defaults to 'AutoSchedule'. Azure resources included in a schedule are discovered by a combination of the presence of this tag and it's value.

#### Tag Value

Default to 'Yes'. Use this value to filter the resources into those that belong to a schedule. For example, you may wish to create multiple schedules for different envrionments using values such as 'Dev', 'Test', and 'Acceptance' to identify the resources to be scheduled.

#### Priority Tag

Defines the order in which the resources will be started. For example you may wish to start a domain controller, then database, then all remaining resources. In the use case your domain controller would get a tag of '100', your database '200', and all remaining resources '300'.

Tag name defaults to 'AutoSchedulePriority'. If the tag or it's value is not present on a resouce, the priority will default to '400'.

Current limitations are that there can only be a maximum of 4 groups. Recommented values are '100', '200', '300', and '400'.

There is a 2 minute delay between each group.

The stop schedule reverses the order of the priority.

#### Enabled

Schedule is enabled or disabled. A disabled schedule can still be triggered manually.

### Schedule Execution

Schedules can be triggered on demand using the start and stop buttons on the Schedule tab.

When manually triggering a schedule the schedule list will not refresh automatically. You'll need to use the refresh button to view the schedules status.

If a schedule fails, for example the user assigned identity doesn't have the necessary rights, the error message for most actions can be viewed in the 'Last Error' column on the Schedules tab. This column is hidden by default so you may need to select the 'Edit columns' button to add the colum, or select the 'Raw details' from the elipse button to the right of the row.

## Bastions

As Bastions are "special" in that they cannot be stopped we must delete them as part of a stop schedule and build them for the start schedule.

This view lists all Bastions, whether they are managed or not (i.e. are part of a schedule), and the details stored about the Bastion to recreate it.

If a managed Bastion is deleted then the record of that Bastion will need to be deleted from this view as well. If not the Bastion will be recreated when it's assocated schedule is next run.

The information stored for a Bastion is updated everytime a Bastion is stopped (deleted) by the Schedule. This means that you can adjust the tier, or any other setting, while the Bastion is running and it will be recreated with those same settings the next time the stop and start Schedules are run.

## All Resources

Use this view to see the current state of the resources (started / stopped) and the schedule a resource belongs to.

List only shows resources that are capabible of being scheduled, i.e. it will not show resources that MMR isn't capable of starting and stopping, or doesn't have at least read permission over.

Due to limitations in the Azure API used to populate this list the state of the resource is not always up to date.
