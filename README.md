#Q1 Solution:

- This is the formula to estimate the number of data nodes (n):

n= H/d

- where, d= disk space available per node. H= Hadoop storage

but H=crS/(1-i)

- Hence final formula to calculate number od data nodes is

n = c*r*S/(1-i)*d 
- where d= disk space available per node. c = average compression ratio. It depends on the type of compression used (Snappy, LZOP, ...) and size of the data. When no compression is used, c=1. r = replication factor. It is usually 3 in a production cluster. S = size of data to be moved to Hadoop. i = intermediate factor. It is usually 1/3 or 1/4. Hadoop's working space dedicated to storing intermediate results of Map phases.

consider c=1; r=3; i=1/4; S=600TB ...given d=7TB ...given

Total no. of data nodes = 1*3*600/((1-1/4)*7) 
                        = 342.857 
                        = 343
                        
                        
                        
#Q2 Solution:

- Although the default blocks size is 64 MB in Hadoop 1x and 128 MB in Hadoop 2x whereas in such a scenario let us consider block size to be 100 MB which means that we are going to have 5 blocks replicated 3 times (default replication factor). Let’s consider an example of how does a block is written to HDFS:

- We have 5 blocks (A/B/C/D/E) for a file, a client, a namenode and a datanode. So, first the client will take Block A and will approach namenode for datanode location to store this block and the replicated copies. Once client is aware about the datanode information, it will directly reach out to datanode and start copying Block A which will be simultaneously replicated to other 2 datanodes. Once the block is copied and replicated to the datanodes, client will get the confirmation about the Block A storage and then, it will initiate the same process for next block “Block B”.

- So, during this process if 1st block of 100 MB is written to HDFS and the next block has been started by the client to store then 1st block will be visible to readers. Only the current block being written will not be visible by the readers
