# eGeoffrey CLI

The `egeoffrey-cli` utility makes a number of complex tasks trivial for an eGeoffrey user or admin. 

## Description

End users can leverage it for installing new eGeoffrey packages, starting/stopping eGeoffrey's components, searching for new packages in the marketplace, updating the software, etc.

Developers can leverage it for commiting changes to their modules or building the Docker image of their packages.

## Install

egeoffrey-cli is automatically deployed on your system under `/usr/local/bin/egeoffrey-cli` when running the eGeoffrey installer. If for any reason you need to install the utility without running the installer simply download the `egeoffrey-cli` file and make it executable.

Every time egeoffrey-cli is run, automatically checks if a newer version is available and if so advise the user to upgrade it.

## Usage

Simply run the utility with a command and a set of arguments. If no input is provided, a help screen is shown:

```
egeoffrey-cli [OPTIONS] SCOPE COMMAND ARGUMENTS
```

#### Options

The following options are allowed. Multiple options can be provided.

```
  -d <directory>                           Set the working directory
  -c <filename>                            Set path of custom docker-compose.yml filename
  -v                                       Enable DEBUG output
  -q                                       Quite mode - do not run interactively
  -o                                       Offline mode
  -f                                       Force an action
```

#### Command

egeoffrey-cli must be provided with one of the following command to execute a specific task.

HOUSE scope commands:
```
  house start [<package(s)>]               Start configured packages
  house status                             Show currently running components
  house stop                               Stop all running packages
  house reload                             Reload the configuration
  house restart [<package(s)>]             Restart all or a subset of running packages
  house stats                              Print out running packages CPU and memory utilization
  house logs [<package(s)>]                Print out log information
  house logs_tail [<package(s)>]           Print out log information and tail the log file
```

CONFIG scope commands:
```
  config setup                             Initialize the installation directory and setup eGeoffrey
  config summary                           Displays information regarding the current setup
  config modules <package_name> <modules>  Reconfigure an installed package with the provided list of modules
  config env <variable> [<value>]          Set a global environment variable
  config dump                              Displays the current raw configuration
```

PACKAGE scope commands:
```
  package install <package(s)>             Install the provided package(s)
  package uninstall <package(s)>           Uninstall the provided package(s)
  package info <package>                   Print out information on a given package
  package search <string> [branch]         Search packages matching the provided description
  package update [<package(s)>]            Update all/selected installed package(s)
  package available [<branch>]             List available packages that can be installed
  package installed                        List already installed packages
```

REPO scope commands:
```
  repo init <type> <name> <git_user>       Create a local empty repository and set a remote upstream URL
  repo commit '<description>'              Commit changes to the local and remote git repository
  repo build [<architectures>]             Build a docker image and push it upstream
  repo status                              Show the status of the repository
  repo checkout <branch_name>              Checkout into a different existing branch
  repo branch <branch_name>                Switch to a different branch
  repo version <version>                   Switch to a new version
  repo merge                               Merge the current branch into master
  repo collection                          Create a collection including the configured packages
```

SDK scope commands:
```
  sdk commit '<description>'               Commit SDK changes to the local and remote git repository
  sdk build <image> [<architectures>]      Build a SDK docker image and push it upstream
  sdk status                               Show the status of the repository
  sdk checkout <branch_name>               Checkout into a different existing branch
  sdk branch <branch_name>                 Switch SDK to a different branch
  sdk version <version>                    Switch to a new SDK version
  sdk merge                                Merge the current SDK branch into master
```

CLI scope commands:
```
  cli upgrade                              Upgrade egeoffrey-cli to the latest version
  cli version                              Print out this script version
```  

#### Arguments

Arguments to provide are dependent on the command to run and are listed just above.

## Examples 

#### End Users

- Install a new package:

  `egeoffrey-cli package install egeoffrey-notification-telegram`

- List installed packages:

  `egeoffrey-cli package installed`

- Update all the installed packages to the latest release:

  `egeoffrey-cli package update`

- Search the marketplace for a package with provides some sort of weather service:

  `egeoffrey-cli package search weather`
  
- Tail the logs of a specific package:

  `egeoffrey-cli house logs -f egeoffrey-controller`
  
- Stop all running eGeoffrey services:

  `egeoffrey-cli house stop`
  
#### Developers

- Commit changes to the Github repository configured in the manifest file after having automatically increased the revision number and generated a README.md file:

  `egeoffrey-cli repo commit "Added a new functionality"`
  
- For a package residing in a different directory, build the Docker image for a amd64 architecture and push it to the DockerHub account configured in the manifest file:

  `egeoffrey-cli -d ../egeoffrey-service-custom repo build amd64`
  
- Builds up an eGeoffrey collection by downloading the packages listed in the `collection/packages.txt` file, merging their contents and generating a manifest file using `collection/manifest.yml` as a template. Once the collection is built, it has to be commited and built like any other package:

  `egeoffrey-cli repo collection`