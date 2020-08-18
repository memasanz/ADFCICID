Azure Data Factory CI/CD Pipeline Hack
=======

Pre-Reqs:
---------
Need to have ability to create service principal.

Review:
---------
### 1. CI/CD Workflow
### 2. Setup Infrastructure
### 3. Preparing for ADF Integration (setting up key vault)
### 4. Creating a pipeline from a template
### 5. Azure DevOps Setup
### 6. Azure Data Factory Link to Azure DevOps
### 7. Deploying with a Release Pipeline


## CI/CD Workflow
--------------------------------------------
1. Work on a new feature branch
2. Test
3. Integrate in collaboration branch
4. Push to QA environment
5. Test in QA environment
6. Approve moving to production

![](media/Picture135.png)

## 2. Setup Infrastructure
--------------------------------------------


Thoughtfully consider a naming strategy

Ex: 
```
{bg}-{projName}-{component}-{environment}
```
### 2.1 Setup Resource groups

| Resource Group Names for Example: |
|--------------------------|
| mm-proj1-rg-dev          |
| mm-proj1-rg-qa           |
| mm-proj1-rg-prod         |


For each environment [dev, qa, prod] we are going to create resources.  

![](media/Picture001.png)

![](media/Picture002.png)

![](media/Picture003.png)

![](media/Picture004.png)

Finally Create the `Create` Button.

![](media/Picture005.png)

### 2.2 Azure Data Factory
--------------------------------------------

Create a new resource searching for `Data Factory`.  Technically, the pipeline would be able to create the qa and prod environments for us, but we do need to link the qa and prod environments to our key vault, so we will go ahead and create them manually to make sure we can provide them with the correct permissions.

| ADF Names for Example: |
|--------------------------|
| mm-proj1-adf-dev         |
| mm-proj1-adf-qa          |
| mm-proj1-adf-prod        |

![](media/Picture006.png)

![](media/Picture007.png)

![](media/Picture008.png)

![](media/Picture009.png)

Move onto the `Git configuration`

![](media/Picture010.png)

![](media/Picture011.png)

![](media/Picture012.png)

![](media/Picture013.png)

### 2.3 Azure Key Vault
--------------------------------------------

Lets create 3 Azure Key Vaults

| Azure Key Vaults:       |
|-------------------------|
| mm-proj1-kv-dev         |
| mm-proj1-kv-qa          |
| mm-proj1-kv-prod        |

![](media/Picture014.png)

![](media/Picture015.png)

![](media/Picture016.png)

![](media/Picture017.png)

![](media/Picture018.png)

![](media/Picture21.png)

![](media/Picture22.png)

### 2.4 Azure Storage Accounts
--------------------------------------------

| Azure Storage Account:   |
|--------------------------|
| mmxproj1xstordev         |
| mmxproj1xstordqa         |
| mmxproj1xstordprod       |

![](media/Picture23.png)

![](media/Picture24.png)

![](media/Picture25.png)

### 2.5 Azure Storage Accounts Containers
--------------------------------------------

Create containers inside storage account `datasource` & `datadest`

![](media/Picture26.png)

![](media/Picture27.png)

![](media/Picture28.png)


## 3. Preparing for ADF Integration (setting up key vault)
--------------------------------------------

We need to setup the key-vault connection string & ADF Key Vault Access.

3.1 Key vault will store your sensitive information, like connection strings.  Go to storage account and copy the Access key we will store it in a variable called ‘blob-connection’

In each key vault create a blob storage connection string.  Go to the dev instance of your storage account and grab the key from `Access keys`

![](media/Picture29.png)

![](media/Picture30.png)

![](media/Picture31.png)

![](media/Picture32.png)

![](media/Picture33.png)

In each key vault create an environment variable

![](media/Picture34.png)

3.2 Setup MSI Key Vault Access

![](media/Picture35.png)

![](media/Picture36.png)

![](media/Picture37.png)

![](media/Picture38.png)

![](media/Picture39.png)

![](media/Picture40.png)

![](media/Picture41.png)


## 4. Creating a pipeline from a template
--------------------------------------------

In the dev instance click on the `Author & Monitor` Button

![](media/Picture42.png)

![](media/Picture43.png)

![](media/Picture44.png)

![](media/Picture45.png)

![](media/Picture46.png)

Selecting a new DataSourceConnection we are able to create a new linked service.

![](media/Picture47.png)

Swap over to Azure Key Vault and we will create a linked service for Azure Key Vault as well.

![](media/Picture48.png)

![](media/Picture49.png)

After selecting the `create` button 

![](media/Picture50.png)

Let all 3 use the same linked service.

![](media/Picture51.png)

Then select `use this template`

![](media/Picture52.png)

Recall that we created storage containers in each of our environments, lets add them to the parameters inside the ADF pipeline we just created from the template

![](media/Picture53.png)

![](media/Picture54.png)

Validate & Publish
![](media/Picture55.png)

If we want, we can go to the storage explorer & view the storage accounts

![](media/Picture56.png)

![](media/Picture57.png)

Clicking debug on the pipeline

![](media/Picture58.png)

Clicking on the refresh we can see that it has completed.

![](media/Picture59.png)

![](media/Picture60.png)

#### Create a trigger

![](media/Picture61.png)

![](media/Picture62.png)

![](media/Picture63.png)

![](media/Picture64.png)

![](media/Picture65.png)

![](media/Picture66.png)

### 5. Azure DevOps Setup
--------------------------------------------

DevOps Organizations
Great resource to check out & Plan Yoour organziational structure

<https://docs.microsoft.com/en-us/azure/devops/user-guide/plan-your-azure-devops-org-structure?toc=%2Fazure%2Fdevops%2Forganizations%2Ftoc.json&bc=%2Fazure%2Fdevops%2Forganizations%2Fbreadcrumb%2Ftoc.json&view=azure-devops>

![](media/Picture67.png)

Ideally – you would add your project to an existing DevOps organization.  So let’s check what you currently have.

Create a new organization if we need to.  This would be a follow up to determine if there is a central team to work with.

![](media/Picture68.png)

![](media/Picture69.png)

![](media/Picture70.png)

Then create a project

![](media/Picture71.png)

Click on the `Create project` button

![](media/Picture72.png)

<http://dev.azure.com/>

Setup Users

Users need to be added to the organization & to the project

![](media/Picture154.png)

![](media/Picture154.png)

![](media/Picture155.png)

### 6. Azure Data Factory Link to Azure DevOps
--------------------------------------------
Let’s head back to ADF and setup the link to our newly created project.

![](media/Picture73.png)

![](media/Picture74.png)

Let’s check back at the repo.  We currently only have a master branch.

![](media/Picture75.png)

Going back into ADF we can select the ‘Publish’
On the collaboration branch ‘master’ when we publish we are generating the adf_publish branch

![](media/Picture76.png)

Going back into devOps project we can see the **adf_publish** branch, but it’s empty.  The first time we need to make an arbituary change to generate the full **adf_publish** branch.

Below is changing the description on the master branch and then publishing

![](media/Picture77.png)

After that – checking the **adf_publish(( branch will have the code.

![](media/Picture78.png)

However, it is empty.
![](media/Picture79.png)

Create a Release Pipeline
![](media/Picture80.png)

![](media/Picture81.png)

Select an Empty Job

![](media/Picture82.png)

Rename the Stage to `QA`, close it with the X and rename the `New release pipeline` to `ADF-Release-Pipeline`

![](media/Picture83.png)

![](media/Picture84.png)

Don't forget to hit the `Save` button.

![](media/Picture85.png)

![](media/Picture86.png)

We will go and create some variable groups

Create variable groups
![](media/Picture87.png)

![](media/Picture88.png)

![](media/Picture89.png)

![](media/Picture90.png)

![](media/Picture91.png)

Next we will add the variables from key vault.

![](media/Picture92.png)

Then save the variable group

![](media/Picture93.png)

Create Prod Variable Group and Authorize

![](media/Picture94.png)

#### Then Add the prod variables

![](media/Picture95.png)

![](media/Picture96.png)

![](media/Picture97.png)

Now that we have the variable groups to use in the release pipeline we will go ahead and set them up.  

Let’s go back into the releases

![](media/Picture98.png)

#### Setting up deployment Variables

Looking at the adf_publish branch you can see several variables.

![](media/Picture99.png)

`factoryName` is a pipeline variable

![](media/Picture100.png)

![](media/Picture101.png)

![](media/Picture102.png)

![](media/Picture103.png)

#### Setting up the 

![](media/Picture104.png)

![](media/Picture105.png)

Click the `Add` button

![](media/Picture106.png)

#### Linking Variable Groups

Link qa to QA Stage

![](media/Picture107.png)

![](media/Picture108.png)

![](media/Picture109.png)

![](media/Picture110.png)

![](media/Picture111.png)

Add another task

![](media/Picture112.png)

Click the `Add` Button

![](media/Picture113.png)

Select the template and template parameter files

![](media/Picture114.png)

![](media/Picture115.png)

![](media/Picture116.png)

Override the factoryName with $(factoryName)

![](media/Picture117.png)

![](media/Picture118.png)

Make sure you select **Incremental**

![](media/Picture119.png)

![](media/Picture120.png)

We need to disable and enable triggers in the adf_publish branch.  We can add the script to the adf_publish branch.

https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-deployment#script

https://github.com/memasanz/ADFCICID/blob/master/powershellscript/disabletrigger.ps1

![](media/Picture121.png)

So now we can add an Azure Power Shell task to the release pipeline

![](media/Picture122.png)

![](media/Picture123.png)

```
-armTemplate $(System.DefaultWorkingDirectory)/_mm-data-team-adf-integration/mm-proj1-adf-dev/ARMTemplateForFactory.json -ResourceGroupName mm-proj1-rg-$(Environment) -DataFactoryName $(factoryName) -predeployment $true -deleteDeployment $false
```

![](media/Picture124.png)

![](media/Picture125.png)

In the post deploy task, update the ‘predeployment’ to be false, and the ‘deleteDeployment’ to be true, it will remove unused resources from ADF (like linked services etc).

![](media/Picture126.png)

Save and clone the QA to create prod

![](media/Picture127.png)

![](media/Picture128.png)

Now that we have cloned the production we will need to manage our variable groups

![](media/Picture129.png)

![](media/Picture130.png)

Change the scope of qa to only be for QA
Then Save

Almost Done. Confirm in key vaults that for stage and prod that the service connection has proper access.


![](media/Picture131.png)

![](media/Picture132.png)

#### Setup continuous integration:


![](media/Picture133.png)

Set pre-deployment for prod & Save


![](media/Picture134.png)



#### ADF Workflow
![](media/Picture135.png)

So now let’s make some changes.
Create a branch, make a change, create a pull request, get it approved, merged and then you need to publish to see the change.
Inside dev instance of ADF let’s create a branch

![](media/Picture136.png)

![](media/Picture137.png)

![](media/Picture138.png)

![](media/Picture139.png)

![](media/Picture140.png)

![](media/Picture141.png)

![](media/Picture142.png)

Approve & Complete

![](media/Picture143.png)

![](media/Picture144.png)

Switch back to Master branch in ADF

![](media/Picture145.png)

You cna debug here

Hit the publish button

![](media/Picture146.png)

![](media/Picture147.png)

![](media/Picture148.png)

![](media/Picture149.png)

![](media/Picture150.png)

![](media/Picture151.png)

![](media/Picture152.png)












