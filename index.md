# Leveraging Azure to CHEAPLY (think CENTS) manage the SDLC and hosting of a full stack web app!


SDLC Component | Tool         | Hosting
-------------- | -------------|--------
Source Control | Git | GitHub
Front End | Angular | Azure Static Website on a Storage Account
Back End | .NET Core | Azure Function App
CICD Pipeline | AzureDevOps | Azure
Persistence Layer | NoSQL | Azure Storage Account Tables API

## Setting up AzureDevOps 
The one stop dev shop!

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
1. Navigate to https://portal.azure.com
1. If necessary, start an Azure Free Trial - $200 for a year goes a LONG way!
    * You DO have to provide a credit card, but there is 0 risk of getting charged. If you run out of credits or after a year, your resource will simply be shut off unless you change your payment plan. 
1. You should be logged in and and at the home page.

## Create a Storage Account

The Storage Account is an Azure product ("resource") that includes several great features.
* A simple file system.
    * This is where the static Angular files will live.
    * The files for the Serverless Azure Functions are stored here too.
* Tables API - A NoSQL database.
    * This is the cheapest persistence layer provided by Azure. 
* Simple Queuing functionality.
    * Azure also provides a more advanced Service Bus product.

1. In the Azure Portal, search for `Storage Account`
1. Add 
1. Create a new **Resource Group**
    * Typically this is the grouping of all the components of one overall application, for one environment. 
    * Example: skeletonapp-dev
    * If you just activated a free trial and get an insufficient permissions error, re-log and try again!
1. **Name** your storage account
    * This name becomes part of the URL used to access your storage account. We will create an Azure Function to serve the Angular App, though, so clients won't see this url. 
    * Example: stskeletonappdev
        * `st` is just Microsoft's recommended prefix. /shrug. 
1. Specify the **Location**
    * This location should be consistent accross all the resources you make. 
    * This hosts your application in the specified region - the further away a client is, the higher latency there will be. For our purposes this is negligible.
1. All other fields can be left as defaults. 
    * You can specify a **Cool** access tier to save more money, but even with **Hot**, it's CHEAP! 
1. **Review + Create**
    * You can tab through the different pages instead of Review + Create if desired, but the defaults work perfectly fine.
1. Hopefully your validation passes.
1. **Create** your Storage Account!
    * And your resource group..
   
After a few seconds, you have a Storage Account!

## Set up your Storage Account to host your Angular code
By default, the Storage Account will require authentication for accessing the various files or features within. By using the **Static Website** feature of the Storage Account, we can serve the static files without requiring authentication. 

1. Navigate to your Storage Accounnt
1. Navigate to the **Static Website** option on the left pane.
1. Toggle the Static website to **Enabled**
1. Set the **Index document name** to **index.html** 
    * This is what Angular Typically compiles down to - an index file, a css file, and a bunch of javascript files.
1. Set the **Error document path** to **404.html**
    * You actually don't need to have this file, but it doesn't hurt.
1. **Save**
1. You should now have the **$web** container, and an endpoint for that container. 
    * You can Navigate to the **$web** container and upload files through the Azure Portal UI and access those files through the endpoint.
    * We'll store the Angular files in this container.
    
Now we have the infrastructure created to host and serve your Angular code!

## Create the release
