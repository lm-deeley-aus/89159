# StaticKube Instance Reboot

This doc is to answer the question, what is the right way to reboot a StaticKube instance if it is on the AWS DEV list to be rebooted?

##Steps
  
  *For Rancher and Mission Control Nodes (no outage necessary)*<br>
    1. Safely Drain a Node and cordon the node<br>
      -  NOTE: If the DEVAWS list contains multiple nodes, it is highly advised that this step gets performed one node at a time.<br> 
      - **ALWAYS** ensure that there are at least two nodes, you risk causing an outage when there are less then two nodes working.<br> 
      - Follow the steps below or navigate to here: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/a#before-you-begin
<br><br> Use kubectl drain to remove a node from service
You can use kubectl drain to safely evict all of your pods from a node before you perform maintenance on the node (e.g. kernel upgrade, hardware maintenance, etc.). Safe evictions allow the pod's containers to gracefully terminate and will respect the PodDisruptionBudgets you have specified.

Note: By default kubectl drain ignores certain system pods on the node that cannot be killed; see the kubectl drain documentation for more details.
When kubectl drain returns successfully, that indicates that all of the pods (except the ones excluded as described in the previous paragraph) have been safely evicted (respecting the desired graceful termination period, and respecting the PodDisruptionBudget you have defined). It is then safe to bring down the node by powering down its physical machine or, if running on a cloud platform, deleting its virtual machine.

    First, identify the name of the node you wish to drain. You can list all of the nodes in your cluster with

    *kubectl get nodes*

    Next, tell Kubernetes to drain the node:

    *kubectl drain <node name>*
  
    2. Shut down the server in EC2 Dashboard
    
    3. Start the server in EC2 Dashboard
    
    4. Wait for it to check back into Rancher. When it does, uncordon the node.<br>
      - Once it returns (without giving an error), you can power down the node (or equivalently, if on a cloud platform, delete the virtual machine backing the node).         If you leave the node in the cluster during the maintenance operation, you need to run
        
    *kubectl uncordon <node name>* 
