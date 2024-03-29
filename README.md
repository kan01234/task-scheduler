# task-scheduler
Project describle a task scheduler system, target to run each scheduled task at specifed time once, even in a clustered environment.

## Quick Start
1. start the system
```bash
docker-compose up -d;
```

2. add schedule Task
[check here](https://github.com/kan01234/task-scheduler-producer/blob/master/README.md#add-schedule-task)

3. check log in consumer-logs folder

## System Architecture
![system architecture](https://github.com/kan01234/task-scheduler/blob/master/img/task-scheduler-system.png)

- other service
  - other services in the system

- task scheduler producer
  - accept request and transfrom the Task

- kafka queue
  - as task persistence
  - let task scheduler consumer poll the task from queue

- task scheduler consumer
  - as task consumer, poll task from queue and store to database
  - fetch task from database and execute it

- task database
  - store the task

- task executor
  - execute the task

## Requriements
- Task Scheduler Producer
  - end point to create schedule task
  - push schedule task to kafka queue

- Task Scheduler Consumer
  - poll schedule task from kafka queue
  - save schedule task to task database
  - fetch schedule task from task database and execute it
  - run once only

### Running Scheduled Task In Cluster
We are going to running schedule task only on the master in cluster, so the task consumer will also have following requeirments.

- master flag for each node
- only one alive master node in cluster
- only master node can assign or execute schedule task
- node will send heart beat in a fixed rate
- assume alive time to determine node is alive or not
- resolve master node in a fixed rate, to ensure master node is available

- - - -

1. application startup

![task-consumer-appnode-init](https://github.com/kan01234/task-scheduler/blob/master/img/task-consumer-appnode-init.png)

- init application
- check alive master
- set to master if no available master
- save application node

- - - -

2. resolve master

![task-consumer-resolve-master](https://github.com/kan01234/task-scheduler/blob/master/img/task-consumer-resolve-master.png)

assume node1 is master
- node1, node2 send heat beat
- node1, node2 execute resolve master

assume node1 is down now
- node2 send heat beat
- node2 execute resolve master, but no master is alive
- find new master and save

- - - -

3. execute scheduleing task

![task-consumer-execute-task](https://github.com/kan01234/task-scheduler/blob/master/img/task-consumer-execute-task.png)

assume node1 is master
- node1, node2 start execute schedule task
- node1 is master, fetch schedule task from database
- node1 update task status, assign the schedule task to task executor
- node2 is not master, return

### Implement
[task-scheduler-producer](https://github.com/kan01234/task-scheduler-producer)

[task-scheduler-consumer](https://github.com/kan01234/task-scheduler-consumer)
