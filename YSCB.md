## Setup and run YCSB tests on the clusters

1. Steps to set up and run YCSB tests on both clusters are identical. 
2. On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the cluster.  The path should have below format. 

    ```
    ssh <sshuser>@<clustername>.azurehdinsight.net
    ```
3. Download the YCSB repository from the below destination 
```
curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz
```
4. Run the below steps to create the HBase tables which will be used to load the datasets
```hbase shell ```





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA2MDgzNzcyNywtMTU2MTM4MzI3MywxNT
QyMTMzNzAsMTUxMTIxMjI5Nl19
-->