---
title: "Runstatus Quick Start"
slug: "quick-start"
meta_desc: "Runstatus is a simple hosted status-page that lets you communicate your status easily. This documentation will guide you to quickly set it up and start using it."
tags: "runstatus,quick-start"
---

Runstatus is a simple hosted status-page that lets you communicate your status to the external world while reducing internal load in your organization.

Runstatus is based upon some simple concepts that help you describe a defined status.

* **Services** describes components of your application, infrastructure or more generally of the entity of which you want to communicate status.
* **Incidents** are time frames where status of your services varies. An incident is composed by several **events** that compose an *incident event stream*.
An incident can affect one or more services.
* **Maintenances** are announced time frames where status of your services my vary in the future. A maintenance is also composed by several events that compose a *maintenance event stream*.

Lets review some of those concepts in more detail.

## Services
Services are the core of Runstatus and the first thing you should set-up and carefully plan. Services let you describe the entity you want the status to be publicly available. Services examples could be:

* "API"
* "Network"
* "Payment Process"
* "Databse"

You can set up services under the settings view of your Runstatus page.
Services are not mandatory though: you can also use Runstatus without them, and simply display to the world a global status.

### What will be displayed to your customers about services

On the front end of your Runstatus page the is of your services will be visible to the world. Each service will have an icon to signal his health in a determined moment of time. Adding a new incident may vary the service and change the display of a service row in the front-end. 

## Incidents
An incident is the main tool to communicate a variation of your services.
Incident can affect none, one, or several services. Incidents are composed by events which are enriched with details about that specific point in time.
An incident is enriched with:

* **State** describes the state of the selected services during an event.
	It can be one of the following:
	* OPERATIONAL
	* DEGRADED PERFORMANCE
	* PARTIAL OUTAGE
	* MAJOR OUTAGE

* **Status** describes the state of the selected services during an event.
	It can be one of the following:
	* RESOLVED
	* INVESTIGATING
	* IDENTIFIED
	* MONITORING

When you open an incident you implicitly create a first event. You will be asked to choose a state and a status for your event, plus a general title for the incident and a description of the event. Opening an incident you can't define an Operational state.

Posting the incident will create a first event and bring you in the incident detail screen. From here you can continue to update your customers about the evolution of the incident posting new events.

At any time you can close the incident with a last event. State of your services will be set to *operational* and status to *resolved*.

### What will be displayed about incidents to your customers

When no incidents are open, your *default status message* is displayed on top. You can change it from the settings view of your Runstatus page.

The *Current Events* section will be empty, signaling no incidents are currently ongoing.

Once you open a incident the status message on the top will change to the incident title and will stay until you don't close the incident. Would you have more incidents open in the same time the status message will report all the titles.

A color code helps quickly get an idea of what's going on. Green is for operational status, yellow for degraded performance or partial outage, red for major outages. The status message will keep the color of the first incident in the time-line.

The *Current Events* section will report a line for each open incident, with the description, status and color of the state of the latest event in his event stream.

The service affected will show an icon corresponding to the their state.

For each incident you will have an *incident time-line* where you will be able to follow the entire event stream.

All past incidents are visible from the historic view, where per-day outage times are showed too.

## Maintenances
As for the incidents, maintenances are composed by events that let you describe the maintenance flow as it happens.

Creating a new maintenance implies you to define a time window for it. You will be asked for a start time and an end time for your maintenance.
A title and a description of the maintenance will also be required.

Maintenances have a status too. They can be one of the following:
	* SCHEDULED
	* IN-PROGRESS
	* COMPLETED

Unlike the incidents, maintenances status is automatically set during his life cycle. A new maintenance starts in a *scheduled* states, becomes *in progress* once you add an event to it and acquires the *completed* status when you close it.

Maintenances too can be affected to services.

The *in progress* state is not mandatory though: you can simply close your maintenance and it will be set to *completed* immediately.

### What will be displayed about maintenances to your customers
Scheduled maintenances are displayed on the front of your Runstatus page. They will show part of the description and will point to a detailed page where you will be able to follow the event stream too.

Past maintenances have their own historic time-line.

## Customization of your Runstatus pages
From the settings view you have access to several option to customize your Runstatus page.

You can define your timezone, a default status message, list your services and tweak the look and feel of your status page header. A dark theme is also available.

## Twitter integration
Under settings you have the opportunity to link your twitter account to the Runstatus application. This will allow you automatically tweet about incidents and next maintenances.

For each new event you will be proposed a tweet text and preview to clearly see what you will tweet. Saving the event will automatically tweet and let your user be updated in no time.


