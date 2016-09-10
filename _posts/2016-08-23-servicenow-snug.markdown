---
layout: post
title: ServiceNow SNUG
date: 2016-08-23 18:00:00 -0800
category: servicenow
tags: [servicenow, books]
---

## NorCal SNUG

Yesterday I attended the [NorCal SNUG at ServiceNow HQ in Santa Clara](https://community.servicenow.com/events/2696).  
When I was in Alabama I hadn't attended any SNUGs in Birmingham, but this one was only a few miles from my new home.

## Chuck Tomasi

I enjoyed getting a chance to chat with Chuck Tomasi about the [ServiceNow Live Coding Happy Hour](https://youtu.be/z2Ebuih3srw) and other videos.  I followed along with this one and got the Activate/Deactivate working.  Our users aren't necessarily employees so we have to do deactivation differently than relying on LDAP refresh dates as a surefire indicator.  Using a script include means I can have these as buttons or call the deactivate methods from a script as well.
Later, during Chuck's talk on ServiceNow as a Development Platform, I learned he had been driving all day to get there so I really appreciated that.

## Istanbul

We got a sneak peek at Istanbul (not Constantinople), the next major release, which should be available before the end of the year.  Next year will be Jakarta, (not Jerusalem).  The CAB workbench looked useful as a way to replace the old multi-hour meetings I used to dial into.
I liked the automated agenda (the system at my previous company involved browsing for changes that were awaiting approval and could last 3 hours or more), and especially the idea of getting a notification when your time to speak is approaching!  

## Asset Life Cycle Management

KPMG & Ebay presented their case study on asset life cycle management that included integration with SAP.  That would be a major challenge and it sounds like they devoted the resources necessary to tackle it.  

## Organizational Change Management

We gathered in small group discussion to talk about our own experiences.  Incidents submitted via email still seems to be a challenge for everyone.
At my old company, we had incident creation via email disabled at Go-Live, cutting that off at the pass.  However, where I am now, we have to allow email incidents even from outside our domain, due to the nature of our business.  Incidents are about 10% spam as a result.  I implemented a business rule that might reduce it a little but am still waiting to see the results.

## Mobile and Barcode
I also learned about the new barcode field type and successfully tested it in my dev instance.  Now any mobile device can be used to scan barcodes directly into ServiceNow, great for asset management and saving the expense of those bulky scanners I used to deal with at my old company.  I set up a mobile page on my dev instance with a barcode field and scanned some of my books.  The next step will be to retrieve the title and author information.

## Cancel Requested Item UI Action
I've added a few more ServiceNow scripts to my snow repo, like the [Cancel Requested Item UI Action](https://github.com/tgbates/snow/blob/master/ui_actions/Cancel_Requested_Item.js).

## Impressions
I thought it was a great experience attending the SNUG, and I enjoyed getting to meet people and hear about how they are using ServiceNow.  And it included two meals!  Who says TANSTAAFL?


