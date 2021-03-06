---
title: Use Template Validator to check templates for Azure Stack | Microsoft Docs
description: Check templates for deployment to Azure Stack
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/1/2016
ms.author: helaw
translationtype: Human Translation
ms.sourcegitcommit: 5145869c8fac90a9415f658bf541ba95ac33c213
ms.openlocfilehash: b0d60bc1c66002614179b828efd5d1ae0214b439
ms.lasthandoff: 12/16/2016


---

# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Check your templates for Azure Stack with Template Validator
You can use the template validation tool to check if your Azure Resource Manager [templates](azure-stack-arm-templates.md) work with Azure Stack.  The tool consists of two PowerShell modules and a JSON file:

 - AzureRM.CloudCapabilities.psm1 creates a cloud capabilities JSON file representing the services and versions in a cloud like Azure Stack.
 - AzureRM.TemplateValidator.psm1 uses a cloud capabilities JSON file to test templates for deployment in Azure Stack.
 - AzureStackCapabilities_TP2.json is a default cloud capabilities file.  You can create your own, or use this file to get started. 

In this topic, you download the tool, run validation against your templates, and optionally build a cloud capabilities file.

## <a name="download-the-tool"></a>Download the tool
The Azure Stack template validation files are hosted in [Azure Stack Tools](https://github.com/Azure/AzureStack-Tools).  To retrieve the files, run the following script in an administrative PowerShell ISE session:

```PowerShell
# Variables
$Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/'
$LocalPath = 'c:\AzureRMTemplateValidator'

# Create folder
New-Item $LocalPath -type directory

# Download Template Validator files
( 'AzureStackCapabilities_TP2.json', 'AzureRM.TemplateValidator.psm1') |`
foreach { Invoke-WebRequest ($uri + 'TemplateValidator/' +$_) -OutFile ($LocalPath + '\' + $_) }

# Download Template Validator files
Invoke-WebRequest ($uri + '/CloudCapabilities/AzureRM.CloudCapabilities.psm1')`
-OutFile ($LocalPath + '\' + 'AzureRM.CloudCapabilities.psm1') 
```

## <a name="validate-templates"></a>Validate templates
In these steps, you validate templates using the AzureRM.TemplateValidator PowerShell module.  

1.  Prepare to run the tool by copying your templates to a local path.  You can use your own templates, or you can validate against the [Azure Stack Quickstart templates](https://github.com/Azure/AzureStack-QuickStart-Templates).  
2.  Import the AzureRM.TemplateValidator.psm1 PowerShell module:
    
    ```PowerShell
    import-module c:\AzureRMTemplateValidator\AzureRM.TemplateValidator.psm1
    ```

2.  Run the template validator:

    ```PowerShell
    test-azureRMTemplate -TemplatePath <path to template.json or template folder>`
    -CapabilitiesPath <path to cloudcapabilities.json>`
    -Verbose
    ```

Any template validation warnings or errors are logged to the PowerShell console, and are also logged to an HTML file in the source directory.  

### <a name="parameters"></a>Parameters

| Parameter | Description | Required |
| ----- | -----| ----- |
| TemplatePath | Specifies the path to recursively find Resource Manager templates | Yes | 
| TemplatePattern | Specifies the name of template files to match. | No |
| CapabilitiesPath | Specifies the path to cloud capabilities JSON file | Yes | 
| IncludeComputeCapabilities | Includes evaluation of IaaS resources like VM Sizes and VM Extensions | No |
| Report | Specifies name of the generated HTML report | No |
| Verbose | Logs errors and warnings to the console | No|


### <a name="examples"></a>Examples
This example validates all the Azure Stack Quickstart templates downloaded locally, and also validates the VM sizes and extensions against Azure Stack TP2 capabilities.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates`
-CapabilitiesPath C:\AzureTemplateValidator\AzureStackCapabilities_TP2.json`
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="build-cloud-capabilities-file"></a>Build cloud capabilities file
The downloaded files include a default AzureStackCapabilities_TP2.json file, which describes the service versions available in a default installation of Azure Stack TP2.  As you install additional Resource Providers, you can use the AzureRM.CloudCapabilities PowerShell module to build a JSON file including the new services.  

1.  Make sure you have connectivity to Azure Stack.  These steps can be performed from [MAS-CON01](azure-stack-connect-azure-stack.md#connect-with-remote-desktop), or you can use [VPN](azure-stack-connect-azure-stack.md#connect-with-vpn) to connect from your workstation. 
2.  Import the AzureRM.CloudCapabilities PowerShell module:

    ```PowerShell
    import-module c:\AzureRMTemplateValidator\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Use the Get-CloudCapabilities cmdlet to retrieve service versions and create a cloud capabilities JSON file:

    ```PowerShell
    Get-AzureRMCloudCapabilities -Location 'local' -Verbose
    ```             


## <a name="next-steps"></a>Next steps
 - [Deploy templates to Azure Stack](azure-stack-arm-templates.md)
 - [Develop templates for Azure Stack] (azure-stack-develop-templates.md)


