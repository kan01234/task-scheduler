# task-scheduler

## System Architecture
![system architecture](https://github.com/kan01234/task-scheduler/blob/master/img/task-scheduler-system.png)

1. other service
- other services in the system

2. task scheduler producer
- accept request and transfrom the Task

3. kafka queue
- as task persistence
- assign received task to Task scheduler consumer pull the task

4. task scheduler consumer
- as task consumer, poll task from queue and store to database
- fetch task from database and execute it

5. task database
- store the task

6. task executor
- execute the task
