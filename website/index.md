---
layout: default
title: Marionette Collective
toc: false
---

[plugins]: http://projects.puppetlabs.com/projects/mcollective-plugins/wiki/
[puppet]: 
[deploy_daemon]: 
[deploy_client]: 
[deploy_amq]: 
[deploy_amq_cluster]: 
[deploy_plugins]: 
[list_plugins]: 
[usage_cli]: 
[usage_filter]: 
[usage_daemon]: 
[plugin_agents]: 
[plugin_agent_reference]: 
[plugin_agent_ddl_reference]: 
[plugin_clients]: 
[plugin_client_reference]: 
[plugin_data]: 

<!-- The following references are not used in the text:
[plugins]:
-->

Welcome
-----

This is the MCollective documentation.

* If you've been here before, use the navigation sidebar or the search field above to find what you're looking for.
* If you're new here, see below for the info you'll need to get started.


What Is MCollective?
-----

MCollective is an extensible framework for running tasks on many computers at once. It's especially good at choosing flexible groups of machines to work with. It's usually used interactively at the command line, but it can also power push-button interfaces and complex orchestration applications.

All of MCollective's capabilities come from easy-to-write plugins, so you can use it to control nearly anything across your infrastructure. There are many pre-built plugins available, which can do things like:

- Start [Puppet][] runs
- Upgrade packages
- Start and stop services
- Retrieve inventory data

...but MCollective's real power comes from writing your own plugins. Use it to trigger application deployments, flush the mail queue, bring up entire clusters of services in a precise order, or do anything that needs to happen on demand across your infrastructure. 


> ### Why MCollective?
> 
> There are a lot of ways to execute remote tasks, but MCollective has some powerful advantages:
> 
> * It's parallel and extremely fast. 
> * It can discover and group nodes on the fly, by using node-specific metadata to filter commands. Instead of keeping a central inventory, you can simply ask all production machines to run an action.
> * It scales as well as its messaging middleware does, and modern middleware like ActiveMQ scales very well.
> * It runs tasks via well-defined RPC interfaces, instead of passing arbitrary shell commands verbatim. This gives you strong validation for your tasks, and makes it a lot harder for one bad command to cause an emergency.
> * It offers strong security, including encryption, per-task authorization, and auditing.
> * It's cross-platform.


Getting Started with MCollective
-----

To start using MCollective, you'll need to deploy it on your servers, learn its command line interface and administrative tools, and start writing your own plugins and applications. 

### Deploying MCollective

MCollective has four main parts to deploy:

* The **MCollective daemon,** which runs on every server and listens for commands on the message bus.
    * [Install and configure the MCollective server daemon][deploy_daemon]
* The **`mco` client,** which is installed on admin workstations and can issue commands to the collective.
    * [Install and configure the command line client][deploy_client]
* A **message bus server.** MCollective can use several popular middleware servers, but we recommend Apache ActiveMQ.
    * [Install and configure ActiveMQ][deploy_amq]
    * [Clustering ActiveMQ servers][deploy_amq_cluster] for large deployments
* **Plugins,** which are installed on both servers and admin workstations, and which provide actions and other  capabilities.
    * [Install and Configure Plugins on Servers and Client Nodes][deploy_plugins]
    * [Available Plugins][list_plugins]

### Using and Administering MCollective

* [Basic command line usage][usage_cli]
* [Advanced filtering and addressing][usage_filter]
* TODO something about the config files
* [Monitoring and controlling the server daemon][usage_daemon]

### Writing Plugins

MCollective uses several different types of plugins and add-ons.

* **Agent** plugins provide actions that you can trigger on your server nodes. Nearly all users will write agent plugins.
    * [Writing SimpleRPC agents][plugin_agents] --- A general tutorial for writing agent plugins.
    * [SimpleRPC agent quick reference][plugin_agent_reference]
    * [Agent DDL quick reference][plugin_agent_ddl_reference]
* **Clients** can send commands to servers and can collect and format the data they return. You can write special-purpose CLI clients that format data in ways the standard `mco rpc` client can't, give MCollective abilities to pre-existing management applications, or create orchestration applications that issue many commands in a specific order. 
    * [Writing SimpleRPC clients][plugin_clients]
    * [SimpleRPC client quice reference][plugin_client_reference]
* **Data** plugins provide new kinds of per-node metadata. You can use this data to filter commands, or just retrieve it to get insight into your infrastructure.
    * [Writing data plugins][plugin_data]

For more information on other types of plugins, see "plugin interfaces" in the navigation sidebar. 
