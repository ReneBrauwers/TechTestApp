# GIT ACTION CONFIGURATION
[![Build Status][git-cicd-badge]][gitworkflow]
[![Release][release-badge]][release]

[git-cicd-badge]: https://github.com/ReneBrauwers/TechTestApp/workflows/CICD/badge.svg
[gitworkflow]: https://github.com/ReneBrauwers/TechTestApp/actions?query=workflow%3ACICD
[release-badge]: http://img.shields.io/github/release/ReneBrauwers/TechTestApp/all.svg?style=flat
[release]:https://github.com/ReneBrauwers/TechTestApp/releases

## Introduction
In order for the CICD workflow to function it depends on certain environment variables which contain sensitive information, such as passwords and usernames. 
These environment variables have been stored as Secrets in the GitHub repo and in order for this repo and workflow to work once cloned, one will need to make 
sure that the following secrets are stored.

[Read more information about Github secrets](https://developer.github.com/v3/actions/secrets/)

| Secret Name  | Description | 
|---|---|
| RUN_PROJECT  | Google Cloud Project Name, containing the cloud services to use | 
|  RUN_SA_KEY |A [JSON service account key](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)   | 
|  RUN_SQL_HOST| PRIVATE IP Address from Cloud SQL for Postgres instance   |
| RUN_SQL_DB  | Database name to use to  | 
|  RUN_SQL_PORT | Database port to use; most likely 5432  | 
|  RUN_SQL_USER | DB User created  |
|  RUN_SQL_PASSWORD |  DB Password created |
