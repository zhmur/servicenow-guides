# Performance considerations

## Async vs sync business rules

In case async business rule is triggered system creates scheduled job (inserts record into sys_trigger table) from the business rule after the user submits the form and after any action is taken on the record in the database. Then Scheduled Worker executes the job.

Impact of slow __async__ business rule:
* The user transaction that triggered the business rule will complete before the business rule is run.
* The user will be able to immediately perform a second transaction without waiting for the business rule to complete.
* The business rule will consume a Scheduler Worker as long as it runs. If several users are doing similar transactions that trigger same async business rule that takes a long time to execute we could potentially end up with all Scheduler Workers running only the jobs triggered by async business rule. In that case following processes will be delayed: SLAs updates, events processing (and, as a result, emails delays)...

Impact of slow __sync__ (after, before, query) business rule:
* The business rule transaction will be same as user transaction so it will consume a Default semaphore as long as it runs.
* The user will not be able to perform a second transaction until the business rules completes and the transaction finishes.
* If several users are doing similar transactions that trigger same sunchronous business rule we could potentially end up with all semaphores consumed with transactions for that particular business rule. In that case new user transactions will be queued until a semaphore is available.

## New fields

New field creation can take a lot of time. Especially on the task table extends. Why? Let's take look to the sequence of actions platform executes to add new field (after Calgary, also known as onlineAlter):
1. A new empty table is created with the same schema as the table that is to be altered. The 'onlineAlter' operation is performed on this new table.
1. Triggers are added on the live table to copy any changes that are made against the live table to the new table.
1. All data is copied in chunks, from the live table to the new table.
1. Once all of the data is copied, the tables are swapped. This requires a very very brief table-lock.
The old unused table is dropped.

While the new process is safer and far more stable it does use additional resources as a result, because of this, users logged into the node that is committing the update set may experience some decreased performance during the update set commit process. It is because of this that it is highly recommended for update sets that include any new columns to the Task or Children of Task tables, should be committed outside of peak business hours. Also, because of the implementation of flattening on the Task table starting with the Dublin release, any alters against Task and Children of Task will now take extensive amounts of time given the size of the flattened task table.
