---
layout: post
title: "Project Update #1"
date: 2019-04-20
---
# Project Update #1 March 20th 2020
## Winter Quarter Initial Plan 

Going into winter quarter we were determined to make up lost ground from the previous quarter. Having previously met with the CS department and having a received written promise of dedicated hardware with which to build our project, we patiently waited for a response at the beginning of the quarter. After reaching out several times through several different channels of communication, we finally received the dedicated hardware promised to us during Fall quarter, only three weeks into Winter quarter.  

Most, if not all, of the actual technical work completed during the Fall quarter had to be discarded when we realized that the machine images, we had created using VMware would not transfer into Proxmox. This was the first of many hurdles we would face throughout the quarter. We also had a lot of difficulties getting Proxmox set up since all of us were unfamiliar with it, which set us back a couple of weeks. Trying to balance both school and work got the better of most of us as well and it became easy to push back deadlines as we struggled through our classes this quarter.  

 <img src="/image/Trust_Engine_revised.jpg" alt="hi" class="inline" height="621" width="345"/>

## Winter Quarter Revised Plan 
The goals of our project have not changed. We intend to build a functional network that implements the tenets of a Zero Trust model. Our setbacks were initially because we did not receive our Proxmox server from the CS department until the third week of Winter Quarter. We initially expected to receive those resources at the beginning of the quarter.  

During this time, we discovered new tools and chose to move forward with simpler plans. This included removing ELK Stack and instead delivering the data to a MySQL Server. We also deferred installing pfSense and an IDS until the Zero Trust elements of the network were first implemented. The main tools for gathering behavioral data of user machines are osquery, Kolide Fleet, and Redis. Managed machines each have osquery installed on them, which is a tool that exposes an operating system as a high-performance relational database. Fleet is a management tool that oversees the queries and scheduling run by osquery and retrieves that data. Redis offers Fleet the ability to run queries in real-time which is helpful for administration purposes. The data is held in a MySQL database. Once the Trust Engine elements are finalized the data stored in the MySQL DB will be compared to data held in the Historical DB. 

We will use the tool Traefik as the Network agent element of the control plane. This tool allows us to proxy requests to our network from both inside and outside the network. It will allow us to implement basic authentication as well as redirecting any foreign request to the osquery image we created. Then we can forward their traffic to our firewall which receives its rules from the Open Policy Agent. The Open Policy Agent tool is essentially Role-Based Access control with privileges and policy also stored within it. This element of the network will give the final determination to allow or deny traffic and send that decision to the Firewall.  

As of March 13th, 2020, we have the design of our network, all the machines are created in our Proxmox instance and we are beginning to work on all of them. We have the Open Policy Agent up and running, as well as the connection between a test osquery machine and the instance of fleet on the Trust Engine. We are still creating the script for scoring the results that osquery will return to us, as well as still implementing the Network Agent with traefik.  

 <img src="/image/winter_quarter_initial.png" alt="hi" class="inline" height="621" width="697"/>

Update for the Trust Engine

 <img src="/image/Trust_Engine_revised.jpg" alt="hi" class="inline" height="1001" width="751"/>

Above is our design for the trust engine as of March 13th, 2020 and a more technical specification is available in the Zero Trust Specifications document attached with this one.  

Update for the Network Agent

The application we have selected for our Network agents is an application layer edge router called traefik. This application acts as a proxy and router all in one service, first it will grab incoming requests routing to our internal or external services. Then it will force them through the entrypoint defined by traefik, which it can discover and automatically route requests towards. Next the middlewares begin to transfer the traffic to different services detailed below. 

 <img src="/image/traefik1.png" alt="hi" class="inline" height="808" width="406"/>
https://docs.traefik.io/middlewares/overview/
The specific middleware we are going to be using is first BasicAuth which asks the user to sign in using a username and password combo as demonstrated below: 

 <img src="/image/traefik2.png" alt="hi" class="inline" height="724" width="203"/>
https://docs.traefik.io/middlewares/basicauth/
After this middleware we will use the redirect middleware to push the user to our specific image of osquery which will connect back and give data to the Trust Engine for use in calculating the trust score. 

The Next middleware will be AuthForward which forwards the authentication to a server before sending it to the service as seen below. That authentication server will be the Open Policy agent/Firewall combination. 


https://docs.traefik.io/assets/img/middleware/authforward.png
Update for the Policy Agent

Open Policy Agent Process Flow 
image 4
The OPA receives data from traefik such as the user, action, resource, and trust score. It uses this data to compare it to a Rego policy script that acts as a Role-Based Access Control list. If it fits in the approved policy, it will modify the pfSense firewall rules and allow passage to the resources behind the firewall. 

Conclusion 
With the slow start to Winter quarter a lot of our planning had to be abandoned in order for a more streamlined and direct implementation of our systems and services. This meant that we had to identify areas of work that would simply take too much time to complete with the deadlines set forth. As such we were not able to progress as far as we had planned and make as much meaningful progress during this work period. Our shortcomings have forced us to more closely examine our plans moving forward into Spring quarter and reassess the outcomes of this project. We have decided that instead of a fully implemented and working network as outlined in our initial plan Fall quarter, it would be wiser to develop systems that can serve as a Proof of Concept to fulfil the ideals of Zero Trust Networking. This way our final product can more accurately reflect the goal we all set out to achieve at the inception of this project and that is an open-sourced implementation of Zero Trust that can be replicated by others. 

References
Gilman, Evan, and Doug Barth. Zero Trust Networks: Building Secure Systems in Untrusted Networks. OReilly Media, 2017. (1) 


