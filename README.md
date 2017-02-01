# coursebuilder-user-archiving
A user archiving cron job that finds users who have been inactive for a certain period of time, anonymises their personal data, such as name and email address, and inserts into an archive table and removes from the Student table

##Modifications to cron.yaml 

**Lines #22-24** - Added the URL and schedule to run the user archiving cron job

##Modifications to the code at models/models.py

###Additional classes added:

**Lines #1291-1328** - `class UserArchive(BaseEntity)`

The UserArchive class is the model that is used when storing the archived and anonymised data to the datastore. In it are variables that serve as fields, and basic methods that allow the adding of new records,
and searching of the the table by user id.

##Modifications to the code at modules/data_removal/data_removal.py

###Additional imports:

**Lines #48-49** - so we can use the datetime functionality

```
from datetime import datetime
from datetime import timedelta
```

###Additional static variables:

**Line #58** - `CM_ADMIN_EMAILS = ['<YOUR_ADMIN_EMAILS_HERE>', '<YOUR_ADMIN_EMAILS_HERE>']`

A string list of admin emails, or other emails, for the course that should not be removed when archiving, even if they meet the archiving criteria

###Additional classes added:

**Lines #573-659** - `class ArchivingUserDataCronHandler(utils.AbstractAllCoursesCronHandler)`

This class contains the cron action code that is run when the URL is called from the cron scheduler. The code does contain comments as it goes through.

###Additional Code to pre-existing methods

**Line #891** in method `register_module`

`[(DataRemovalCronHandler.URL, DataRemovalCronHandler), (ArchivingUserDataCronHandler.URL, ArchivingUserDataCronHandler)],`

The part that was added, `, (ArchivingUserDataCronHandler.URL, ArchivingUserDataCronHandler)`, is to make sure that coursebuilder knows about this mapped URL and when it receives it, 
it points it to run this class and the cron action associated with it.
