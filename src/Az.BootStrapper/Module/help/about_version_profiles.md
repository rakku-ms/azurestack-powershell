﻿# Version Profiles
## about_version_profiles  

# SHORT DESCRIPTION
Version profiles provide a mechanism for managing powershell cmdlets that target 
specific versions of azure services supported in different instances of Azure.

# LONG DESCRIPTION
Different concrete instances of Azure (AzureCloud, AzureChinaCloud, 
AzureGermanCloud, AzureUSGovernmentCloud, AzureStack) may have different 
versions of Azure services installed, with different capabilities. Azure 
Version Profiles provide a mechanism for managing these version differences.  
Each Azure instance has a discoverable set of supported version profiles.  
A user can select a version profile supported by the instances of Azure they 
target, and this version profile corresponds to versions of the Azure PowerShell 
modules. Users can then select these Azure PowerShell module versions and be 
confident that their scripts will work when targeting those Azure instances.

The Az.Bootstrapper module provides cmdlets to discover, acquire, and use 
modules that are appropriate for the azure version profile you are targeting.

You can also use Tags in the Az modules to discover profile information 
for each module version.  

Tags for a Profile use the form VersionProfile:2019-03-01-hybrid

The Az bootstrapper module uses the PowerShell Gallery to install and 
load needed modules when you want to target a specific version profile.

# EXAMPLES

## Finding appropriate version profiles

Use the Get-AzApiProfile cmdlet to discover available profile versions, 
and profile versions supported by an Azure instance.

```
Get-AzApiProfile -ListAvailable 
```

lists all available version profiles.

```
Use-AzProfile -Profile 2019-03-01-hybrid
``` 

Installs and loads cmdlets for one of the listed profiles.

## Targeting all Azure Instances

```
Get-AzApiProfile
``` 

Lists the profiles that are currently installed on the machine.

```
Use-AzProfile -Profile 2019-03-01-hybrid
``` 
Installs and loads cmdlets compatible with one of the listed profiles.

## Targeting the Latest Stable Features

```
Use-AzProfile -Profile Latest
```
Installs and loads the latest published cmdlets for Azure PowerShell.

## Acquiring and Loading All Azure modules using the BootStrapper

```
Use-AzProfile -Profile '2019-03-01-hybrid' -Force
```

Checks if  modules compatible with the '2019-03-01-hybrid' profile are 
installed in the current scope, downloads and installs the modules if necessary, 
and then loads the modules in the current session.  You must open a new 
PowerShell session to target a different version profile.  Using the 
'Force' parameter installs the necessary modules without prompting.

## Acquiring and Loading Selected Azure modules using the Bootstrapper

```
Use-AzProfile -Profile '2019-03-01-hybrid' -Module Az.Compute
```

Checks if an Az.Compute module compatible with the 
'2019-03-01-hybrid' profile is installed in the current scope, downloads 
and installs the module if necessary, and then loads the module in the 
current session.  You must open a new PowerShell session to target a different 
module.

## Switching Between Version Profiles

To switch between version profiles on a machine, in a new PowerShell window, 
execute the following cmdlet:

```
Use-AzProfile -Profile '2019-03-01-hybrid'
```

This loads the modules associated with the '2019-03-01-hybrid' profile in 
the current session.  You must open a new PowerShell session to target a 
different version profile.  

## Updating and Removing Profiles

To update a profile to the latest versions in that profile and import updated 
modules to the current session, execute the following cmdlet:

```
Update-AzProfile -Profile 'latest'
```

This checks if the latest versions of Azure PowerShell modules are 
installed, if not prompts the user if they should be installed and imports 
them into the current session. This should always be executed in a new 
PowerShell session.

If you would like to update to the latest modules in a Profile and remove 
previously installed versions of the modules, use:

```
Update-AzProfile -Profile 'latest' -RemovePreviousVersions
```

## Setting and Removing Default Profiles

To set or update a profile as a default to be used with all Azure PowerShell 
modules, execute the following cmdlet:

```
Set-AzDefaultProfile -Profile '2019-03-01-hybrid'
```
The default profile selection is persisted across shells and sessions.

After default profile is set using the above cmdlet,  the 'Import-Module'
when used with Az modules will automatically load Azure PowerShell 
modules compatible with the given profile. You may also use API version 
profile cmdlets without the '-profile' parameter.

```
Import-Module Az.Compute
Use-AzProfile
Uninstall-AzProfile
```

To remove a default profile from all sessions and shells, execute the following
 cmdlet:

```
Remove-AzDefaultProfile 
```
