---
layout: post
title: "Zero Trust: Demonstration and Project Update"
date: 2020-06-01
---

Our project has drastically changed since its inception. Assumptions made about data flow and handling were revised, and the Trust Engine took on many revisions. Utlimately, we now have a product that can build a trust score based on user activity and authorize access dependent on a passing score. There is more to do, and we will speak on that at the end of the blog, but in its current state we can now show a working product. 

## Project Overview

The overview below is a high level mapping of our network. The flow of information travels in two directions from the user. The first is the osquery logs being delivered hourly to the Fleet server in order for the Trust Engine to determine a trust score. In the other direction the user is making an attempt to log into a secured application. If the user has not accessed this resources within a recent window then traffic will be redirected by the Network Agent to the Policy Engine where the Policy Engine will decide if the user is granted access based on the users trust score.

<img src="/image/Zero Trust Network Overview.png" alt="hi" class="inline" height="750" width="950"/>

## Data Flow

The time between when a user is interacting with their computer and when the user trys to log into a secured application has many different process at play and tools involved. The Documentation section will provide a more extensive description of each of the steps.

<img src="/image/Flowchart.png" alt="hi" class="inline" height="750" width="950"/>

## Trust Engine

The Trust Engine is built with Python. Using the elasticsearch-dsl package, we were able to extract queries held in Elasticsearch. The query snapshot data is in JSON format so it needs to be parsed with regex in order to put the query fields into Python lists. We established what data was already considered expected or 'trusted' into lists and those lists were compared to the list data extracted from Elasticsearch.

The way the scoring is done at the moment is relatively simple. Each field from the query starts at a base score and if the field it is
compared to is matching it increases by a specified amount, but if there is not a match after comparing lists then the score is reduced.

We also created a Trust API Handler. The Trust Engine waits on standby to get a username from the Trust API Handler, and that username is what is used for querying from Elasticsearch.

This program took a long time to make. We tried a lot of different routes for extracting the information and also for parsing the JSON data into string format for Python lists. It was also very difficult to get the scoring code section to work because of a series of if and for loops creating a messy outcome. There is still more work to do but we are satisfied with its current state.

## Network Agent

The Network Agent is a combintation of Authelia and Traefik. Authelia is a Single Sign-On service and Traefik is a proxy users connecting to the secured applications. The full data flow through Authelia and Traefik is detailed on the Documentation page under the Network Agent. Authelia sits before any of our applications on the front end and requires users to sign into it before accessing any applications. Traefik will route requests according to our specifications, which begins by forcing them through Authelia before re-routing them to our secured applications once the Policy Engine allows it.

## Policy Engine

The policy agent uses Role Based Access Control to grant access to users. The tool used is Open Policy Agent (OPA). OPA receives information from the Swissknife handler which acts as a Middleware between OPA and the Trust API Handler. OPA reads that information and gives it a true or false for user rights access dependent on the trust score received.

## Final Thoughts on Implementing a Zero Trust Network

We started this project with only a vague idea of how to implement a Zero Trust network. Through multiple evaluations and testing we managed to find a combination of tools that could demonstrate a working example of a Zero Trust network. We believe the amount of time we put into this project was worthwhile. It introduced us to a lot of new tools and furthered some of our understanding in areas we only felt somewhat knowledgeable about initially. We became more familiar with Docker containers, JSON data and HTTP requests, designing a Zero Trust environment from the ground up, and even a little CSS and HTML.

## Future Plans

* Add caching to the Handler and Trust Engine to ensure it only runs once 
* Containerize all the systems and programs so that they can all be spun up quickly and easily 
* Make the Trust Engine score more accurate
* Set standards for scoring and metrics 
* Refine osquery data fields 
* Add Intrusion Detection System and another end-point monitoring system to increase metrics we gather and score on 
