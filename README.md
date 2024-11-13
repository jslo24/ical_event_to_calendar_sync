I have written the associated script to read public iCal feed(s), create and synchronize private google calendar events with the public feed(s) to be shared with specified email recipients. The script as written:
-- should never delete host calendar events
-- updates specified fields with changes read from the public feed
-- assumes a one hour duration for events in the public feed for which no end time is specified (to resolve an observed feed data issue from testing)
-- will not be resilient to various feed data formats - testing was limited to a handful of public iCal feeds from the same publisher
-- supports multiple feeds and email specification via array format - I have not tested upper/size limits with email/feed array
-- respects Google Apps Script environment constraints - exceeding API rate limits may limit completion of script, depending on size and/or number of feeds
-- does not log everything that may be desired - season to taste, though logging can get expensive
-- requires a trigger to run on some cadence for synchonization - I chose weekly to catch calendar updates at the beginning of the week


Setting up a time-based trigger to run your syncCalendarEvents() function periodically is a great way to keep your calendar automatically synchronized. Here's how you can set it up in Google Apps Script:

Open your Google Apps Script project where your syncCalendarEvents() function is located.
In the Apps Script editor, click on the clock icon on the left sidebar. This icon represents "Triggers".
If you don't see the clock icon, you can also go to "Edit" > "Current project's triggers" in the menu bar.
In the Triggers page, click the "+ Add Trigger" button at the bottom right corner of the page.
In the "Add Trigger" dialog that appears, set up the trigger with the following settings:

Choose which function to run: syncCalendarEvents
Choose which deployment should run: Head
Select event source: Time-driven
Select type of time based trigger: This depends on how often you want the sync to occur. You can choose:

Minutes timer: Runs every n minutes
Hour timer: Runs every hour
Day timer: Runs once a day
Week timer: Runs once a week
Month timer: Runs once a month

Select time of day: If you chose Day, Week, or Month timer, you can specify the time of day it should run.

Click "Save" to create the trigger.

For example, if you want the script to run daily at 1:00 AM, you would choose:

Select type of time based trigger: Day timer
Select time of day: 1am to 2am

After setting up the trigger, Google Apps Script will automatically run your syncCalendarEvents() function at the specified intervals.
A few things to keep in mind:

Make sure your script has the necessary authorizations. The first time it runs automatically, it may fail if it hasn't been authorized to access your calendar and fetch the iCal URL.
Google has quotas and limitations on script runtime and frequency. If your script is processing a large number of events, you might need to be mindful of these limits.
You can create multiple triggers if you need more complex scheduling.
You can always come back to the Triggers page to modify or delete the trigger if you need to change the schedule or stop the automatic syncing.

By setting up this time-based trigger, your calendar will stay synchronized without you having to manually run the script each time. The frequency you choose should depend on how often your iCal feed is updated and how quickly you need those updates reflected in your Google Calendar.

NOTE - You do need to deploy the script to run it via a trigger. However, the deployment process for Google Apps Script has been simplified in recent years. Here's what you need to know:

New Apps Script Runtime:
With the new Apps Script runtime, you don't need to explicitly create a deployment for time-based triggers. The script will run using the latest code in the editor.
Head Deployment:
When you set up a trigger, it will automatically use what's called the "Head" deployment, which always runs the most recent version of your script.

Here's how to set up the trigger:

Save your script in the Apps Script editor.
Click on the clock icon on the left sidebar to open the Triggers page.
Click the "+ Add Trigger" button at the bottom right.
In the "Add Trigger" dialog:

Choose which function to run: syncCalendarEvents
Select event source: Time-driven
Select type of time based trigger: (Choose your preferred frequency)
Select time of day: (If applicable, based on your chosen frequency)


Click "Save"

That's it! You don't need to go through any additional deployment steps. The trigger will automatically use the latest version of your script each time it runs.
Important notes:

Authorization: The first time the script runs (either manually or via trigger), you'll need to authorize it to access your Google Calendar and make external requests (for fetching the iCal feeds). You might need to run the script manually once to grant these permissions.
Testing: It's a good idea to test the script manually before setting up the trigger, to ensure everything works as expected.
Execution Limits: Be aware of Google Apps Script's execution limits, especially if you're syncing multiple or large calendars.
Updates: Any changes you make to the script in the editor will be automatically used the next time the trigger runs. You don't need to update or redeploy anything.

By following these steps, your script will run periodically as scheduled, keeping your calendars in sync without any manual intervention.
