# Performance considerations

## Async vs sync business rules

In case async business rule is triggered system creates scheduled job (inserts record into sys_trigger table) from the business rule after the user submits the form and after any action is taken on the record in the database. Then Scheduled Worker executes the job.

Impact of slow performing __async__ business rule:
* The user transaction that triggered the business rule will complete before the business rule is run.
* The user will be able to immediately perform a second transaction without waiting for the business rule to complete.
* The business rule will consume a Scheduler Worker as long as it runs. If several users are doing similar transactions that trigger same async business rule that takes a long time to execute we could potentially end up with all Scheduler Workers running only the jobs triggered by the ASYNC business rule. In that case following processes will be delayed: SLAs updates, events processing (and, as a result, emails delays)...

Impact of slow performing __sync__ (after, before, query) business rule:
* The business rule transaction will be same as user transaction so it will consume a Default semaphore as long as it runs.
* The user will not be able to perform a second transaction until the business rules completes and the transaction finishes.
* If several users are doing similar transactions that trigger same sunchronous business rule we could potentially end up with all semaphores consumed with transactions for that particular business rule. In that case new user transactions will be queued until a semaphore is available.
