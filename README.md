# Assessment

## The application
This repository contains a frontend and a backend service. These services together serve as a ToDo List App.
Read the below documentation for details about each service.

[FrontEnd Readme](Frontend/README.md)

[Backend Readme](Backend/TodoList.Api/README.md)

## Pre Requisites
As this assessement came from a Github repo and had an exisiting docker-compose file for testing then in the spirit of not over engineering a solution the prequisite for the CI/CD solution are a Github account, Docker Desktop installed locally (only Docker desktop has docker compose has ECS intergration :( )  for local testing, and to have a simple cloud solution to also have credentials for a valid AWS account.
This proposed solution uses Github Actions to test the apps whilst under the develop branch. Once a pull request has been raised to merge the develop branch to main then docker-compose AWS ECS integration will be used again with github actions to deploy/re-deploy the application.

## Highlevel overview
All development is done under the develop branch of this repo. When you want to run the tests for the Frontend or Backend apps then _tag the commit_ with a semantic version number and the CI/CD flow will run the tests. When you want to deploy to the cloud then run a pull requests and when it is merged to main the Github action will deploy to AWS.
The solution uses basic AWS ECS as it is a basic app and Kubernetes would be an overkill.
we can get scale out by having multiple forntends behind an AWS load-balancer. 

## Process Instructions

### Local docker devlopment and testing
- clone the repo
- checkout the develop branch `git checkout -b develop` 
- code your changes.
- if you have local docker development environment then you can run the code locally via `docker compose up --detach`
- if you wish to run the dotnet or npm test then you can tag the devlopment branch with a semantic tag aka '1.0.1' `git tag 1.0.1` and then push to origin `git push origin --tags`
- when you want to deploy to the cloud raise a pull-request to merge to main

### Cloud deploy privileges from main
You will need an AWS account with IAM privileges 
- create docker ecs aws context `docker context create ecs ecs-context` (if you have Docker Desktop or linux version of docker)
- store your AWS credentials ( `AWS_ACCESS_KEY_ID` & `AWS_SECRET_ACCESS_KEY` and `AWS_REGION` ) in `$HOME/.aws/credentials`.
- Store your aws config (aka REgion ANd ECR Repository) in `$HOME/.aws/config` (`AWS_REGION` `ECR_REPOSITORY`)
- once pull-request is approved and merged it will be deployed to the cloud with the AWS docker-compose 

## Additional Comments

### Testing

I used act to test the github actions locally. As you can see there was a TDD "error" so my test flows wont complete. I didnt fix the TDD "error" as thought was aprt of process?

#+begin_src bash
[agasson@TRex]> act -s GITHUB_TOKEN="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" -j "test-frontend"
[Docker Compose Development CI/Test Frontend application]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /var/run/act/workflow/4] user= workdir=Frontend
| 
| > todolist.app@0.1.0 test
| > react-scripts test --env=jsdom --verbose
| 
|  PASS  src/__test__/AddTodoItem.test.tsx
|   ✓ renders the Add todo input element (56 ms)
|   ✓ renders the Add todo and clear button (156 ms)
|   ✓ Clicking on Add Item button should call handleAdd with entered todo description (106 ms)
|   ✓ Clicking on Clear button should clear the value from the add todo input (44 ms)
| 
|  PASS  src/__test__/TodoItemList.test.tsx
|   ✓ renders the todo items (54 ms)
|   ✓ the todo item count is correctly shown (24 ms)
|   ✓ refresh button should call the getItems prop (169 ms)
|   ✓ Mark as completed button should call the handleMarkAsComplete prop (74 ms)
| 
|  FAIL  src/__test__/AppInstructions.test.tsx
|   ✕ renders the technical test instructions (32 ms)
| 
|   ● renders the technical test instructions
| 
|     TestingLibraryElementError: Unable to find an element with the text: /Welcome to the ClearPoint frontend technical test/i. This could be because the text is broken up by multiple elements. In this case, you can provide a function for your text matcher to make your matcher more flexible.
| 
|     Ignored nodes: comments, <script />, <style />
|     <body>
|       <div>
|         <div
|           class="row"
|         >
|           <div
|             class="col"
|           >
|             <div
|               class="fade alert alert-success show"
|               role="alert"
|             >
|               <div
|                 class="alert-heading h4"
|               >
|                 Todo List App
|               </div>
|               Welcome to the ClearPoint SRE technical test.
|               <br />
|               <br />
|             </div>
|           </div>
|         </div>
|       </div>
|     </body>
| 
|        8 |   // Act
|        9 |   const header = screen.getByText(/Todo List App/i);
|     > 10 |   const instructionHeading = screen.getByText(/Welcome to the ClearPoint frontend technical test/i);
|          |                                     ^
|       11 |
|       12 |   // Assert
|       13 |   expect(header).toBeInTheDocument();
| 
|       at Object.getElementError (node_modules/@testing-library/dom/dist/config.js:38:19)
|       at node_modules/@testing-library/dom/dist/query-helpers.js:90:38
|       at node_modules/@testing-library/dom/dist/query-helpers.js:62:17
|       at getByText (node_modules/@testing-library/dom/dist/query-helpers.js:111:19)
|       at Object.<anonymous> (src/__test__/AppInstructions.test.tsx:10:37)
| 
Test Suites: 1 failed, 2 passed, 3 total
| Tests:       1 failed, 8 passed, 9 total
| Snapshots:   0 total
| Time:        4.542 s
| Ran all test suites.
[Docker Compose Development CI/Test Frontend application]   ❌  Failure - Main 🔍 run tests


#+end_src





