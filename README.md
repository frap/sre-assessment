# Assessment

## The application
This repository contains a frontend and a backend service. These services together serve as a ToDo List App.
Read the below documentation for details about each service.

[FrontEnd Readme](Frontend/README.md)

[Backend Readme](Backend/TodoList.Api/README.md)

## Pre Requisites
this CI/CD solution used Github Actions to test the apps and then provison an AWS ECS network to run the application. Therefore you will need your AWS credentials to be added
as secrets in the gituhub Actions

## Highlevel overview
All development is done under the develop branch of this repo. When you want to run the tests for the Frontend or Backend apps then tag the commit with a semantic version number and the CI/CD flow will run the tests. When you want to deploy to the cloud then run a pull requests and when it is merged to main the Github action will deploy to AWS.
The solution uses basic AWS ECS as it is a basic app and Kubernetes would be an overkill.
we can get scale out by having multiple forntends behind an AWS load-balancer. 

## Process Instructions
- clone the repo
- checkout the develop branch `git checkout -b develop` 
- code your changes.
- if you have local docker development environment then you can run the code locally via `docker compose up --detach`
- if you wish to run the dotnet or npm test then you can tag the devlopment branch with a semantic tag aka '1.0.1' `git tag 1.0.1` and then push to origin `git push origin --tags`
- when you want to deploy to the cloud raise a pull-request
- once pull-request is approved and merged it will be deployed to the cloud


Candidates should provide documentation on their solution, including:

* Pre requisites for your deployment solution.
* High level architectural overview of your deployment.
* Process instructions for provisioning your solution.



## Assessment Grading Criteria

##### Key Criteria

The submission should the following criteria:

* Must be able to start from a cloned git repo.
* Must document any pre-requisites clearly.
* Must deploy infrastructure using code.
* Must deploy to a cloud account/subscription.

##### Grading

Candidates will be assessed across the following categories:

##### General Approach

* Clarity of code
* Comments where relevant
* Consistency of Coding

#### Security

* Least Privilege
* Network segmentation (if applicable to the implementation)
* Secret storage (if applicable)
* Platform security features

#### Simplicity

* Do not overengineer the solution

#### Resiliency

* Infrastructure should support Auto scaling and the application should be highly available

#### Bonus points

* Deploy via an automated CICD process.
