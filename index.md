# Leveraging Azure to CHEAPLY (think CENTS) manage the SDLC and hosting of a full stack web app!


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
1. Clone the repo to your computer. 
1. Install [NodeJS](https://nodejs.org/en/download/)
1. Install [Angular](https://cli.angular.io/)
1. Create an angular project `ng new full-stack-walking-skeleton`
1. Push the code to your repo! 

## Build the Angular app in AzureDevOps
1. Navigate to https://dev.azure.com
1. Pipelines
1. Create Pipeline
1. Use Classical Editor
    * the Classical Editor gives you a GUI which is much easier to work with. 
1. Select your source
    * If your Source Code is in GitHub, you will need to authorize the connection by following the prompts.
    * If your Source Code is in your AzureDevOps Repo, no further authorization is needed.
1. Choose an `Empty Build`
1. Update the name of your pipeline on the top left: `WalkingSkeletonAngular`!
    * Arbitrary
1. Add an `npm` task to **install** npm on the build agent
    * Command: `install`
    * Working folder that contains package.json: use the ellipses to navigate to the proper folder.
1. Add another 'npm' task to **build** your angular app
    * Command: `custom`
    * Working folder: the same folder
    * Command and Arguments: `run build -- --prod --outputPath="$(Build.ArtifactStagingDirectory)"` 
        * this is equivalent to `ng build --prod`, but produces the files in a different folder than dist/app-name. 
        * One can further customize the build command as needed, like specifying a configuration. 
1. Add a `Publish Pipeline Artifacts` task to **save** the static files that your Angular build produces. 
    * File or directory path: `$(Build.ArtifactStagingDirectory)`
    * Artifact name: `WalkingSkeletonAngular`
        * Arbitrary
    * Artifact publish location: `Azure Pipelines`
        * The build artifacts will be stored in the cloud.
1. Enable commits to your repo to automatically kick off a build.
    1. Go to the `Triggers` tab
    1. Check `Enable continuous integration`
    1. If your Angular code isn't stored at the root of your Repo:
        1. `Add` a `Path filter`
        1. Provide the path to your Angular source code
            * I added the `ui` `Path Filter` because my repo will have the `ui` and `api` folders, with separate builds for each compontent, and I only want to trigger a build for the Angular app if I modify files in the `ui` folder! 
1. Save & queue your build! 
1. Wait a few minutes...
1. Click `1 published` and expand the Artifact name to view the files Angular created!

Now changes to your angular app will automatically build in AzureDevOps, and store the compiled files ready to be served!
        

## Access the Azure Portal 
## Create a Storage Account
## Create the release
