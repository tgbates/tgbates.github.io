---
layout: post
title: Client UI Action in ServiceNow
date: 2017-05-20 06:30:00 -0600
category: blog
tags: [servicenow, javascript]
---



# Client UI Action in ServiceNow

I had a requirement for work start and end dates to be required for any "Implementation"
Change Tasks.  My existing Close Task UI Action on the change_task form was bypassing
the mandatory change_request.work_start field.
I had it automatically inserting the current timestamp into the work_end field if it was empty,
but now I needed to check and make sure work_start was also entered before closing the task.
This had to happen only for the implementation task in the workflow, not the testing task.


1. Add OnSubmit client script to return false if Work Start is mandatory, but not completed
2. Update UI Action with client and server code to check form and update record
3. Update UI policies surrounding work start/end fields and when they are required.
4. Deactivate other Close Task UI Actions on change_task table

# Client Script

	Note: This client script relies on the ui policies to set the work_start field as mandatory
	Name: Change_Task - Reject if No Work Start
	Description: If work start field is mandatory, but not entered, halt submission
	Table: change_task
	Type: onSubmit
	Script:


    function onSubmit() {
	    var work_start_mand = g_form.isMandatory('change_request.work_start');	
	    if (work_start_mand) {
	    	var workStartDate = g_form.getValue('change_request.work_start');
	    	g_form.clearMessages();
	    	if ((workStartDate === "") ) {
	    		g_form.addErrorMessage(new GwtMessage().getMessage("{0} must be completed", g_form.getLabelOf("change_request.work_start")));
	    			return false;
	    		}
	    }
    }

	


# UI Action - Close Task 

This ui action has a client-side function workstart which is called when the user clicks the form button.
If the work start field has been set mandatory by a UI policy, but it is not completed, it gives an error.

The server-side function runBusRuleCode enters the current time in the work end field.

	Table: Change Task [change_task]
	Action name: close_change_task
	Show insert: true
	Show update: true
	Client: true
	List v2 Compatible: true
	Form button: true
	Onclick: workstart();
	Condition: current.state < 3 && current.approval != 'requested'
	
	    /*
	     * If the workflow activity for this task is an implement task:
	     *  1. timestamp the work end field for the change request with current time
	     *  2. close task
	     * (see incident for model used)
	     * Force change_request.work_start field to be completed before implementation task can be closed complete
	     */
	    function workstart() {
	    	// client script called initially Onclick
	    	// sets state to closed complete to trigger UI policies
	    	// checks whether work_start is mandatory
	    	g_form.hideFieldMsg('change_request.work_start');
	    	g_form.clearMessages();
	    	g_form.setValue('state', 3);
	    	var mandatoryWorkStart = g_form.isMandatory('change_request.work_start');
		
	
		if (mandatoryWorkStart) {
			// make sure work start is completed
			if (g_form.getValue('change_request.work_start') == ''){			
				g_form.showFieldMsg('change_request.work_start','Date is mandatory when closing implementation task.','error');
				g_form.addErrorMessage(new GwtMessage().getMessage("{0} must be entered to close the task", g_form.getLabelOf("change_request.work_start")));		
				return false;
			}					
		}
	
			//Call the UI Action and skip the 'onclick' function
		gsftSubmit(null, g_form.getFormElement(), 'close_change_task'); // 'Action name' set in UI Action header
	}
	
		//Code that runs without 'onclick'
		//Ensure call to server-side function with no browser errors
		if(typeof window == 'undefined') {		
			runBusRuleCode();
		}
	
		
	//Server-side function
	function runBusRuleCode(){		
		if (current.wf_activity.name == 'Implement') {		
			// Implementation step in workflow requires work end timestamp
			var cr = new GlideRecord("change_request");
			cr.addQuery("sys_id", "=", current.change_request);
			cr.addNullQuery('work_end');  // Do not overwrite existing work_end data
			cr.query();
			// find change request that is parent of the implementation task
			if (cr.next()) {
				// set the change request work end timestamp to current datetime (and proper timezone)
				var gdt = new GlideDateTime();
				gdt.setDisplayValue(gs.nowDateTime);
				cr.work_end.setValue(gdt);			
				cr.update();
			}
		}
			 
		current.state = 3;
		current.update();	
	    action.setRedirectURL(current);
		
	}
	

# UI Policy

	Short description: Make Change Work Times Mandatory
	Table: Change Task [change_task]
	Conditions: Short Description contains Implement AND State is Closed Complete
	On load: false
	Reverse if false: true
	
	UI Policy Actions
	change_request.work_start - Mandatory: True (Visible and Read only: Leave alone)
	change_request.work_end - Mandatory: True (Visible and Read only: Leave alone)
	
	
