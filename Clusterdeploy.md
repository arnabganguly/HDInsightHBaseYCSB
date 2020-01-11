## Provision HDInsight  HBase cluster with Azure Management Portal

To provision HDInsight HBase with the new experience on  Azure Management Portal, perform the below steps. 

1. Go to the Azure Portal portal.azure.com. Login using your azure account credentials.

![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image001.png)
    
2. Select  **Create a resource -> Analytics -> HDInsight**

![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image002.png)



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
![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image004.png)

4. Click **Next:Storage**  to launch the Storage Tab and populate the below fields 

- **Primary Storage Type**: Azure Storage.
 - **Selection Method**: Select from a list Radio Button
 - **Primary Storage Account**:  A default storage account name(formed from your cluster name) and a corresponding container should be prepopulated. Click **OK** to select these.


![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image005.png)
 - Leave the rest of the options untouched and scroll down to check the checkbox **Enable HBase accelerated writes**.  *(Note that we would later be creating a second  cluster without accelerated writes using the same steps but with this box unchecked.)* 


![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image006.png)

5. Leave the **Security+Networking** blade to its default settings with no changes and go to the **Configuration+pricing** tab. 



6. In the **Configuration+pricing** tab, note the **Node configuration** section now has a line Item titled **Premium disks per worker node**. 
7. Choose the Region node to **10** and Node Size to **DS14v2**(*you could chooser smaller number and size also but ensure that the both clusters have identical number of nodes and VM SKU to ensure parity in comparison*) 

![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image007.png)

8. Click **Next: Review + Create**

9. In the Review and Create tab , ensure that **HBase Accelerated Writes** is Enabled under the **Storage** section. 

![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image008.png)

10. Click **Create** to start deploying the first cluster with Accelerated Writes. 

11. Repeat  the same steps again to create a second cluster without Accelerated writes by unchecking the **Enable Accelerated Writes** checkbox on the storage tab. 

![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image010.png)

12. In the **Configuration+pricing** tab for this cluster , note that the **Node configuration** section  does NOT have a **Premium disks per worker node** line item.
13. Choose the Region node to **10** and Node Size to **D14v2**.(*Also note the lack of DS series VM types like earlier*) 

![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image009.png)


14. Click **Create** to start deploying the second cluster without Accelerated Writes. 
15. Now that we are done with cluster deployments , in the next section we would set up and run  YCSB tests on both these clusters. 


[NEXT-->](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/YSCB.md)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMTMwOTc2NjgsLTczNjEzMTQ3LDMxNz
g4MTUwMiwtNzMzNzYyOTIyLDc4ODI4NTYxNSwtNDMxNTQ1OTI0
LDEyMDEzNzg1OTldfQ==
-->