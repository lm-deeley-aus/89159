# StaticKube Instance Reboot

This doc is to answer the question, what is the right way to reboot a StaticKube instance if it is on the AWS DEV list to be rebooted?

##Steps
  
  *For Rancher and Mission Control Nodes (no outage necessary)*<br>
    1. Safely Drain a Node and cordon the node<br>
      -  NOTE: If the DEVAWS list contains multiple nodes, it is highly advised that this step gets performed one node at a time.<br> 
      - **ALWAYS** ensure that there are at least two nodes, you risk causing an outage when there are less then two nodes working.<br> 
      - Link to Steps: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/a#before-you-begin
    
    2. Shut down the server in EC2 Dashboard
    
    3. Start the server in EC2 Dashboard
    
    4. Wait for it to check back into Rancher. When it does, uncordon the node.<br>
      - Go back to the link in step 1 to complete the rest of the tasks 
