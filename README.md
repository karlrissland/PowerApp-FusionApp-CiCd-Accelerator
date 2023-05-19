# PowerApp, FusionApp CiCd Accelerator
## What
This is a evolution of the PowerPlatform Actions Lab, which can be found here: https://github.com/microsoft/powerplatform-actions-lab.

This solution expands on the origional by adding the following capabilities;

- Host multiple power platform solutions within the repository
- Support fusion development by hosting non power platform solutions alongside power platform solutions
- Provide Power Automate Flow that triggers GitHub workflows when solution has been saved, published, versioned, etc.
- Provide Power App to manage Power Platform Solution to GitHub repository mapping and define trigger to kick off workflow

## Why
The origional GitHub lab doesn't demonstrate how to replace parameters or wire up connections.  Nor does it take into account additional solution types that may be part of the complete solution.

While there is a more complete ALM accelerator, it feels more appropriate for a citizen development team as opposed to a mixed pro dev/citizen dev team supported by a DevOps team.  The ALM Accelerator can be found here; https://learn.microsoft.com/en-us/power-platform/guidance/alm-accelerator/setup-admin-tasks.  The ALM accelerator also leverages Azure DevOps as the backend engine as opposed to GitHub.

If your team is mostly comprised of Citizen developers without a DevOps team, I would recomend looking at the ALM Accelerator.  If you have an established DevOps team using GitHub who is being asked to manage Low Code / No Code Power Platform solutions, this may be what you are looking for.

## How(Setup) 

### Security
You will need to setup permissions to before you get started.  This link; https://learn.microsoft.com/en-us/power-platform/alm/tutorials/github-actions-start will walk you though the initial setup.  Please make sure you copy the security GUIDs for later use.

### GitHub
To start working in your own repo, simply create your repository from this repository's template.  You can find more information here; https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template.  You are also welcome to Fork this repository.

Once you have your own repository, you will need to setup some environments and variables.

**Repository Secrets**

- Go to Settings
- Select Secrets and Variables
- Select Actions
- Click the green 'New Repository Secret' button
- The secret name is ```POWERPLATOFRMSPN```, set the service principle secret as your contents.  This is one of the pieces of information you saved when setting up security.
- Save

Note: will most likely move this to an environment secret 

**Environments and Environment Variables**
We will need three environments ```Dev, Test, Prod```.  NOTE: this is case sensitive.

- Go to Settings
- Select Environments
- click the 'New environment' button and create all three environments

For the ```Dev``` Environment, setup the following variables

---
Variable | Value 
--- | --- 
CLIENT_ID | value saved when setting up your service principle
TENANT_ID | value saved when setting up your service principle
ENVIRONMENT_URL | found in environment details from the PowerPlatform admin center.  Prefix with ```https://```
---

For the ```Test``` and ```Prod``` Environments setup the following variables

NOTE: For the GitHub Solution, you will need to setup two connections, one to the DataVerse and another to GitHub.  Once you have the connections setup, this document explains where to find the connectionId; https://learn.microsoft.com/en-us/power-platform/alm/conn-ref-env-variables-build-tools.

---
Variable | Value 
--- | --- 
CLIENT_ID | value saved when setting up your service principle
TENANT_ID | value saved when setting up your service principle
ENVIRONMENT_URL | found in environment details from the PowerPlatform admin center.  Prefix with ```https://```
CHECK_CONSISTENCY | ```True``` or ```False```
DEPLOY_MANAGED | ```True``` or ```False```
ALMLAB_SETTINGSFILE | copy the contents from ```<repo>/powerPlatSolutions/ALMLab_artifacts/ALMLab_SettingsFile.json``` Chance the Value to whatever you like.
XGITHUB_SETTINGSFILE | copy the contents from ```<repo>/powerPlatSolutions/GitHub_artifacts/GitHub_SettingsFile.json``` and update the ConnectionId
---

## Finish Up
You should be able to run the release workflow.  This will deploy the ALMLab example and the GitHub app to both the Test and Prod environments.

Before you can use the extract workflow, you need to deploy these solutions to your dev environment.  At this time, use the Power Platform CLI or simply import the solutions to your dev environment to bootstrap.  The release workflows will not deploy to the Dev environment.

## Details
You can find more details about the architecture here [GitHub Overview](docs/GitHub%20Overview.md)

## Thoughts and Future Enhancements
I was thinking of adding a manual override to allow the release to deploy to the Dev environment.

Thinking about adding a real fusion app as the second application instead of using the GitHub application.  It really is only valid to deploy the GitHub application to your dev environment... but.. it was easy to use that as another sample app.

Possibly add a switch to the export workflow that will allow you to only get the zip files and the json file.  i.e. skip extracting the PowerApp solution.  You really shouldn't be changing any of those files outside of the power platform anyway and I don't think merging is advisable.  

Currently don't handle PowerApp Solution dependencies.











