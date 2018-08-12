# ecs-run

This utility allows you to run an arbitrary command in a container on an ECS cluster. It is similar to the aws-cli command `aws ecs run-task` however with a number of benefits:

* Can wait for the ECS task to finish before exiting
* Exits with the exit code of the command which was run in the container
* Can stream the output from the command (if the container uses AWS Cloudwatch logging)
* Can override placement requirements (CPU and memory) of both the container to run the task in and other containers in the task definition
* Doesn't require a JSON object to be constructed and passed on the command line for such overrides (making it easier to use in some CI/CD systems)

## Usage

```
usage: ecs-run.py [-h] [--cluster ECS_CLUSTER]
                  [--task-definition ECS_TASK_DEF] [--region AWS_REGION]
                  [--profile AWS_PROFILE] [--count TASK_COUNT]
                  [--started-by TASK_STARTED_BY]
                  [--cmd-container CMD_CONTAINER]
                  [--cmd-container-cpu CMD_CONTAINER_CPU]
                  [--cmd-container-memory-reservation CMD_CONTAINER_MEMORY_RESERVATION]
                  [--cmd-container-memory-limit CMD_CONTAINER_MEMORY_LIMIT]
                  [--other-container-cpu OTHER_CONTAINER_CPU]
                  [--other-container-memory-reservation OTHER_CONTAINER_MEMORY_RESERVATION]
                  [--other-container-memory-limit OTHER_CONTAINER_MEMORY_LIMIT]
                  [--wait] [--timeout TIMEOUT] [--logs] [--quiet]
                  [COMMAND [COMMAND ...]]

Runs an arbitrary command in a container on an ECS cluster. Optionally wait for it to finish and stream log output.

positional arguments:
  COMMAND               Command to run (default: -)

optional arguments:
  -h, --help            show this help message and exit
  --cluster ECS_CLUSTER
                        Name of ECS cluster to run the task (default: -)
  --task-definition ECS_TASK_DEF
                        Task definition to run (default: -)
  --region AWS_REGION   AWS region in which the ECS cluster resides (default:
                        -)
  --profile AWS_PROFILE
                        AWS profile to use (default: -)
  --count TASK_COUNT    Number of instances of the task to start (default: 1)
  --started-by TASK_STARTED_BY
                        Tag to pass to AWS to identify what started the task
                        (default: 'ecs-run-task')
  --cmd-container CMD_CONTAINER
                        Which container to run the command in (default: -)
  --cmd-container-cpu CMD_CONTAINER_CPU
                        Required CPU unit reservation for the command
                        container (default: -)
  --cmd-container-memory-reservation CMD_CONTAINER_MEMORY_RESERVATION
                        Required memory reservation for the command container
                        reservation (MB) (default: -)
  --cmd-container-memory-limit CMD_CONTAINER_MEMORY_LIMIT
                        Memory limit for command container - will be killed if
                        exceeds this (MB) (default: -)
  --other-container-cpu OTHER_CONTAINER_CPU
                        Required CPU unit reservation for other containers
                        (default: -)
  --other-container-memory-reservation OTHER_CONTAINER_MEMORY_RESERVATION
                        Required memory reservation for other containers (MB)
                        (default: -)
  --other-container-memory-limit OTHER_CONTAINER_MEMORY_LIMIT
                        Memory limit for other containers - will be killed if
                        exceed this (MB) (default: -)
  --wait                Wait for command to exit (default: False)
  --timeout TIMEOUT     Timeout (in seconds) when --wait or --logs is
                        specified. Zero to wait indefinitely (default).
                        (default: 0)
  --logs                Stream cloudwatch log output from command (implies
                        --wait) (default: False)
  --quiet               Do not output periodicadlly to stderr whilst the
                        process is running (default: False)
```

