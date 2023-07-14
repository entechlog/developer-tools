- [Overview](#overview)
  - [List of tools in this container](#list-of-tools-in-this-container)
- [Instructions](#instructions)
  - [Using Docker Build](#using-docker-build)
  - [Using Docker Registry](#using-docker-registry)
  - [Other containers](#other-containers)
    - [Jupyter](#jupyter)
    - [LocalStack](#localstack)
  - [Clean Resources](#clean-resources)

# Overview
Developer tools repository contains code required to build the docker container which that data engineers use to build applications.

## List of tools in this container

| Name        | Command to validate Install |
| ----------- | --------------------------- |
| dbt         | dbt --version               |
| awscli      | aws --version               |
| aws-sam-cli | sam --version               |
| streamlit   | streamlit --version         |
| terrafrom   | terraform --version         |
| asciinema   | asciinema --version         |
| snowsql     | snowsql --version           |

# Instructions

## Using Docker Build
Image is build locally, This helps to make changes to Dockerfile if needed for testing

```bash
docker-compose up --remove-orphans -d --build
```

## Using Docker Registry
Uses the pre-build image `entechlog/developer-tools` from docker registry

```bash
docker-compose -f docker-compose-reg.yml up -d --build
```

## Other containers

### Jupyter
```bash
docker-compose -f docker-compose-jupyter.yml up -d --build
```

### LocalStack
```bash
docker-compose -f docker-compose-localstack.yml up -d --build
```

## Clean Resources

```bash
docker-compose down --volumes 
```