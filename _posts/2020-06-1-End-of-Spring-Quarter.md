---
layout: post
title: "Zero Trust: Demonstration and Project Update"
date: 2020-06-01
---
<style type="text/css">
    ol { list-style-type: upper-alpha; }
</style>

Our project has drastically changed since its inception. Assumptions made about data flow and handling were revised, and the Trust Engine took on many revisions. Utlimately, we now have a product that can build a trust score based on user activity and authorize access dependent on a passing score. There is more to do, and we will speak on that at the end of the blog, but in its current state we can now show a working product. 

The overview below is a high level mapping of our network. The flow of information travels in two directions from the user. The first is the osquery logs being delivered hourly to the Fleet server in order for the Trust Engine to determine a trust score. In the other direction the user is making an attempt to log into a secured application. If the user has not accessed this resources within a recent window then traffic will be redirected by the Network Agent to the Policy Engine where the Policy Engine will decide if the user is granted access based on the users trust score.

## Project Overview

<img src="/image/Zero Trust Network Overview.png" alt="hi" class="inline" height="890" width="1372"/>

## Data Flow

The time between when a user is interacting with their computer and when the user trys to log into a secured application has many different process at play and tools involved. The Documentation section will provide a more extensive description of each of the steps.

1. Kolide Fleet server runs a scheduled query pack on the client machine that already has osquery installed on it.
2. The queried data is delivered to the Kolide Fleet server.
3. Log file data from Kolide is shipped by Filebeat to Logstash. If there are multiple servers then each would have a Filebeat agent on it.
4. Logstash aggregates the data recieved from Filebeat and sends it to Elasticseach.
5. Elasticsearch holds the JSON formatted data under the index: osquery-result-yyyy-mm-dd. The data can be viewed in Kibana.
6. The Trust Engine ingests data from elasticsearch and uses the query fields to determine a trust score. The Trust Engine only requests
data from Elasticsearch when triggered by the Trus API Handler seeking validation of a specific user.

A. The users goes to log into the secured application and Authelia verifies the user via their username and password.
B. After the user is validated the traffic is redirected the web proxy Traefik.
C. Traefik forwards the request to the Swissknife Handler.
D. The Swissknife Handler 
E.
F.
G.
H.
I.
J.

<img src="/image/Flowchart.png" alt="hi" class="inline" height="834" width="1414"/>

## Trust Engine

## Network Agent

## Policy Agent
