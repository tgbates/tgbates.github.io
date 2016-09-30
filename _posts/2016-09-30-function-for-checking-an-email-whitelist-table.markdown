---
layout: post
title: ServiceNow Function for Checking an Email Whitelist Table 
date: 2016-09-30 08:00:00 -0600
category: servicenow
tags: [servicenow, javascript]
---

# ServiceNow: Function for Checking an Email Whitelist Table 
Where I work now we accept incidents submitted via email from internal and external customers.
I recently had a requirement for a new assignment group that only wanted to accept incidents from a short list of domains.  Emails submitted to this group from other domains would not be processed into incidents.

We had an existing script include and inbound action to process incoming emails already, and an easy way to handle this would be with some if statements inside the inbound action.  However, this would not be easy to maintain or update if another group wanted a similar restriction down the road.

So I set up an email whitelist table with a reference field to the group table, a string field for the domain, and an active flag to turn off the rule if needed.
![Email Whitelist table](/assets/email_whitelist.png "Email Whitelist table")

I added a function to the existing script include to check this table.
If the assignment group has an active record (or multiple records), check the domains.  If there is a matching whitelist rule, allow the incident to be created.
If a record isn't found, allow the incident creation as normal.


```javascript
	checkWhitelist: function(assignment_grp, sender_domain) {
		// assignment_grp = reference to sys_user_group table
		// sender_domain = string
		// lookup whitelist table and if there is an active entry, apply it
		// if no entry returned, then allow any sender
		
		var gr = new GlideRecord('u_email_whitelist');
		gr.addQuery('u_group', assignment_grp);		
		gr.addQuery('u_active', true);  // if entry has been deactivated ignore it		
		gr.query();
		var grp_flag = false; // at least one active entry on whitelist table?
		var match_flag = false;  // matching domain for assignment group on whitelist?
		var i = 0;
		while (gr.next() && (!match_flag)) { // the group has at least one entry on the table but no match found yet
			i++;
			grp_flag = true;  // group entry on whitelist table
			if (gr.u_domain == sender_domain) {
				match_flag = true;  // sender allowed on whitelist
			}
			else {
				gs.log('checkWhitelist - no match for ' + gr.u_group.name + ' : ' + gr.u_domain);
				// sender not a match, keep looping through all matching records
			}		
		}
		if (grp_flag)   { // both entries good
			if (match_flag) {
				gs.log('checkWhitelist - match for ' + gr.u_group.name + ' and ' + gr.u_domain);
				return true; // there is a match and that's ok
			}
			return false; // there wasn't a match
		}
		else {
			return true; // there wasn't a match for the grp so it's ok to accept from any domain
		}		
	},
	
```

To whitelist new domains we only need to add another entry to the whitelist table.  The same goes for a new assignment group with a similar requirement.
