# Scale-Azure-Virtual-Machine-Scale-Sets
Autoscale is a built-in feature that helps applications perform their best when demand changes. 
You can choose to scale your resource manually to a specific instance count, or via a custom Autoscale policy that scales based on metric(s) thresholds, or schedule instance count which scales during designated time windows. Autoscale enables your resource to be performant and cost effective by adding and removing instances based on demand.

In this task, you scale the virtual machine scale set using a custom scale rule.
Select Go to resource or search for and select the vmss1 scale set.
Choose Availability + Scale from the left side menu, then choose Scaling.
Did you know? You can Manual scale or Custom autoscale. In scale sets with a small number of VM instances, increasing or decreasing the instance count (Manual scale) may be best. In scale sets with a large number of VM instances, scaling based on metrics (Custom autoscale) may be more appropriate.
Scale out rule
Select Custom autoscale. Then change the Scale mode to Scale based on metric. And then select Add a rule.
Let’s create a rule that automatically increases the number of VM instances. This rule scales out when the average CPU load is greater than 70% over a 10-minute period. When the rule triggers, the number of VM instances is increased by 20%.
Setting
Value
Metric source
Current resource (vmss1)
Metric namespace
Virtual Machine Host
Metric name
Percentage CPU (review your other choices)
Operator
Greater than
Metric threshold to trigger scale action
70
Duration (minutes)
10
Time grain statistic
Average
Operation
Increase percent by (review other choices)
Cool down (minutes)
5
Percentage
50



Be sure to Save your changes.
Scale in rule
During evenings or weekends, demand may decrease so it is important to create a scale in rule.
Let’s create a rule that decreases the number of VM instances in a scale set. The number of instances should decrease when the average CPU load drops below 30% over a 10-minute period. When the rule triggers, the number of VM instances is decreased by 20%.
Select Add a rule, adjust the settings, then select Add.
Setting
Value
Operator
Less than
Threshold
30
Operation
decrease percentage by (review your other choices)
Percentage
50

Be sure to Save your changes.
Set the instance limits
When your autoscale rules are applied, instance limits make sure that you do not scale out beyond the maximum number of instances or scale in beyond the minimum number of instances.
Instance limits are shown on the Scaling page after the rules.
Setting
Value
Minimum
2
Maximum
10
Default
2

Be sure to Save your changes
On the vmss1 page, select Instances. This is where you would monitor the number of virtual machine instances.
Note: If you are interested in using Azure PowerShell for virtual machine creation, try Task 5. If you are interested in using the CLI to create virtual machines, try Task 6.
Task 5: Create a virtual machine using Azure PowerShell (option 1)
Use the icon (top right) to launch a Cloud Shell session. Alternately, navigate directly to https://shell.azure.com.
Be sure to select PowerShell. If necessary, configure the shell storage.
Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the New-AzVM command reference for all the parameters associated with creating a virtual machine.
codeCopy
New-AzVm `
 -ResourceGroupName 'az104-rg8' `
 -Name 'myPSVM' `
 -Location 'East US' `
 -Image 'Win2019Datacenter' `
 -Zone '1' `
 -Size 'Standard_D2s_v3' `
 -Credential (Get-Credential)
Once the command completes, use Get-AzVM to list the virtual machines in your resource group.
codeCopy
Get-AzVM `
 -ResourceGroupName 'az104-rg8' `
 -Status
Verify your new virtual machine is listed and the Status is Running.
Use Stop-AzVM to deallocate your virtual machine. Type Yes to confirm.
codeCopy
Stop-AzVM `
 -ResourceGroupName 'az104-rg8' `
 -Name 'myPSVM' 
Use Get-AzVM with the -Status parameter to verify the machine is deallocated.
Did you know? When you use Azure to stop your virtual machine, the status is deallocated. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.
Task 6: Create a virtual machine using the CLI (option 2)
Use the icon (top right) to launch a Cloud Shell session. Alternately, navigate directly to https://shell.azure.com.
Be sure to select Bash. If necessary, configure the shell storage.
Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the az vm create command reference for all the parameters associated with creating a virtual machine.
shellCopy
az vm create --name myCLIVM --resource-group az104-rg8 --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys
Once the command completes, use az vm show to verify your machine was created.
shellCopy
az vm show --name  myCLIVM --resource-group az104-rg8 --show-details
Verify the powerState is VM Running.
Use az vm deallocate to deallocate your virtual machine. Type Yes to confirm.
shellCopy
az vm deallocate --resource-group az104-rg8 --name myCLIVM
Use az vm show to ensure the powerState is VM deallocated.
Did you know? When you use Azure to stop your virtual machine, the status is deallocated. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.
Cleanup your resources
If you are working with your own subscription take a minute to delete the lab resources. This will ensure resources are freed up and cost is minimized. The easiest way to delete the lab resources is to delete the lab resource group.
In the Azure portal, select the resource group, select Delete the resource group, Enter resource group name, and then click Delete.
Using Azure PowerShell, Remove-AzResourceGroup -Name resourceGroupName.
Using the CLI, az group delete --name resourceGroupName.
Extend your learning with Copilot
Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to copilot.microsoft.com. Take a few minutes to try these prompts.
Provide the steps and the Azure CLI commands to create a Linux virtual machine.
Review the ways you can scale virtual machines and improve performance.
Describe Azure storage lifecycle management policies and how they can optimize costs.
Learn more with self-paced training
Create a Windows virtual machine in Azure. Create a Windows virtual machine using the Azure portal. Connect to a running Windows virtual machine using Remote Desktop
Build a scalable application with Virtual Machine Scale Sets. Enable your application to automatically adjust to changes in load while minimizing costs with Virtual Machine Scale Sets.
Connect to virtual machines through the Azure portal by using Azure Bastion. Deploy Azure Bastion to securely connect to Azure virtual machines directly within the Azure portal to effectively replace an existing jumpbox solution, monitor remote sessions by using diagnostic logs, and manage remote sessions by disconnecting a user session.
Key takeaways
Congratulations on completing the lab. Here are the main takeaways for this lab.
Azure virtual machines are on-demand, scalable computing resources.
Azure virtual machines provide both vertical and horizontal scaling options.
Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings.
Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration.
In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.

