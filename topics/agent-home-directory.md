[//]: # (title: Agent Home Directory)
[//]: # (auxiliary-id: Agent Home Directory)

The _Build Agent Home Directory_ is the directory where the agent is installed.

The [Build Agent](build-agent.md) can be installed into any directory. If you use the TeamCity [.tar.gz distribution or .exe distribution](installing-and-configuring-the-teamcity-server.md#Installing+TeamCity+Server) opting to install a Build Agent, the agent will be placed into `<`[`TeamCity Home`](teamcity-home-directory.md)`>/buildAgent`. The default directory suggested by the .exe agent installation is `C:\BuildAgent`.

The agent stores all related data under its directory and the only place that requires installation/uninstallation into an OS is integrating into the [automatic start system](setting-up-and-running-additional-build-agents.md#Automatic+Start) (for example, service settings under Windows). 

## Agent Directories

The agent consists of:
* agent binaries (stored under `bin`, `launcher`, and `lib` directories). The binaries can be automatically updated from the server to match the server version.
* agent plugins and tools (stored under `plugins` and `tools` directories). These are parts of agent binary installation and are managed by the agent itself, updating automatically whenever necessary from the TeamCity server.
* agent configuration (stored under `conf` and `launcher\conf` directories). This is a unique piece of information defining the agent settings and behavior.
* [agent work directory](agent-work-directory.md) (stored under the `work` directory by default, configurable via agent configuration).
* agent auxiliary data (stored under `system`, `temp`, `backup`, `update` directories). The data necessary during agent running.
* agent logs (stored under `logs` directory): The directory storing internal agent logs that might be necessary for agent issues investigation.
### Agent Files Modification

The agent configuration directory is the only one designed to have files that can be edited by the user. All the other directories should not be edited by the user.

The content of the agent work directory can be deleted (but only entirely). This will result in a [clean checkout](clean-checkout.md) for all the affected builds.

The content of directories storing agent auxiliary data can be deleted (but only entirely and while the agent is not running). Deletion of data can result in extra actions during next builds on this agent, but this is meant to have only a performance impact and should not affect consistency.

## Important Agent Files and Directories

* __/bin__ 
    * `agent.bat` — batch script to start/stop the build agent from the console under Windows
    * `agent.sh` — shell script to start/stop the build agent under Linux/Unix
    * `service.install.bat` — batch file to install the build agent as a Windows service. See also [related section](setting-up-and-running-additional-build-agents.md#Build+Agent+as+a+Windows+Service).
    * `service.start.bat` — starts the build agent using the installed build agent service
    * `service.stop.bat` — stops the installed build agent service
    * `service.uninstall.bat` — batch file to uninstall the currently installed build agent Windows service

* __/conf/__: this folder contains all configuration files for the build agent 
    *  `buildAgent.properties` — [main configuration file](build-agent-configuration.md). This file is generated by the TeamCity server .exe installer and build agent .exe installer.
    * `buildAgent.dist.properties` — sample configuration file. You can rename it to `buildAgent.properties` to create an initial agent configuration file.
    * `teamcity-agent-log4j.xml` — build agent logging settings. For details, refer to comments inside the file or to the [log4j manual](http://logging.apache.org/log4j/1.2/manual.html)

* __/launcher/conf/__ 
    * `wrapper.conf.template` — sample configuration file to be used as a template for creating the original configuration
    * `wrapper.conf` — current build agent Windows service configuration. This is a Java Service Wrapper configuration java properties file. For details, see comments inside the file or Java Service Wrapper documentation.

* __/logs__ 
    * `launcher.log` — log of the build agent launcher
    * `teamcity-agent.log` — main build agent log
    * `wrapper.log` — log of the Java Service Wrapper. Available only if the build agent is running as a windows service
    * `teamcity-build.log` — log from the build
    * `upgrade.log` — log from the build agent upgrade process
    * `teamcity-vcs.log` — agent\-side checkout logs

* __/system__ 
    * `.artifacts_cache` — cache for all build's artifacts; can be [configured](free-disk-space.md#Configuring+artifacts+cache)

* __/temp__: temporary folder; the path can be overridden in the [`buildAgent.properties`](build-agent-configuration.md) file
    * `agentTmp` — temporary folder that is used by the build agent to store build\-related files during the build. Is cleaned after each build.
    * `buildTmp` — temporary folder that is set as the default temp directory for the build process and is cleaned after each build
    * `globalTmp` — temporary folder that is used by the build agent for its own temporary files. Is cleaned on the agent restart.