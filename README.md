
# ADF CI/CD Pipelines


1.  GitHub Integration

2.  Infrastructure

3.  Preparing for ADF Integration

4.  Deploy with Release Pipeline

CI/CD Workflow:
===============

1.  Work on new feature branch

2.  Test

    Integrate in collaboration branch.

3.  Push to qa Environment

4.  Test stg Environment

5.  Push to production Environment.

Infrastructure
==============

Thoughtfully consider a naming strategy:

Example **{bg}-{projName}-{component}-{environment}**

\-Create for each environment (dev, qa, prod)

>   1. Azure Resource Group

![](media/b6f2d4012d7ba1ba382cb1bfc8b590d4.png)

Following the suggestion, the resource group is given the name

**mm-proj1-rg-dev**

**mm-proj1-rg-qa**

**mm-proj1-rg-prod**

![](media/a363dd7f58c3ec8f888e1bf6c3da24ab.png)

![](media/20d9f55fd6fe5b7898c9d88f5bedf05d.png)

![](media/dd76188b14b0e44fadb3ead78e2ec804.png)

>   Finally Click on the Create Button

![](media/1f8e8ba3468da006f72c92927731a316.png)

>   2. Azure Data Factory

>   Create a new resource searching for ‘Data Factory’

![](media/3503d98667ad26578cd620a5c6d6faf4.png)

![](media/c7fbfa9045d710b87b7e72fc21aed8be.png)

![](media/c218e1cd763022dffa6d7a5d4249e8eb.png)

>   Provide with name following convention

>   mm-proj1-adf-dev, carefully select the correct region

![](media/1d772252aa1df63b82bb06bc1acf0fae.png)

>   Move onto the ‘Git configuraiton’

![](media/030a039d6e44b063479cd199ec607dbc.png)

![](media/a371668451ca8cfda12b51cb158c326c.png)

![](media/0f84e99bbaf9a8d4e840fc8f9319cb68.png)

![](media/d13d80b491adf53f59fd2687803cf2ec.png)

1.  Azure Key Vault

    **mm-proj1-kv-dev**

    **mm-proj1-kv-qa**

    **mm-proj1-kv-prod**

    Search for Key Vault

    ![](media/64655b822645bc66ffd6b77401041762.png)

    ![](media/4701c66c06073d5f0fa2867a1ccc3fac.png)

    ![](media/400eafe8fc17d9ad48bdfc60ac9d7072.png)

    ![](media/6e9097f2ef8ab30aad4844e1f8808e11.png)

    ![](media/fea8bc6d652fde8d5c1ab3dd1c380ae9.png)

    ![](media/f48ab7f72ab7b6b2c7d176c24d51eeff.png)

    ![](media/d625ac2da7690f926929f89220c7d719.png)

2.  Create Azure Storage Account

    mmxproj1xstordev

    mmxproj1xstorqa

    mmxproj1xstorprod

    ![](media/9b2a7f94ddf0f7fec8e8081df078c239.png)

    ![](media/f7bb6d7487df9722c1d54a699fa0697c.png)

    ![](media/489a01ab53c801e6be3c43e3e62e4ef7.png)

3.  Create Containers inside Storage Account ‘datasource’ & ‘datadest’

    ![](media/894e7f3a1ddb19ed2a9dc50b4cf733b4.png)

    ![](media/68681f832e4d6d37fc1f4edc899a09d6.png)

    ![](media/8eadeaa14f6d62c4b6bf600ce2844cec.png)

Preparing for ADF Integration
=============================

Key vault will store your sensitive information, like connection strings. Go to
storage account and copy the Access key

![](media/6822577e0811b86c4037cfe8389ae438.png)

\-Azure DevOps Project:
