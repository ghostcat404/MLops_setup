# MLops

This repository is a step by step tutorial on deploying multiple MLops tools:

- Jenkins
- MLflow
- Minio

## Table of contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [MLflow](#mlflow)
- [First Jenkins setup](#first-jenkins-setup)
- [Jenkins GitLab setup](#jenkins-gitlab-setup)

## Prerequisites

- docker ([install](https://docs.docker.com/engine/install/))
- docker-compose ([install](https://docs.docker.com/compose/install/))
- conda [Optional] ([install](https://docs.anaconda.com/anaconda/install/) you need CLI version)

## Installation

1. Create ```.env``` file at the root of the project and pass this variables:

```bash
MINIO_ACCESS_KEY=root
MINIO_SECRET_KEY=toortoor
AWS_ACCESS_KEY_ID=root
AWS_SECRET_ACCESS_KEY=toortoor
MLFLOW_S3_ENDPOINT_URL=http://localhost:9000
POSTGRES_USER=root
POSTGRES_PASSWORD=toor
```

2. Up containers via ```docker-compose```:

```bash
docker-compose --env-file ./.env up -d
```

You should see something like this in your terminal:

```bash
Starting s3                         ... done
Starting jenkins                  ... done
Starting postgresql ... done
Starting mlops_pipeline_waitfordb_1 ... done
Starting mlflow_server              ... done
```

***Note: Jenkins setup can take some minutes. Just Wait***

To stop all containers, run

```bash
docker-compose stop
```

3. Install python packages for mlflow
    - use only python3-pip:

        ```bash
        pip3 install -r requirements.txt
        ```

    - use conda

        ```bash
        conda create -n mlflow_env python=3.8
        ```

        ```bash
        conda activate mlflow_env
        ```

        ```bash
        conda install --file requirements.txt
        ```

## MLflow

After all the containers are started, you can enter ```http://localhost:5000``` in your browser and you should see something like this
![plot](./img/mlflow_img.png)

Then you can run

```bash
python3 train_example.py
```

and see experiment in MLflow UI

## First Jenkins setup

- Go to ```https://localhost:8081```. You should see this window
![plot](./img/jenkins_enter_screen.png)
- Go to terminal and enter this command:

```bash
docker-compose logs jenkins
```

- Find this block in logs and copy the key and paste it to Jenkins window:

```bash
jenkins      | *************************************************************
jenkins      | *************************************************************
jenkins      | *************************************************************
jenkins      | 
jenkins      | Jenkins initial setup is required. An admin user has been created and a password generated.
jenkins      | Please use the following password to proceed to installation:
jenkins      | 
jenkins      | c0cd2e7c8a5d48b7b0d336c544ce6caa
jenkins      | 
jenkins      | This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
jenkins      | 
jenkins      | *************************************************************
jenkins      | *************************************************************
jenkins      | *************************************************************
```

- In the next window choose ```Select plugins to install``` and:
  - choose all plugins in ```Pipelines and Continuous Delivery``` section
  - choose ```Git parameter, GitHub, GitLab``` in ```Source Code Management``` section

- click install button and wait some minutes

- create admin user
![plot](./img/jenkins_admin_user.png)

- follow finish steps and start using Jenkins

## Jenkins GitLab setup
