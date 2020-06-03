---
layout: post
title: "Zero Trust: Demonstration and Project Update"
date: 2020-06-01
---

Our project has drastically changed since its inception. Assumptions made about data flow and handling were revised, and the Trust Engine took on many revisions. Utlimately, we now have a product that can build a trust score based on user activity and authorize access dependent on a passing score. There is more to do, and we will speak on that at the end of the blog, but in its current state we can now show a working product. 

## Project Overview

The overview below is a high level mapping of our network. The flow of information travels in two directions from the user. The first is the osquery logs being delivered hourly to the Fleet server in order for the Trust Engine to determine a trust score. In the other direction the user is making an attempt to log into a secured application. If the user has not accessed this resources within a recent window then traffic will be redirected by the Network Agent to the Policy Engine where the Policy Engine will decide if the user is granted access based on the users trust score.

<img src="/image/Zero Trust Network Overview.png" alt="hi" class="inline" height="890" width="1372"/>

## Data Flow

The time between when a user is interacting with their computer and when the user trys to log into a secured application has many different process at play and tools involved. The Documentation section will provide a more extensive description of each of the steps.

<img src="/image/Flowchart.png" alt="hi" class="inline" height="834" width="1414"/>

## Trust Engine

## Network Agent

## Policy Agent
