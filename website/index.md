
[plugins]: http://projects.puppetlabs.com/projects/mcollective-plugins/wiki/

What Is MCollective?
-----

MCollective is an extensible framework for running tasks on many computers at once.

It can start Puppet runs, trigger application deployments, change system configurations, retrieve and report inventory data, and more --- since all of its capabilities come from easy-to-write plugins, you can use it to direct nearly any routine task you need to run on your systems.

MCollective is usually used interactively at the command line, but can also be used to build push-button web interfaces for common tasks, as well as complex orchestration applications.

> ### Why MCollective?
> 
> There are a lot of ways to execute remote tasks, but MCollective has some powerful advantages:
> 
> * It's parallel and extremely fast. 
> * It scales as well as its messaging middleware does, and modern middleware like ActiveMQ scales _very_ well.
> * It's extremely flexible about identifying and grouping nodes. You aren't limited to hostname-based identities, and can filter commands based on nearly any kind of server metadata, including location, operating system, hardware type, or even the state of an arbitrary file somewhere on disk. 
> * It runs tasks via well-defined RPC interfaces, instead of passing arbitrary shell commands verbatim. This gives you strong validation for your tasks, and makes it a lot harder for one bad command to cause an emergency.
> * It offers strong encryption, granular per-task authorization, and auditing for all commands, making it a good fit for dividing responsibilities in large teams.
> * It's cross-platform.


How Does MCollective Work?
-----

An MCollective deployment has four main parts:

* A central **message bus server,** which routes requests and replies. MCollective uses pre-existing middleware software instead of a custom server, which helps ensure smooth and well-understood scaling and clustering capabilities. (We generally recommend Apache ActiveMQ, although it's possible to use alternate middleware like RabbitMQ.)
* A **daemon,** which runs on every server in the deployment. It polls the message bus for requests, then triggers actions when necessary. 
* **Agent plugins,** which reside on every server and provide all of the actions that the daemon can trigger.
* **Clients,** which are operated by users and send requests on the message bus. The main one is the built-in `mco rpc` command-line client, but you can also build your own command-line and GUI clients. 

These parts work together to help you control your entire infrastructure:

TODO: insert diagram here

1. A user issues a request with a client.
    * Requests specify an **action** to run, and can be addressed to specific nodes or broadcasted with a **filter.** Filters describe a condition that must be met; each node tests that condition on itself, and ignores the request if it doesn't match.
2. The message server routes the request to its destination(s).
3. Nodes receive the request and check whether it's valid. A valid request must:
    * Be authenticated and authorized
    * Be addressed to this node or have a matching filter
    * Specify an action that is available on this node
4. Every node that accepts the request will use an agent plugin to execute the requested action, then send any data the action generates to the message server. 
5. The client that sent the request receives every reply, and can summarize the results for the user. 


Getting Started With MCollective
-----

To start using MCollective, you need to:

* Deploy and configure a message bus server, preferably ActiveMQ. 
* On all of your servers, deploy and configure MCollective and its prerequisites, including a connecter plugin and security plugin.
    * At this point, you can send requests with the standard `mco rpc` client and your servers can receive and react to them. They can't do many actions, but they can do things like send back system information.
* Choose some ready-made agent plugins and distribute them to your servers. 
    * At this point, you can do quite a lot! There are many agent plugins available, and they can interactively update packages, check whether services are running, run NRPE commands, do ad-hoc management of any Puppet resource type, and more.
* Write your own agent plugins for site-specific tasks, then distribute them to your nodes. 
    * At this point you have fast and flexible command over your entire infrastructure .
* Use advanced clients (like Puppet Enterprise's live management) or write your own clients to wrap actions and node discovery in alternate interfaces.
