SDLC Component | Tool         | Hosting
-------------- | -------------|--------
Source Control | Git | GitHub
Front End | Angular | Azure Static Website on a Storage Account
Back End | .NET Core | Azure Function App
CICD Pipeline | AzureDevOps | Azure

## Setting up AzureDevOps 
The one stop shop for a dev shop!

Navigate to https://dev.azure.com and follow the prompts. 
* Choose **Free With Github** to re-use your GitHub credentials.
* Choose **Free** to use your MSDN account or to create a new one. 
* If you already have an account and have multiple organizations, choose an organization. 

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
