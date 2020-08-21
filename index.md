SDLC Component | Tool         | Hosting
-------------- | -------------|--------
Source Control | Git | GitHub
Front End | Angular | Azure Static Website on a Storage Account
Back End | .NET Core | Azure Function App
CICD Pipeline | AzureDevOps | Azure
Persistence Layer | NoSQL | Azure Storage Account Tables API

## Setting up AzureDevOps 
The one stop shop for a dev shop!

Navigate to https://dev.azure.com and follow the prompts. 
* Choose **Free With Github** to re-use your GitHub credentials.
* Choose **Free** to use your MSDN account or to create a new one. 
* If you already have an account, sign in and choose you organization.

Voila, you have an AzureDevOps instance in the cloud! This provides you with:
* A place to host repos, in case you didn't want to use GitHub.
* Automated Builds & Releases for deploying your code.
    * AzureDevOps has many different tasks and build agent operating systems to build and deploy many types of applications, to many types of hosting environments.
* Agile tooling to organize yourself and teams.
* Testplans to organize and automate testing.
* The ability to share the instance with collaborators. 
* and Dashboards to visualize it all!

Allotted Parallel Jobs | Allotted CPU time | Cost
-----------------------|-------------------|-----
1 | 30 hours/month | Free
x | unlimited | x * $40/month

30 hours/month of build & release CPU time is PLENTY for an individual or small teams. 


## Push some Angular code to Source Control 
1. Create your repo
    * To use AzureDevOps Repos, go to the **Repos** tab in your AzureDevOps instance. Either create a new repo or use the auto generated one.
    * To use GitHub... go to GitHub!
2. Clone the repo to your computer. 
3. Install [NodeJS](https://nodejs.org/en/download/)
4. Install [Angular](https://cli.angular.io/)
5. Create an angular project `ng new full-stack-walking-skeleton`
6. Push the code to your repo! 

## Access the Azure Portal 
## Create a Storage Account
