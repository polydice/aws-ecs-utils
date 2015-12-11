# AWS Container Service (ECS) CLI utils

Pack of useful CLI commands for working with Amazon AWS Container Service.

## Installation

1. (Prereq) Install `awscli` (https://aws.amazon.com/cli/) and `jq` (https://stedolan.github.io/jq/)
2. Make sure you have `aws` configured with stored authentication
4. Download `*.sh` files from this repository
5. Add them to your `/usr/local/bin`
6. Add path to your ECS PEM file to `ECS_PEM_FILE` environment variable

## Commands

### Conventions

All commands expect your `aws` command is configured with default AWS region. Also majority of commands expect `ECS_PEM_FILE` ENV variable to be pointing to PEM file for SSH access to ECS cluster hosts.

### `ecs-connect`

Connect to running container of specific service. 

```bash
ecs-connect cluster-name service-name
```

This is useful to investigate inner state of processes inside container or run processes with available environment variables from task definition.

If your service is running more than 1 task, first task provided by `aws` interface is used to connect to.

If your task has more than 1 containers, first container is taken.

Warning: Be aware that you are inside container that runs the main process already. Starting heavy processes may cause the whole container will run out of memory and be killed.


### `ecs-run-shell`

Start new ECS task with overridden CMD to `sleep 30m` and connect to its shell. Stops tasks after shell is exited.

```bash
ecs-run-shell cluster-name task-definition-name[:version]
```

This is useful to run your app framework console (like `rails console`) within environment of your ECS service or task without affecting containers running services.

If task definition defines more than 1 container, all of containers are started but it connects you to first one.
