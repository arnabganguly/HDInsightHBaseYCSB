## Provision HDInsight  HBase cluster with Azure Management Portal

To provision HDInsight HBase with Azure Management Portal, perform the below steps. 

1. Go to the Azure Portal portal.azure.com. Login using your azure account credentials.
    
2. Select  **Create a resource -> Analytics -> HDInsight**

3. On the Basics Tab populate the below fields towards the creation of an HBase cluster. 
 - **Cluster Name**: *Enter the cluster name. A green tick will appear if the cluster name is available.*
 - **Region**: Enter the name of the region of deployment
 - **Cluster Type** : Cluster Type -  *HBase* 
  Version-   *HBase 2.0.0(HDI 4.0)* 
 - **Cluster login username**:Enter username for cluster administrator(default:admin) 
 - **Cluster login password**:*Enter password for cluster login(default:sshuser)*
 - **Confirm cluster login password**: *Enter the same password for cluster login(default:sshuser)*
 - *Check the box for Use cluster login password for SSH*
 - **Resource Group**:*Put the cluster in the same resource group as the Storage account and MI.* 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1NzE5NTM3NCwxMjAxMzc4NTk5XX0=
-->