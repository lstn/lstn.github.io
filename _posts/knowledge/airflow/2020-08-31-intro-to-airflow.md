---
layout: post
title: "Apache Airflow"
date: 2020-08-31
categories: [knowledge, airflow]
tags: [apache]
---

# Apache Airflow: Intro

Airflow is a tool to build, schedule and monitor data pipelines.

A data pipeline is a set of data processing elements connected in serties. The output of one element is the input of the next one.

## Building blocks of Airflow

- **Operator** (Worker)
    Knows *how* to perform a task and has the *tools* to do it.

    Example:
    - Python Operator
    - Postgres Operator
    - Bash Operator
    - Email Operator

- **DAG - Directed Acyclic Graph** (Protocol/Instructions)
    Describes the order of tasks and what to do if a task is failing.

    Example:
    Run Task A, when it is finished, run Task B. If one of the tasks failed, stop the whole process and send me a notification.

- **Task** (Specific job)
    Job that is done by an Operator.

    Example:
    - Load data from some API using Python Operator
    - Write the data to the database using MySQL Operator

- **Connection**
    Credentials to the external systems that can be securely stored in the Airflow.

    Example:
    - Postgres Connection = Connection string to the Postgres database
    - AWS Connection = AWS access keys

- **Hooks**
    Interfaces to the external platforms and databases. Implements common interface (all hooks look very similar) and use Connections

    Example:
    - S3 Hook
    - Slack Hook
    - HDFS Hook

- **Variables**
    Like environment variables. Can store arbitrary information and be used in the Tasks

    Example:
    - Stack Overflow base URL
    - Gmail client ID and Secret

- **XComs**
    Lets Tasks exchange *small* messages.

## Quickstart

```python
# airflow needs a home, ~/airflow is the default,
# but you can lay foundation somewhere else if you prefer
# (optional)
export AIRFLOW_HOME=~/airflow

# install from pypi using pip
pip install apache-airflow

# initialize the database
airflow initdb

# start the web server, default port is 8080
airflow webserver -p 8080

# start the scheduler
airflow scheduler

# visit localhost:8080 in the browser and enable the example dag in the home page
```

