
# Hadoop Architecture


## Overview

To deal with the 3V's or 5V's of Big Data, we utilize Hadoop Architecture, which primarily offers two approaches:

- Scale-up
- Scale-out

### Scale-up

Scale-up, or vertical scaling, refers to increasing the capacity of a single machine by adding more resources to it, such as memory and storage. It's akin to upgrading an existing server with more powerful hardware.

- **Storage**: Initially, 1 GB of memory and 1 TB of storage are provided.
- **Intermediate Upgrade**: The machine is then scaled up to 5 GB of memory and 5 TB of compute space.
- **Final Upgrade**: Eventually, it reaches 10 GB of memory and 10 TB of storage, significantly enhancing its capacity.

The scale-up approach makes the machine more powerful by upgrading its internal components.

### Scale-out

Scale-out, or horizontal scaling, involves adding more machines to a system to increase overall capacity. Instead of upgrading a single machine, multiple machines are networked together to share the workload.

- **Initial Configuration**: Starts with a base unit of 1 GB memory and 1 TB storage.
- **Expansion**: More machines of the same capacity are added to the system.
- **Extended Capacity**: As demand increases, even more machines are networked into the system, each contributing 1 GB of memory and 1 TB of storage to handle larger data sets.

Scale-out is particularly known for its reliability and robust nature, and its ability to provide fault tolerance—if one node fails, others in the cluster can pick up the slack.

Among these approaches, Scale-out has become the preferred strategy for Hadoop due to its flexibility in handling data and computation distribution.

#### Learning Points

- How we use the Scale-out approach
- How to configure Scale-out
- Handling failure (fault tolerance) with HDFS

For example, in a Scale-out scenario, a dataset is distributed among 5 devices. If one of the devices fails or crashes, we might face data loss. This case is addressed using HDFS which ensures data replication and recovery in the event of a failure.

## Computation of Data

When dealing with the computation of data, we have two main ways:

### 1. Legacy Approach

- **Client** sends a query to the **Database** (which contains the data).
- **I/O**: Computation happens here. Data will be passed to this device (as the database doesn't have the capability of computation).

This approach was utilized in legacy systems. However, moving data from one device to another is not ideal when it comes to Big Data.

### 2. Modern Approach

- **Storage and Execution**: Have a device that has both capabilities of storage and computation, similar to a laptop.
- Instead of moving data from one device to another, we move the code (e.g., 100,000 lines == 10kb only) from the Query to Storage/Execution.

This results in the Hadoop 1.x architecture, which incorporates Scale-out and on-device computation, allowing for more efficient data processing.
