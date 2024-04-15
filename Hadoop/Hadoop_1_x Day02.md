# Hadoop 1.x

Hadoop 1.x follows the concept of Master-slave, similar to a manager and employees in a team.

## Master-Slave Relationship


|  Master  |--------------------> | Slave  |

## Architecture Diagram





                                        | TaskTracker1 |- | DataNode1 |

                                        | TaskTracker2 |- | DataNode2 |
| NameNode |----- | JobTracker   |------
                                        | TaskTracker2 |- | DataNode3 |

                                        | TT4          |- | DataNode4 |


In the above image:

- NameNode + JobTracker == Master
- TaskTracker + DataNode == Slave

NameNode - Meta Data, Memory Mapping
DataNode - It holds Actual data, Process done here
JobTracker - It controls overall execution in a cluster
TaskTracker - It always runs on individual machines and individual processing

Example: I have a file size of 260Mb (split into 128Mb - Block1, 128Mb - Block2, 4Mb - Block3)
In Hadoop 1.x Architecture:

The NameNode contains information about the available nodes to complete this task, considering factors like memory and compute. It passes this task to the JobTracker. The JobTracker sends signals to the TaskTrackers, and whichever TaskTracker node responds first, it allots the first chunk/Block of data to that device for execution. Similarly, data is distributed to other devices.

The problem that can arise is system failure: Master can fail / Slave can fail.

If a Slave fails, we might encounter DATA LOSS.
If the Master fails, everything will be LOST.

**Solution for Slave failure**


+-------+-------+-------+
|  RACK 1  |  RACK 2  |  RACK 3  |
+-------+-------+-------+

                            | 1. B1 | 5. B1 | 9.    |

                            | 2. B3 | 6. B1 | 10. B2|

                            | 3. B3 |    B2 | 11. B2|

                            | 4.    | 7.    | 12. B3|

+-------+-------+-------+---------+---------+----------------+------+

==> Block 1, Block 2, Block 3



Here, the NameNode has an idea of how data is stored in different racks. In case of failure, the JobTracker sends signals to know the status of other replicas. Once it receives a signal from a DataNode, it moves forward with that node. It always ensures to have at least three replicas.

**Solution for Master (NameNode) failure**


+---------------+                                  +------------------------+
|   NameNode    |                                  |   CheckPoint Node      |
|               |                                  |  Secondary NameNode    |
+---------------+                                  +------------------------+
| edits         |                                  | edits                  |
|               |                                  |                        |
|               |                                  +-------------------------+
+---------------+                                  | fsimage                |
| fsimage       |                                  |                        |
|               |                                  |           ..............|
|               |                                  |                        |
|               |---First time copy---------------->fsimage.ckpt           |
+---------------+                                  +------------------------+
| edits.new     |
|               |
| temporary     |
| during checkpoint|
+---------------+
 Master Node 1                                       Master Node 2

 
- Initially, the NameNode has an FS_image (time=0) with information about the current architecture at Time=0.
- The Secondary node stores FS_images and Log_edits_20 (logs about what happened from timestamp 0-20 seconds).
- Checkpointing happens in the secondary node - meaning combining Log_edits_20 + FS_images = New_FSImage_20.
- Now the secondary node pings the primary node that it has a new file. Whenever the primary node is free, it receives the new file.
- The secondary node is not a replica of the primary node; it's just a checkpointing node.
- Since it's HDFS, the Secondary node will have replicas (in case of failure).

Finally, if the NameNode fails, the admin will try to create a new NameNode with