# Performance considerations

## New fields

When a field creation on a table is initiated there are two possible outcomes:

__Glomming__ - The system will attempt to find a physical column on the storage table where the field is being created to see if there is a column of the same data type and max length. If one is found a new storage alias entry is created (in sys_storage_alias table) for that field and no ALTER on the physical storage table is executed.

__Alter table__ - If the system cannot find a physical column to glomm to a new field will be created directly on the physical storage table resulting in an online alter. Depending on how large the physical storage table is an online alter can take time to complete.

When creating a field from the UI and if an ALTER is performed it is possible to see a table/field that has been partially created. There are quota rules at play on every instance that prevent long running transactions from executing. Specifically the UI transaction quota rule which cancels any UI transaction which runs longer than 298 seconds. Users/admins should be mindful of this as any field/table creation from the UI which results in an online alter that runs longer than 298 seconds will be aborted by the transaction quota rule leaving metadata records behind for partially created fields. The transaction quota does not apply when creating tables/fields from an update set.

The UI Transaction module can be found under System Definition > Transaction Quota Rules > UI Transactions

If the table where the field is being created is greater than 500,000 rows it is best to add conditions to ensure that if an online-alter is performed against the table it will complete successfully without aborting. To do this, an admin can simply add two OR conditions to the existing quota rule:

URL does not contain sys_dictionary OR URL does not contain sys_db_object

Table/field creation from the UI will occur mainly in two places sys_dictionary and sys_db_object. This is the reasoning for adding these two conditions. By adding the conditions the UI Transaction quota bypasses UI transactions when a field or table is being created.

INSTANT, NOCOPY and INPLACE alters are available starting from Quebec release. Your database have to be upgraded to the version of MariaDB where this is available (10.3.7). To get database version navigate to `https://<instance>.service-now.com/xmlstats.do?include=database` and check value of db.product_version tag.

Additional information is avialable in MariaDB [documentation](https://mariadb.com/kb/en/innodb-online-ddl-overview/).

By using one update set, you are able to group table alters. For example, if 10 columns (for one storage table) are created through UI then it has to go through 10 ALTERS, but if they all committed through one update set then all of them will be done through 1 alter. This works only in scope of 1 update set. Combining multiple update sets using batching will NOT allow this optimization to combine alters from multiple update sets.

## Async business rules

When async business rule is triggered system inserts record into sys_trigger table when user submits the form. Scheduled Worker executes the trigger after any action is taken on the record which triggered BR execution. <!-- Add information about condition. -->

Slow __async__ business rule:
* User transaction will complete without waiting for BR code execution. As a result, user will be able to immediately perform another transaction.
* The business rule (sys_trigger) will consume a Scheduler Worker as long as it runs. If several users are doing similar transactions (or in case batch update/upload of records is performed) we could potentially end up with all Scheduler Workers running only the jobs triggered by async business rule. In that case following processes can be: event processors, SMTP sender, POP reader...
