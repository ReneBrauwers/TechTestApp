# TechTestApp
[![Build Status][git-cicd-badge]][gitworkflow]
[![Release][release-badge]][release]
[![GoReportCard][report-badge]][report]
[![License][license-badge]][license]

[git-cicd-badge]: https://github.com/ReneBrauwers/TechTestApp/workflows/CICD/badge.svg
[gitworkflow]: https://github.com/ReneBrauwers/TechTestApp/actions?query=workflow%3ACICD
[release-badge]: http://img.shields.io/github/release/ReneBrauwers/TechTestApp/all.svg?style=flat
[release]:https://github.com/ReneBrauwers/TechTestApp/releases
[report-badge]: https://goreportcard.com/badge/github.com/ReneBrauwers/TechTestApp
[report]: https://goreportcard.com/report/github.com/ReneBrauwers/TechTestApp
[license-badge]: https://img.shields.io/github/license/ReneBrauwers/TechTestApp.svg?style=flat
[license]: https://github.com/ReneBrauwers/TechTestApp/license


## Documentation structure

[readme.md](readme.md) - this file
[gitaction-config.md](git-action-config.md) - how to configure git actions (secrets used for deployment and service tasks)

### Architecture Design Records (ADR)

Architectural decisions are recorded in the `adr` folder, details on why can be found in the [first entry](adr/0001-record-architecture-decisions.md)

Naming convention: `####-<decision title>` where the first 4 digits are iterated by 1 for each record.

## Tech Test Application

Single page application designed to run effortlessly in a container or on a vm (IaaS). The application relies on a postgres database to store data.

It is completely self contained, and should not require any additional dependencies to run.

## Test Drive (live) version
In order to view the last deployed version [![Build Status][git-cicd-badge]][gitworkflow] visist the releases page [![Release][release-badge]][release]. 

Please do note that the application might have been removed since the last Release in order to reduce costs and just keep the used GC subscription clean.

## Installation

### Local using source
Please refer to the [original installation documentation](https://github.com/servian/TechTestApp/blob/master/doc/readme.md) from Servian

### Local using binary
Please refer to the original git repo from servian. The binary can be downloaded from the [release folder](https://github.com/servian/TechTestApp/releases)

### Local (or in a VM) using Docker Container

1. Ensure you have installed either 
    1) [Docker Desktop on Windows](https://docs.docker.com/docker-for-windows/install/) 
    2) [Docker Desktop on Mac](https://docs.docker.com/docker-for-mac/install/)
    3) [Docker engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/) and [Docker compose](https://docs.docker.com/compose/install/)
2. Clone this Repo and unzip (if necessary)
3. Open entrypoint.sh and comment the last line and uncomment line 3 (adding a wait of 10 sec, before continuing allowing postgres to be ready)
![entrypointmodifications](images/animated-entrypoint-modifications.gif)
4. Open docker-compose.yaml and update line 23 context and line 25
![dockercomposemodifications](images/animated-dockercompose-modifications.gif)
5. Open a terminal and browse to the directory containing the docker-compose.yaml file.
6. Enter the following command: docker-compose up 
7. Wait a bit and then you're ready to go, use
   1) Go to http://localhost:8080 to manage your local postgres instance (server: postgresdb, user/pass postgres) 
   2) Go to http://localhost:81 to view the TechTestApp in action. 

### Cloud Deployment
If you are intending to deploy into Google Cloud, please ensure you complete the steps mentioned in the next sections; if you are intending to deploy the solution into
a different cloud such as Azure or AWS then please note that you will need to modify the workflow contained in the.github directory in order to replace the google specific 
services (Google Cloud SQL and Google Cloud Run)

Note; the solution has been tested against PostgreSQL version 9.6 and 10.7, but it is expected to work with older versions as well.

### Google Cloud
#### Create a new project
* [Create a new Google Project](https://cloud.google.com/resource-manager/docs/creating-managing-projects)

#### API Registrations
Ensure the following Google Admin APIs have been enabled (in the project you are intending to use)
* [Cloud SQL API](https://console.cloud.google.com/flows/enableapi?apiid=sqladmin&redirect=https://console.cloud.google.com)
* [CLoud RUN API (Fully Managed)](http://console.cloud.google.com/apis/library/run.googleapis.com)
* [Serverless VPC Access API ](https://console.cloud.google.com/marketplace/details/google/vpcaccess.googleapis.com)
* [Container Registry AP](https://console.cloud.google.com/flows/enableapi?apiid=containerregistry.googleapis.com&redirect=https://cloud.google.com/container-registry/docs/quickstart)

#### Create a Service Account

Using Google Cloud Console or the CLI create a [service account](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwi__KadgJ3qAhXExjgGHdiNBpAQFjAAegQIAhAB&url=https%3A%2F%2Fcloud.google.com%2Fiam%2Fdocs%2Fcreating-managing-service-accounts&usg=AOvVaw0g7W6sYGlfhjgvUJqJmcAm) which for demo purposes is a member of the following groups:
* Cloud Build Service Account
* Cloud Build Editor
* Service Account User
* Cloud Run Admin
* Viewer
* Serverless VPC Access Admin

Once you have created the service account and assigned the rights; please create a [JSON service account key](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) and note it down)

for real-life scenario's it is recommended to apply the [least privilege access principle](https://en.wikipedia.org/wiki/Principle_of_least_privilege) 

#### Provisioning all required Google Services
In order for the solution to work the following services need to be manually provisioned. See links below explaining how to do this.

!IMPORTANT! Please deploy all services in the same region; this repo leverages asia-northeast1
* [Cloud SQL for PostgreSQL: Create a new instance](https://cloud.google.com/sql/docs/postgres/create-instance) (note; please configure using a private IP)
* [Cloud Run (fully managed): Create a new instance](https://cloud.google.com/run/docs/setup)  
* [Container Registry: Create a new instance](https://cloud.google.com/container-registry/docs/quickstart) 

Note; it is the intention to automate these steps as well in a later phase.



## Continuous Integration and Deployment
[![Build Status][git-cicd-badge]][gitworkflow]
[![Release][release-badge]][release]

This repo has been designed and setup in such a way that it will build and deploy into Google Cloud for every commit pushed into the master branch. The whole CICD 
cycle has been defined using Github actions; the riginal Servian application leverages circleci for this. Once a new version has been deployed the CICD process
creates a new release which contains the link to the active online running application. 

[Click to view the Github action used for CICD](../.github/workflows/deploy_to_cloud_run.yaml) 
[Click to read how to configure the secrets for GitHub Actions](git-action-config.md)



## MISC
THis section details some other noteworthy  aspects of the TechTestApp. So please do read on...

### Repository structure

``` sh
.
├── assets      # Asset directory for the application
│   ├── css     # Contains all the css files for the web site
│   ├── images  # Contains all the images for teh web site
│   └── js      # Contains all the react javascript files
├── cmd         # Command line UI logic is managed in this location
├── config      # Contains the configuration logic for he application
├── daemon      # Contains the logic of the daemon that runs and controll the app
├── db          # Contains the data layet and db connectivity logic
├── doc         # Documentation folder
├── model       # Data model for the application
└── ui          # Web UI, routing, connectivity
```



### GitHub Workflow Secrets
[gitaction-config.md](git-action-config.md) - how to configure git actions (secrets used for deployment and service tasks)



### Application Architecture

![architecture](images/architecture.png)

The application itself is a React based single page application (SPA) with an API backend and a postgres database used for data persistence. It's been designed to be completely stateless and will deploy into most types of environments, be it container based or VM based.



### TechTestApp commands and configuration
Note for cloud deployment and container deployment one will need not to worry about the following nor make any changes!

update `conf.toml` with database settings (details on how to configure the application can be found in [config.md](config.md))

`TechTestApp updatedb` to create a database, tables, and seed it with test data. Use `-s` to skip creating the database and only create tabls and seed data.

`TechTestApp serve` will start serving requests

### TestTestApp Interesting endpoints

`/` - root endpoint that will load the SPA

`/api/tasks/` - api endpoint to create, read, update, and delete tasks

`/healthcheck/` - Used to validate the health of the application



