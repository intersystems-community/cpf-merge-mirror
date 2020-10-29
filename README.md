# CPF Merge File example

## Intro
This project shows how to automate the configuration of InterSystems IRIS instances. It shows how an InterSystems HA mirror-pair topology can be achieved with a simple **CPF merge file** declaration.

It shows how to configure
- instance parameters and  
- the pair  of mirror instances

The *CPF merge file* allows injecting configuration parameters as an instance starts up according to the [12-factor](https://12factor.net/) app. The process is simple, it is based on a declaration that tells each instance its role (mirror primary | backup | DR Async). Each instance implements its state by configuring itself. The process is intuitive and easy to understand as the idempotent configuration parameters and resources are merged and implemented into the system before it starts up.
The CPF merge facility appeared in InterSystems IRIS 2019.4 and is continually improved.

## WARNING
The container used for the *InterSystems Virtual Summit 2020* demo is not yet available. The feature described herein the *.conf* files will be available from InterSystems IRIS 2020.4.
You might have noticed that we have improved the verbose way we had to define a database with a better, more succinct syntax.
Also be aware that you will need a valid *iris.key*

## Implementation Details
The docker-compose runs 3 services:  
- 2 x IRIS instances and  
- 1 x Arbiter  

The 2 InterSystems IRIS instances will configure themselves as a mirror-pair, respectively, one as the Primary and the other as the Backup member. An Arbiter instance is also provisioned.
Please review the files:  
- *mirrorPrimary.conf* and  
- *mirrorBackup.conf*  

and see how the Primary and the Backup members are defined. It should be self-explanatory.

- [The main CPF merge file documentation page that will soon be updated with this new features](https://docs.intersystems.com/irislatest/csp/docbook/Doc.View.cls?KEY=ADOCK#ADOCK_iris_customizing)  

InterSystems IRIS [durable %SYS](https://docs.intersystems.com/irislatest/csp/docbook/Doc.View.cls?KEY=ADOCK#ADOCK_iris_durable) directories will be created in  
- ./iris.sys.d1 and  
- ./iris.sys.d2  

and the mirrored database, that we want created at startup will be in 
- ./iris.sys.d1/myappdata and  
- ./iris.sys.d2/myappdata  

A container-network is created with a range of ip addresses to  
- pin the services to a fixed IP and  
- to avoid conflict with local subnets


## Requirements and Notes
- You will need a non CE version of InterSystems IRIS 2020.4
- You will need an adequate iris.key file supporting InterSystems mirroring
- To reset your local  environment and remove the durable %SYS directories there is the *deleteDurableSYS.sh* script
- Two convenient scripts to start and stop the composition are also provided: *start.sh* and *stop.sh*

---
