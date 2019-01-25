---
title: "Azure CLI Alias extensions"
layout: single
date: 2019/01/22
categories: 
    - Azure
tags:
    - azure
    - cloud
    - tools
    - cli
---

Azure CLI is a powerful cross-platform command line tool for managing Azure resources, this has become my go-to command line utility for doing so. It’s written using Python and has a simple standardized command pattern of “az {resourceType} {command} {options}”.

Azure CLI also packs another powerful feature called “extensions” which allow users to experiment with gems which are still unreleased or in the preview stage. One such gem is “Alias”, it allows you to customize the entire command and rename/shorten them for your needs.

I have a dev-VM setup in the Azure cloud (for obvious corporate policy reasons ;-)), now every day I would have to start the VM and stop the VM for saving pennies and also ensure that I get two times bigger VM for the same cost. 

I had run the following command to start the VM
```bash
az vm start -n <<name>> -g <<group>>
```
and to stop the VM
```bash
az vm stop -n <<name>> -g <<group>>
```
Now just by using the following simple commands I have customized starting and stopping the commands to simple and intuitive name

```bash
az alias create –name devvmstart ‘vm start -n <<vmName>> -g <<rgName>>’
az alias create –name devvmstop ‘vm stop-n <<vmName>> -g <<rgName>>’
```
After executing these two commands now I can just use ‘az devvmstart’ and ‘az devvmstop’, which makes my life a lot simpler (sigh!!!).

### References
[Azure CLI Documentation](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)

[Alias Extension](https://docs.microsoft.com/en-us/cli/azure/ext/alias/alias?view=azure-cli-latest)

Happy Coding!!!