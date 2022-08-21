- [Overview](#overview)
- [Instructions](#instructions)
  - [Using Docker Build](#using-docker-build)
  - [Using Docker Registry](#using-docker-registry)

# Overview
Developer tools repository contains code required to build the docker container which that data engineers use to build applications.

# Instructions

## Using Docker Build

```bash
docker-compose up --remove-orphans -d --build
```

## Using Docker Registry

Use the image `entechlog/developer-tools`