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
egeoffrey-cli [OPTIONS] COMMAND ARGUMENTS
```

#### Options

The following options are allowed. Multiple options can be provided.

```
  -d <directory>                      set the working directory
  -f <filename>                       set custom docker-compose.yml filename
  -v                                  enable debug output
  -q                                  quite mode - do not run interactively
  -o                                  offline mode
```

#### Command

egeoffrey-cli must be provided with one of the following command to execute a specific task.

```
  upgrade                             Upgrade egeoffrey-cli to the latest version
  version                             Print out this script version
```

```  
  info <package>                      Print out information on a given package
  install <package(s)>                Install the provided package(s)
  list_available [<branch>]           List available packages that can be installed
  list_installed                      List already installed packages
  logs [-f] [<package(s)>]            Print out log information
  reload                              Reload the configuration
  search <string> [branch]            Search packages matching the provided description
  start [<package(s)>]                Start configured services
  stats                               Print out running packages CPU and memory utilization
  status                              Show currently running components
  stop                                Stop running services
  update [<package(s)>]               Update the installed package(s)
  uninstall [<package(s)>]            Uninstall the provided package(s)
```  

```  
  commit '<description>'              Commit changes to the local and remote git repository
  build <amd64,arm>                   Build a docker image and push it upstream
  build_sdk <image> <amd64,arm>       Build a SDK docker image and push it upstream
  make_collection                     Create a collection including the configured packages
```  

#### Arguments

Arguments to provide are dependent on the command to run and are listed just above.

## Examples 

#### End Users

- Install a new package:

  `egeoffrey-cli install egeoffrey-notification-speaker`

- List installed packages:

  `egeoffrey-cli list_installed`

- Update all the installed packages to the latest release:

  `egeoffrey-cli update`

- Search the marketplace for a package with provides some sort of weather service:

  `egeoffrey-cli search weather`
  
- Tail the logs of a specific package:

  `egeoffrey-cli logs -f egeoffrey-controller`
  
- Stop all running eGeoffrey services:

  `egeoffrey-cli stop`
  
#### Developers

- Commit changes to the Github repository configured in the manifest file after having automatically increased the revision number and generated a README.md file:

  `egeoffrey-cli commit "Added a new functionality"`
  
- For a package residing in a different directory, build the Docker image for a amd64 architecture and push it to the DockerHub account configured in the manifest file:

  `egeoffrey-cli -d ../egeoffrey-service-custom build amd64`
  
- Builds up an eGeoffrey collection by downloading the packages listed in the `collection/packages.txt` file, merging their contents and generating a manifest file using `collection/manifest.yml` as a template. Once the collection is built, it has to be commited and built like any other package:

  `egeoffrey-cli make_collection`