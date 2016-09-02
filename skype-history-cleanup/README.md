
---
## Skype History Cleanup

---
### How to open main.db
* Download *SQLite Database Browser* [ClickMe](http://sqlitebrowser.org/) (SQLDB) or *SQLiteDatabaseBrowserPortable* (SQLDBP)
* Run SQLDB and Open `%appdata%\Skype\<yourprofile>\main.db`

---
### How to Cleanup
In SQLDB, go to the **Execute SQL** tab, paste this code, and run it (pressing F5 or clicking the arrow &gt; button):

```sql
DELETE FROM `CallMembers`;
DELETE FROM `Calls`;
DELETE FROM `MediaDocuments`;
DELETE FROM `MessageAnnotations`;
DELETE FROM `Participants`;
DELETE FROM `Transfers`;
DELETE FROM `Videos`;
DELETE FROM `VideoMessages`;
DELETE FROM `Messages` WHERE `timestamp` < (strftime('%s', date('now')) - (60*60*24*15));
DELETE FROM `Conversations` WHERE `last_activity_timestamp` < (strftime('%s', date('now')) - (60*60*24*15)) OR `last_activity_timestamp` is NULL;
DELETE FROM `Chats` WHERE `last_change` < (strftime('%s', date('now')) - (60*60*24*15));
```

_Note: It will clean the most useless rows, and also all the messages/conversations/chats older than 14 days._

---
### How to Save the changes made in main.db
Just by doing:

* File -> Write Changes
* File -> Compact Database (select all of them)

_Note: Be sure to make a backup of main.db before doing this._
