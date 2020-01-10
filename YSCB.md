## Setup and run YCSB tests on the clusters

 1. Steps to set up and run YCSB tests on both clusters are identical. 
     - On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` ssh <sshuser>@<clustername>.azurehdinsight.net ```
     - Download the YCSB repository from the below destination  ``` curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz
    ```
     - Run the below steps to create the HBase tables which will be used to load the datasets
 2. List item

          

```hbase shell ```





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUyODU0NjA3MCwtMTU2MTM4MzI3MywxNT
QyMTMzNzAsMTUxMTIxMjI5Nl19
-->