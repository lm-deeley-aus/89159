# StaticKube Instance Reboot

This doc is to answer the question, what is the right way to reboot a StaticKube instance if it is on the AWS DEV list to be rebooted?

##Steps
  
  *For Rancher and Mission Control Nodes (no outage necessary)*<br>
   1 A.) Safely Drain a Node and cordon the node<br>
      -  NOTE: If the DEVAWS list contains multiple nodes, it is highly advised that this step gets performed one node at a time.<br> 
      - **ALWAYS** ensure that there are at least two nodes, you risk causing an outage when there are less then two nodes working.<br> 
      - Follow the steps below or navigate to here: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/a#before-you-begin
<br><br> Use kubectl drain to remove a node from service
You can use kubectl drain to safely evict all of your pods from a node before you perform maintenance on the node (e.g. kernel upgrade, hardware maintenance, etc.). Safe evictions allow the pod's containers to gracefully terminate and will respect the PodDisruptionBudgets you have specified.

Note: By default kubectl drain ignores certain system pods on the node that cannot be killed; see the kubectl drain documentation for more details.
When kubectl drain returns successfully, that indicates that all of the pods (except the ones excluded as described in the previous paragraph) have been safely evicted (respecting the desired graceful termination period, and respecting the PodDisruptionBudget you have defined). It is then safe to bring down the node by powering down its physical machine or, if running on a cloud platform, deleting its virtual machine.

    First, go to Rancher UI: https://rancher.gimsd6.internal.udev.nga.mil/g/clusters 
    Click global, then navigate to ___(Insert program word for (dev06l)_____
    In the top right corner, click 'Launch kubectl'
    
identify the name of the node you wish to drain. You can list all of the nodes in your cluster with

    *kubectl get nodes*

    Next, tell Kubernetes to drain the node:

    *kubectl drain <node name>*
  
    B. Shut down the server in EC2 Dashboard
    
    C. Start the server in EC2 Dashboard
    
    D. Wait for it to check back into Rancher. When it does, uncordon the node.<br>
      - Once it returns (without giving an error), you can power down the node (or equivalently, if on a cloud platform, delete the virtual machine backing the node).         If you leave the node in the cluster during the maintenance operation, you need to run
        
    *kubectl uncordon <node name>* 

2. For worker nodes, it is going to depend on if we are in an outage window or not. A pre-requisite for no outage is that we need enough worker nodes so that we can cordon off one worker node, and have all workloads able to reschedule to the remaining resources. If we are in an outage window, there’s certain things we can expedite.
A. Drain and cordon the node
o	If multiple nodes exist on the list from DEV AWS:
	If we’re in an outage window:
B. We do not care about the workloads. Drain and cordon all necessary nodes and perform the rest of the procedure in parallel.
	If we’re not in an outage window/trying to prevent outage:
C. We care about the workloads. If we have enough resources, we can drain and cordon sequentially like we would do for the rancher/control plane nodes
D. Shut down the server in EC2 Dashboard
E. Start the server in EC2 Dashboard
F. Wait for it to check back into Rancher. When it does, uncordon the node.



