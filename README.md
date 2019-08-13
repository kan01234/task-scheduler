# task-scheduler
Project describle a task scheduler system, target to run each scheduled task at specifed time once, even in a clustered environment.

## System Architecture
![system architecture](https://github.com/kan01234/task-scheduler/blob/master/img/task-scheduler-system.png)

1. other service
- other services in the system

2. task scheduler producer
- accept request and transfrom the Task

3. kafka queue
- as task persistence
- let task scheduler consumer poll the task from queue

4. task scheduler consumer
- as task consumer, poll task from queue and store to database
- fetch task from database and execute it

5. task database
- store the task

6. task executor
- execute the task

### Running Scheduled Task In Multi Nodes
1. application startup

![task-consumer-appnode-init](https://github.com/kan01234/task-scheduler/blob/master/img/task-consumer-appnode-init.png)

2. resolve master

![task-consumer-resolve-master](https://github.com/kan01234/task-scheduler/blob/master/img/task-consumer-resolve-master.png)

3. execute scheduleing task

![task-consumer-execute-task](https://github.com/kan01234/task-scheduler/blob/master/img/task-consumer-execute-task.png)

### Implement (doing...)
[task-scheduler-producer](https://github.com/kan01234/task-scheduler-producer)

[task-scheduler-consumer](https://github.com/kan01234/task-scheduler-consumer)
