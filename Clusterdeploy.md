## Provision HDInsight  HBase cluster with Azure Management Portal

To provision HDInsight HBase with the new experience on  Azure Management Portal, perform the below steps. 

1. Go to the Azure Portal portal.azure.com. Login using your azure account credentials.
    
2. Select  **Create a resource -> Analytics -> HDInsight**

3. On the Basics Tab populate the below fields towards the creation of an HBase cluster. 

 - **Subscription**: *Should be autopopulated with the subscription details*
 - **Resource Group**: Enter a resource group for holding your HDInsight HBase deployment

    <br>
    <br>

 - **Cluster Name**: Enter the cluster name. A green tick will appear if the cluster name is available.
 - **Region**: Enter the name of the region of deployment
 - **Cluster Type** : Cluster Type -  *HBase* 
  Version-   *HBase 2.0.0(HDI 4.0)* 
 - **Cluster login username**:Enter username for cluster administrator(default:admin)
 - **Cluster login password**:Enter password for cluster login(*default:sshuser*)
 - **Confirm Cluster login password**: Confirm the password entered in the last step 
 - **Secure Shell(SSH) username**: Enter the SSH login user  (*default:sshuser*)
 - **Use cluster login password for SSH**: Check the box to use the same password for both SSH logins and Ambari Logins etc. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ0MDQ2NzQxNCwxMjAxMzc4NTk5XX0=
-->