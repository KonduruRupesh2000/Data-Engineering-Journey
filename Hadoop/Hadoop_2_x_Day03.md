# Hadoop 2.x

## Hadoop 1.x job tracker:
Roles of job tracker:
- Resource Management
- Job scheduling
- Job monitoring

Job Tracker operates on the cluster level, while Task Tracker performs similar roles on the node/system level.

In Hadoop 2.X, there is no Job Tracker or Task Tracker, but the functionality of the Name Node is intact with the functionality of the Resource Manager. They introduced a resource manager called YARN (Yet Another Resource Negotiator).

1. **Global Resource Manager**  
   Assigns resources to the cluster/system and ensures optimal allocation.

2. **Node Manager**  
   Operates on the system level/on each slave, establishes communication between the resource manager, and creates JOB ID.

3. **Application-Specific Application Master**  
   Related to a specific job; submitting one job results in the creation of one application master to communicate regarding that job/task.

4. **Scheduler**  
   Managed by the resource manager; upon creation of a job ID, the resource manager starts scheduling the job.

5. **Container**  
   The actual resources (main memory, CPU) needed to execute.

In certain situations, a single slave/Node can allocate the required resources and complete the job.

### Working:
- The client assigns the job to the Resource Manager.
- The Resource Manager checks for resource availability by sending signals to all nodes in the cluster. Upon response, an Application Master Job is created; it creates an App ID to keep track with the Resource Manager.
- Once the job is completed, the App ID is deleted, and the memory is freed up.

## Need of shared execution to complete task:

### Working:
- The Resource Manager is informed about which system can contribute to shared execution.
- Then it creates a Task JOB - the Node manager will create it.
- An Application master should be created to monitor job execution (the Application ID is a number that the resource manager uses to track the job).
- After the Application ID is created, it is submitted to the Resource Manager. Now, the Resource Manager creates a virtual space (as requested by the application master) called CONTAINER.
- The reason for container creation is to facilitate easy communication between nodes.
- Once the job is completed, the CONTAINER is deleted, and the space is freed up.

## Hadoop 2.x:
1. YARN is introduced.
   - Job Tracking
   - Job Monitoring
   - Job Scheduling tasks done by YARN
2. Java 7
3. Scalability was missing (due to the non-availability of containers).

## Hadoop 3.x (minor differences):
1. YARN along with Container.
2. Java 8
3. It was highly scalable.
