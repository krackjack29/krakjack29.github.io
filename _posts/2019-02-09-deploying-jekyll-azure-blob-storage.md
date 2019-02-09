---
title: 'Deploying Jekyll Websites on Azure Blob Storage'
layout: single
comments: true
date : 2019/02/09
categories:
    - Azure
tags:
    - azure
    - cloud
    - blogs
    - jekyll
    - blob storage
---

Azure Storage Accounts (GPV2 sku), allows users to host static websites using the blob storage. This feature was recently GA'ed. [Jekyll](https://jekyllrb.com/) is a static website generator using simple markdown files, it powers the current website you are reading as well.

In this article, we would see how to create a new Jekyll website and host it using Azure blob storage. 

## Create a new Jekyll website

I am using Windows Subsystem for Linux (WSL) on windows 10 for local development. Follow the detailed instructions on the [Jekyll official documentation](https://jekyllrb.com/docs/installation/windows/) to install Jekyll and the prerequisites. 

I had issues while installing **nokogiri** extension on ruby, which was resolved by installing it manually by running the following command

```bash
sudo apt-get install build-essential patch ruby-dev zlib1g-dev liblzma-dev
gem install nokogiri
```

Once everything is setup, you can create a new website using the following command, this would create basic infrastucture needed. You can add new blogs in the **'_posts'** folder. 

```bash
jekyll new my-sample-blog
```

You can run the website locally using the following command
```bash
cd my-sample-blog
bundle exec jekyll serve
```
This would build the jekyll site and serve at http://localhost:4000.

## Deploying the website to Azure Blob storage

```bash
bundle exec jekyll build
```
above command will build the Jekyll website, which literally transforms all the markdown files you have into static HTML pages and move them all into a folder named **_site**. 

![Site Directory](/assets/images/blobstatic/dir.png)

These are the contents that need to be hosted, in this case we will need to move this folder to Azure blob storage. I have preferred to use Azure CLI in this case, can be done using either Powershell or Portal.

0. Prerequisites
   ```bash
   ACCOUNT_NAME='samplejekyll0209'
   SUBID='<<SubscriptionId>>'
   LOCATION='southindia'
   RESOURCEGROUP='samplejekyll'
   ```

1. Login to Azure CLI
   ```bash
   az login
   az account set --subscription $SUBID #Only if you are part of multiple subscription
   az create group --name $RESOURCEGROUP --location $LOCATION
   ```

2. Create an Azure storage account
    ```bash
    az storage account create --name $ACCOUNT_NAME --resource-group $RESOURCEGROUP --kind StorageV2 
    ```
    **Kind has to StorageV2, only GPV2 storage accounts support static website**

3. Install Storage Preview extension
   ```bash
   az extension add --name storage-preview
   ```
   This extension needs Azure CLI to be minimum version of 2.0.52, upgrade your CLI if anything less than that.

4. Update Service properties to enable static website
   ```bash
   az storage blob service-properties update --account-name $ACCOUNT_NAME \ 
    --static-website --404-document '404.html' --index-document 'index.html'
   ```
   Above command would enable the static site and create a container by name $web, you can verify by logging into to the portal.
   ![Static Website](/assets/images/blobstatic/staticweb.png)
   By default Jekyll has the entrypoint to index.html and not found page as 404.html in the _site folder.

5. Upload the contents of the _Site folder to $web
   ```bash
   az storage blob upload-batch -s '_Site' -d \$web --account-name $ACCOUNT_NAME
   ```
   This would update all the folders and files (recursively) within the _Site folder.

6. Verify the deployed site
   ![Home](/assets/images/blobstatic/home.png)
   In the portal one could verify the endpoint configured for the static website, and browse to that endpoint. If you have deployed storage account as RAGRS (default), then you would also have a default secondary url.

Now that you have successfully uploaded the site you could map the endpoints of the storage account to a [custom domain](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website). 

## Things to consider

### Pricing
Even though Azure Storage account is cheap to store data and read within datacenters, you have to be careful about the Egress bandwidth considered. First 5GB is free per month, which is a lot, but if you have a website which shares images and other large static content, you might have to think about the charges.

### Automation
Since this entire process of writing a blog till uploading to the contents to the container, one could automate this process using any CI/CD tools such as Jenkins, AzureDevOps etc. 

### Persistence
In the step 6 of the upload command, it creates/updates the blobs in the $web container but wouldn't delete the unreferenced ones yet. I would recommend running the following command each time before step 6.
```bash
az storage blob delete-batch -s '_Site' -d \$web --account-name $ACCOUNT_NAME
```

Happy Coding!!!!