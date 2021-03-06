[![Build status](https://ci.appveyor.com/api/projects/status/pq7kyu84crviig2q/branch/master?svg=true)](https://ci.appveyor.com/project/RamblingCookieMonster/psstash)

Stash PowerShell Module
=============

This is a PowerShell module for working with the Atlassian Stash REST API.

This is an quick and dirty implementation based on my environment's configuration, with limited functionality.  Contributions to improve this would be more than welcome!

Chances are high that there will be breaking design changes, this is just a simple POC.

### Examples:

Query for projects:

[![Public projects](/Media/Get-StashProject.png)](/Media/Get-StashProject.png)

Query for repositories:

[![Public projects](/Media/Get-StashRepo.png)](/Media/Get-StashRepo.png)

Query for project permissions:

[![Public projects](/Media/Get-StashProjectUserPermission.png)](/Media/Get-StashProjectUserPermission.png)

[![Public projects](/Media/Get-StashProjectGroupPermission.png)](/Media/Get-StashProjectGroupPermission.png)

Query for arbitrary objects:

![Objects](/Media/repos.png)



### Instructions:

```PowerShell
# One time setup
    # Download the repository
    # Unblock the zip
    # Extract the PSStash folder to a module path (e.g. $env:USERPROFILE\Documents\WindowsPowerShell\Modules\)

# Import the module.
    Import-Module PSStash    #Alternatively, Import-Module \\Path\To\PSStash

# Get commands in the module
    Get-Command -Module PSStash

# Get help for a command
    Get-Help Get-StashObject -Full
    Get-Help Get-StashConfig -Full

# Set a default Uri
    Set-StashConfig -uri https://stash.contoso.com

# List public projects
    Get-StashProject

# List repositories that user in $cred has access to
    Get-StashObject -Object repos -Credential $cred

# List repositories under the 'sysinv' project
    Get-StashObject -Object projects/sysinv/repos -Credential $cred

# Create, change, and delete objects; for example, projects
    New-StashObject -Object projects -Credential $Cred -Body @{
        key = "TSTPRJ"
        name = "Test Project"
        description = "A Project To Delete"
        avatar = "data:image/png;base64,$Base64EncodedImage"
    }

    Set-StashObject -Object projects/TSTPRJ -Credential $cred -Body @{ description = "MODIFIED DESCRIPTION!" } -Force

    Remove-StashObject -Object projects/TSTPRJ -Credential $Cred -Force

# Fork a repository into my personal project:
    New-StashFork -Credential $cred -Project SYSINV -Repo SystemsInventory

# View repositories in my personal project:
    Get-StashObject -Object projects/~MYUSERNAME/repos -Credential $Cred

# Get project user and group permissions
    Get-StashProject | Get-StashProjectUserPermission -Cred $Cred
    Get-StashProject | Get-StashProjectGroupPermission -Cred $Cred

```

### References:

* [Stash Developer Docs](https://developer.atlassian.com/stash/docs/latest/)
* [Stash Core REST API](https://developer.atlassian.com/static/rest/stash/3.9.2/stash-rest.html)

### TODO:

Everything. This is in the 'can I get it working' state. Need to identify requirements (e.g. which specific objects to create functions for, parameters for these objects, whether to pursue OAUTH) for further work, write functions, tests, etc.

Potential functions:

* [x] Get-StashProject
* [x] Get-StashRepo
* [x] New-StashFork
* [x] Get-StashProjectUserPermission
* [x] Get-StashProjectGroupPermission
* [ ] New-StashProject
* [ ] New-StashRepo
* [ ] Remove-StashRepo
* [ ] Get-StashProjectPermission
* [ ] Set-StashProjectPermission
* [ ] Add-StashProjectPermission
* [ ] Get-StashRepoPermission
* [ ] Set-StashRepoPermission
* [ ] Add-StashRepoPermission
* [ ] Remove-StashRepoPermission
* [ ] Get-StashCommit
* [ ] Merge-StashPullRequest
* [ ] Add-StashPullRequestComment
* [ ] Deny-StashPullRequest

Project Status, 1/17/2016: I no longer work with or have access to Stash / BitBucket Server. Feel free to fork this or use it as needed, but there will likely be no further development, barring external contributions.
