---
title: 'Must have VSCode extensions for Cloud Developers'
layout: single
comments: true
date : 2019/09/27
categories:
    - Tech
tags:
    - azure
    - cloud
    - tools
    - vscode
---

Visual studio code is the most preferred IDE amongst the frontend developers, its opensource and supports a wide variety of plugins. In this blog I will list down all plugins that I regularly use for Cloud Development. These extensions make your development process as smooth and productive as possible.

## Productivity Extensions

* [Remote WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) I use windows subsystem for linux to do a lot of operations involving python, docker and kubernetes. This plugin helps you to run VSCode directly from the bash.

* [Prettier-Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) Code formatting support for most of file types. All you need to do is press F1 and then 'Format Document'

* [Gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) Shows line by line history if needed, as the description says a supercharged client for Git.

* [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare) You can share your coding session with your peers and have live reviews, very useful when you are working in a team consisting remote members.

* [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) Keeps your settings and extensions in sync between different environments.

* [Rest Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) Another gem of an extension I have been an avid user of this extension to execute the REST Apis, works so well with Azure REST Apis.

* [Sort Lines](https://marketplace.visualstudio.com/items?itemName=Tyriar.sort-lines) Handy extension to sort lines alphabetically, really useful when you have a long list of configuration stored.

* [TODO Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree) Shows the TODOs in your workspace in a tree view.

## Programming Language support

* [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) My go to language for application development including but not limited to Azure functions, Web applications.

* [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) As popular as VSCode if not more, is Python language. Its used in all aspects of cloud computing nowadays including AI and Machine learning.

* [Go](https://marketplace.visualstudio.com/items?itemName=ms-vscode.Go) Another popular language to write containers and cloud native applications.

* [Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) Preferred scripting language for windows automation.

* [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) Intellisense, syntax highlighting, linting of Dockerfile and commands.

* [Yaml](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) Linting for Yaml files.

* [Kubernetes](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools) Extension to manage, create and deploy Kubernetes either on prem or Azure

* [Terraform](https://marketplace.visualstudio.com/items?itemName=mauve.terraform) Terraform is IaC and IaC is terraform. This extension provides intellisense, syntax highlighting and commands to create terraform templates.

## Azure Extensions

Extensions Pack in VS Code is a group of extensions that work well with each other. Microsoft has packaged such necessary extensions for Azure developers.

* [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) Comprises necessary tools for website hosting, sql deployment etc. Installs the following tools
    * [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
    * [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
    * [Azure Storage](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) Manage storage accounts, file shares directly from Visual studio code.
    * [Azure CosmosDb](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
    * [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli) : Intellisense and auto complete for Azure CLI.
    * [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools): Intellisense and visualization support for ARM templates.
    * [Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-azure-devops.azure-pipelines) Intellisense for Azure Deveops Yaml files.
    * [Azure API Management](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-apimanagement)

* [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Installs the following extensions used to manage and program IoT using Azure.
    * [Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) Interact, manage with Azure IoT Hub
    * [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
    * [Azure IoT Device Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench)
* [ARM Template Viewer](https://marketplace.visualstudio.com/items?itemName=bencoleman.armview) Displays a graphical view of ARM Templates, helps you in documenting the resources deployed.

## Markdown Extensions

I use [Jekyll](https://jekyllrb.com/) to write the blogs, and I had blogged about it earlier and you can read about [here](https://www.cloudmanav.com/blogs/wordpress-to-jekyll/). Jekyll is built for markdown files and you know what VSCode is my blog editor too, and the following extensions makes it happen.

* [Markdown Preview Enhanced](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced) Provides a side by side preview of the markdown file.

* [Markdown all in one](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) Markdown linting, shortcuts to adding TOC, tables, ordered lists etc.

* [Docs Authoring Pack](https://marketplace.visualstudio.com/items?itemName=docsmsft.docs-authoring-pack) Another good extension pack from Microsoft, but sometimes annoying with its warning. It contains a spell checker, Markdown lint and other shortcuts.

## Themes and Other UI Utilities

* [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons) Icons for every filetype.

* [bracket pair colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer) Different colors for pair of brackets, good visualization when you are writing a large json or an ARM template.

* [Night Owl](https://marketplace.visualstudio.com/items?itemName=sdras.night-owl) A Theme for a Night OWL :-D

* [Code Blue](https://marketplace.visualstudio.com/items?itemName=Sujan.code-blue) An eye soothing blue theme. I keeping switching between the previous and this theme every now and then.

Happy Coding!!!
