## Provision HDInsight  HBase cluster with Azure Management Portal

To provision HDInsight HBase with the new experience on  Azure Management Portal, perform the below steps. 

1. Go to the Azure Portal portal.azure.com. Login using your azure account credentials.
    
2. Select  **Create a resource -> Analytics -> HDInsight**

3. On the Basics Tab populate the below fields towards the creation of an HBase cluster. 

 - **Subscription**: *Should be autopopulated with the subscription details*
 - **Resource Group**: Enter a resource group for holding your HDInsight HBase deployment


 - **Cluster Name**: Enter the cluster name. A green tick will appear if the cluster name is available.
 - **Region**: Enter the name of the region of deployment
 - **Cluster Type** : Cluster Type -  *HBase* 
  Version-   *HBase 2.0.0(HDI 4.0)* 
 - **Cluster login username**:Enter username for cluster administrator(default:admin) 
 - **Cluster login password**:Enter password for cluster login(*default:sshuser*)
 - **Use cluster login password for SSH**: Enter the same password for cluster login(*default:sshuser*)
 - Check the box for Use cluster login password for SSH



4. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1Mzc0MjQ0MzcsMTIwMTM3ODU5OV19
-->