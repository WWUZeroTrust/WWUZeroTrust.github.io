---
layout: post
title: "Zero Trust: Initial Project Overview"
date: 2019-12-6
---

## Introduction to Zero Trust Networks

Since the early 80’s when the first network, the ARPANET was created, we’ve always likened their interconnectivity to a neighborhood. We have our houses which are the individual computers, our post office which is the way we deliver messages between our houses, the roads which connect those houses are the ethernet cables, and the laws which govern how we drive are similar to protocols and so on.  In this metaphor gated communities would be businesses, since there was a gate to keep out unknown people everyone within the community could trust everyone and freely walk throughout each other's houses. They would be able to forego locks on their doors, share kitchens and amenities with each other and it would truly be a paradise. However, if a malicious person climbed the gate or somehow gained access to this community, well they would have the run of it. They could walk through people's houses taking what they wanted, and no one would think twice, after all once they were within the gates of the community they were trusted. This describes the problem facing enterprises in the modern era of the internet, especially when you have services hosted by third parties, heavily outsourced web applications, and convoluted supply chains.
<p align="center">
  <img src="/image/traditionalnetworkarch.JPG" alt="hi" class="inline" height="450" width="634"/>
</p>
The solution becomes simple for both our metaphor and modern networks when examining the shortcomings of these gated communities. The first step is to remove the gate outside the community, it serves no purpose if everyone and everything inside its walls are trusted. After this is removed, we can start securing our houses, putting locks on doors, place locks on, and shades over our windows, install a security system. Additionally, things like not inviting people we don’t know into our house and getting to know our neighbors so they can share information about strangers in the neighborhood can be good precautions to take. When we meet new people, often we are introduced by a friend, this applies to our metaphor as well as these friends act as a trusted proxy to begin a new relationship. Our neighborhood has transformed, before it was secure only in perimeter, now every house and individual are safe and secure. An intruder would immediately be identified and blocked from getting inside anyone's house, while friends and neighbors could still easily enter. This neighborhood sounds very much like a modern community and the security principles discussed using this metaphor can be translated to a modern network and when they are, the network becomes what we call a Zero Trust Network. 

## Initial Plan 

The deliverables for this quarter were going to be a full working network using hardware we owned, specifically two physical servers, connected to the computer science department network.  We met with Dan Van Pelt, the senior system administrator for the CS department network, on October 25th, 2019 where he told us they could not host our physical servers. Dan informed us that they would give us virtual servers and allow us to access them at the start of Winter Quarter. This significantly delayed our timeline, by our estimate it pushed us back for the whole quarter. Additionally, we lost some element of control of our project by outsourcing physical management of the machines to the CS department. Below is the topology and timeline of our initial plan for the quarter: 

<img src="/image/fall responsibilites.JPG" alt="hi" class="inline" height="450" width="634"/>

<img src="/image/ZTN Topology.jpg" alt="hi" class="inline" height="450" width="634"/>

## Revised plan 

In order to recover from these setbacks, we were determined to make progress on what we could at the time, so we began developing virtual machine images for our devices. This helped us to discover and remedy any issues as they appeared, it will also allow us to move swiftly at the beginning of Winter quarter by placing these images into our virtual environment quickly. Fall quarter we recovered handily and created a barebones network, set up with a web server with the domain name ‘wwuzerotrust.com’. We connected this with a remote MySQL database and installed WordPress on the server as well as securing the connection through an SSH tunnel. While we are not aligned with our timeline or our initial plans topology, we were able to make significant progress despite our setbacks as outlined in our updated topology and timeline below: 
<p><center>
<img src="/image/revisedtopology.JPG" alt="hi" class="inline" height="442" width="623"/>
</p>
Orange indicates not completed items 

Green indicates completed items 

<img src="/image/revisedfallplan.JPG" alt="hi" class="inline" height="442" width="623"/>

## Elements of a Zero Trust Network 

Beginning Winter Quarter, we will start implementing the more unique aspects of our project and of Zero Trust Network, namely the control plane. The overall goal and deliverable for Fall Quarter is to create a network where we can easily collect usage data and metrics to form data points to give the control plane an easier time making determinations. The control plane consists of three distinct elements, the Trust Engine, Network Agents, and the Policy Engine. This control plane watches every action that takes place on the network and authorizes every action. Below is an example how the control plane interacts with different elements from the traditional security architecture shown in the introduction: 

<p><center>
<img src="/image/controlplaneexample.JPG" alt="hi" class="inline" height="442" width="623"/>
</p>
The distinction between Authentication and Authorization becomes important when designing the control plane, Authentication should happen once at the entry point into the network, while Authorization should happen for every communication happening within the network.  

## Trust Engine 

Our design for a trust engine system, which is documented below, incorporates three layers: User, Device, and Application. 

<p><center>
<img src="/image/Trust Engine Diagram.png" alt="hi" class="inline" height="592" width="922"/>
</p>
Each layer (User, Device, and Application) has an independent trust score. The Trust Engine weighs the layers equally in order to reach a final trust score of 1. Each upper layer is dependent on the action of the lower layer (User is the bottom) as required by the scoring scheme. For instance, the ID, PKI, and TGB create a .99 User score. This means it passes the test. The .99 User score is the equivalent weight of .33 Trust Engine score. This .33 Trust Engine score from the User layer is used in two calculations. The first is its contribution to the Device layer trust score. The Device layer combines the ID, Certificate, and User, in order to achieve a Device score of .99 . This process is similarly repeated for the Application layer trust score. Second, the .33 Trust Engine User score previously mentioned is tallied along with the Device and Application score to form the complete Trust Engine score.  

The trust engine score is then used in both Authentication decisions and Authorization decisions. Usually a user will never know their trust score, they may be shown their trust score periodically and infrequently to prevent abusing methods to gain more trust. This trust score is then utilized when forming network agents, essentially the trust score computed from these three layers gets signed out to a network agent which acts on behalf of the user as a sort of proxy. This implementation will be discussed further below. 

## Network Agents 

Network agents are a service which is formed on-demand in communication channels within the network to handle communications. These communications could be from user to device, user to application, application to application, device to device, etc. That agent is formed with specific identifiers and a computed trust score is also assigned to it. This way we authorize not just the user, device or application individually, we authorize all three for one action. Below we can see this data from user, device and application along with the current activity being computed through the trust score and assigned to a network agent.

<p><center>
<img src="/image/networkagentexample.JPG" alt="hi" class="inline" height="592" width="922"/>
</p>

## Policy engine

The policy engine works in tandem with the trust engine to make the authorization decisions for the network. Once the trust score is computed and assigned to a network agent, the policy engine takes that score and compares it to a pre-defined policy set in place for that specific resource, device, or application based on historical data. If the policy engine grants access, a process daemon would edit security rules on a firewall or similar device for one-time access for that network agent. The system we are choosing for this is the Open Policy Agent.

<p><center>
<img src="/image/policyengineexample.JPG" alt="hi" class="inline" height="592" width="922"/>
</p>

## Conclusion 

The next step is to prepare for Winter Quarter. We intend to have the machines ready, tested, and the network planned so that we can set up our network immediately at the start of the quarter. It is important to begin on the actual Zero Trust aspects of the project prior to starting Winter Quarter as it will be a time intensive and long learning process. This requires writing down and fleshing out details on how the Trust Engine, Network Agent, and Policy Engine will function. As there is little documentation online on how the Engines and Agent function, we will need to be creative and seek out unique solutions fitting for our project. 

## Sources 

Gilman, Evan, and Doug Barth. Zero Trust Networks: Building Secure Systems in Untrusted Networks. OReilly Media, 2017. 
