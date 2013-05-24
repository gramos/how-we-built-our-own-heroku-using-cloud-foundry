!SLIDE 

.notes Hello everybody, we are happy for giving this talk for the Belarus Ruby
on rails user group, we will cover the main concepts of cloud foundry
and how you can do to install it and start hacking on it.

How we built our own heroku using Clound Foundry
================================================
Belarus Ruby on Rails User Group Meetup

![cloud foundry](cloudfoundrylogo.jpg)

.notes My name is Gastón Ramos, I'm from Santa Fe, Argentina
I work in Altoros
I've using Ruby since 2006. 
and I've done some contributions on projects such as Rails, Debian, Rubinius and Cloud Foundry

!SLIDE
Who are we? 
==========

!SLIDE

Gastón Ramos
============

* Using Ruby since 2006
* I'm from Santa Fe Argentina
* I Work in Altoros
* Open Source contributor: Rails, Debian, Rubinius, Cloud Foundry

!SLIDE

.notes I'm Alan Morán, I've been using Ruby since 2007, I'm from Bs As Argentina
I contribute in projects such as billingly and Cloud Foundry.

Alan Morán
=========

* Using Ruby since 2007
* I´m from Buenos Aires, Argentina
* I Work in Altoros.
* Open Source contributor: billingly, Clound Foundry

.notes Before starting with this talk we would like do some disclaimers. We are not cloudfoundry experts. We been working on Cloud foundry for the last couple of months so our knowledge is still limited. And the last thing we would like to point is that this talk will cover Cloud foundry V2 that is still under heavy development so is under constant changing.

!SLIDE
Disclaimer 
==========

* We are not Cloud Foundry experts.

* ~1 month working with CF.

* This talk will cover CF v2.

* V2 is under heavy development.


.notes So what`s Cloud foundry. Cloud foundry is a Pass, its an open source project and it available though a variety of pprivate cloud distribution and public cloud instances. including Cloudfoundry.com

!SLIDE
# What is Cloud Foundry? #

Is a PaaS,

It is an open source project and is available through a variety 
of private cloud distributions and public cloud instances, 
including CloudFoundry.com.


!SLIDE
# What is a Paas? #

* Is a category of cloud computing services.

* Facilitate the deployment of applications. 

* Heroku, Cloud Foundry, AWS Elastic Beanstalk, Engine Yard, Google App Engine.

!SLIDE
# What programming languages does Cloud Foundry support? #

Spring, Java, Rails and Sinatra for Ruby, Node.js. 
Scala and other JVM languages/frameworks including Groovy and Grails.

![CF supported langs](cloud-foundry-supported-langs.png)

Buildpacks in v2. ????

!SLIDE
Why Cloud Foundry matters to hackers?
-------------------------------------

![Hackers](hackers.jpg)

!SLIDE
Why Cloud Foundry matters to hackers?
=====================================

* It is open source. You can modify it. In fact we are doing it.

* It is well designed.

* No vendor locking. 

Cloud Foundry providers: AppFog, cloudfoundry.com, pass.io

!SLIDE
# Cloud Foundry Architectural Goals #

* No single point of failure.
* Distributed state.
* Self healing.
* Horizontally scalable.


!SLIDE

.notes  We will attend only to the main components of cf, which are 
the router, the cloud_controller, Dea and Nats.

# CF Core Components #

![cf-architecture](cf_architecture.png)

!SLIDE
# Nats #

* All the cloud foundry components communicate through nats.

* publish-subscribe messaging system.

https://github.com/derekcollison/nats

nats-sub foo &
nats-pub foo 'Hello World!'

!SLIDE

# Cloud Controller  

* REST API endpoints for clients 

* Written in Ruby, awesome!

* Maintains a database with tables for user information, like
  organizations, spaces, etc.

* CC

.notes 

!SLIDE
# Cloud Controller  

* Interacts with CF via http

   *Cf is the client used to manage user apps*

* Interacts with Dea and Health Manager via nats

**cf** <--- http ---> **Cloud Controller** <-- Nats --> **CF components**

!SLIDE
# CC and Nats #

 * Instructs a DEA to stage an application 
 
 * Instructs a DEA to start or stop an application.
 
 * Receives information from the Health Manager about applications.

!SLIDE
# CC and Nats #

 * Subscribes to Service Gateways that advertise available services
 
 * Instructs Service Gateways to handle provisioning, unprovision, 
   bind and unbind operations for services

!SLIDE

# CC Blob Store #

* **Resources** - files that are uploaded to the Cloud Controller with a unique SHA 
  
* **App packages** - unstaged files that represent an application
   
* **Droplets** - the result of taking an app package and staging it 
                 and getting t ready to run
	 
!SLIDE
# Droplet Execution Agent (Dea) #

* Is written in Ruby and takes care of managing an application instance's lifecycle.

* It can be instructed by the CC to start and stop application instances.

!SLIDE
# Droplet Execution Agent (Dea) #

* It keeps track of all started instances, and periodically broadcasts messages about their state over NATS.

* DEA depends on **Warden** to run application instances.

!SLIDE
![app_push](app_push_flow_diagram.png)

!SLIDE
# CLI for Cloud Foundry #

Using this tool you can deploy and manage applications.

gem install cf

cf push my-new-app
cf help --all

!SLIDE
# How can we do to get all cloud foundry components up and running?

**v1 vs v2**

When we started working with cloud foundry was only one way to install it.
BOSH, and it was not working.

There was no way to install it, easy.

So we made it.

!SLIDE
# A big app

A big app

* We had to learn a lot. 

* Cloud Controller - Dea - Health Manager - Router - 
  Warden - Services - Uaa

* 7 git repos

!SLIDE
Code Stats
-----------

```
git ls-files | wc -l
```

* Cloud Controller: 313
* Dea: 218
* Health Manager: 42
* Warden: 249
* Uaa: 628 


!SLIDE

Code Stats
-----------

```
git ls-files | xargs cat | wc -l
```

* Cloud Controller: 36211
* Dea: 24625
* Health Manager: 5422
* Warden: 23023
* uaa: 87512
* Router: 28151

!SLIDE
# CF vagrant installer #

CF Vagrant install born

* We decided to develop an installer using the dea_ng vagrant VM,

* we added the components one by one.

* vcap-dev mailing list was our best friend.

* Lot of hacking, research and reverse engineering.

!SLIDE
Resources:
==========

http://docs.cloudfoundry.com
http://www.slideshare.net/ramnivas2/cloudfoundry-architecture
http://blog.appfog.com/why-cloudfoundry-matters-to-hackers/
