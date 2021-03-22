
# ServiceNow development guide
## Syntax, names, labels, defaults
1. Follow [Update sets naming convention](Update%20sets%20naming%20convention.md).
1. Use singular name for tables. Example: "incident", "task".
1. Capitalize each word of a table label. Examples: "Catalog Task", "Support Group".
1. Use plural name for the slushbacket (list collector) fields and variables. Examples: "u_regions", "u_countries".
1. Capitalize only first letter of a field label. Abbreviations can be considered as an exception. Examples: "Due date", "JWT token". __WARNING:__ read "New fields" section of the [Performance considerations](Performance%20considerations.md#new-fields) document.

## Programming, code, style
1. Use ES5 [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) for all JavaScript code.
1. By default use full table name for GlideRecord class variables. Example: `var incident = new GlideRecord('incident');`.
1. Use "ID" suffix for variables containing GlideRecord sys_id in scripts. Example: `var userID = '';`.
1. Do not use global business rules as they have no conditions or table restrictions and as a result load on every page in the system.
1. Do not use `gs.sleep`. The `gs.sleep` does not release session and blocks thread (occupies Scheduler Worker); the instance may run out of worker threads for other jobs.
1. Logic inside business rule should be simple. Complex logic should be incapsulated in script include(s).
1. A script include method should tend to be context independent. Calls to `current`, `previous`, `worklow.scratchpad`, `current.variables` objects should be exceptional. Passing GlideRecord through parameter or constructor is okay.
1. Use `gs.debug` and `gs.info` instead of `gs.log` as it doesn't work in a scoped apps. If you want to use it in Global scope always specify "source" parameter: `gs.log('Test message', 'Test source');`. This will help to identify origin of the messages in system log.
1. If a business rule, workflow, scheduled job/whatever script updates visible field on a record (especially task.state), always put a comment/work note into the target record which allows to quickly identify update source.
1. Never use direct "Table API" for integrations, use "Import Set API" instead. It enables better control of imported data and source indentification.
1. Before changing a code, be sure it is the same as in production and not changed by others. Be aware not to capture and transfer changes made by others.
1. When using `current.setAbortAction(true);` in business rules, always add `current.setWorkflow(false);` as otherwise it won't stop remaining BR to run in sequence.

## Maintenance
1. Put old item name to sc_cat_item.meta field in case of catalog item rename. If item was used in production at least once in last 3 months. For popular items consider keeping old name in the sc_cat_item.name for some time. So that users will have time to adapt.
1. Never delete obsolete records, use archive instead.

## Deployments
1. Use batch update sets whenever possible. Even if you have only 2 update sets for commit, go ahead and select one of them as a parent.
1. Do not back out update sets unless absolutely necessary.

## Patterns
1. Avoid custom states introduction for OOB task table extends. Address business requirements using substates.
1. Never allow deletion of records for end users (not platform administrators). Use "active" field instead.
1. Use templates to store static (default) values in case you need to create/update incident, task etc. Template can be exposed for end users. If template can't be used (for example, you want to submit request using Service Catalog API), create system property and put JSON with static values there.
1. Put meaningful information in task.short_description, especially for generic tasks. This rule is applicable for both: when you are programmatically generating it or when you are creating task by yourself. Remember! Main purpose is to quickly identify correct task from the list.
1. Consider a script include for each table to implement data manipulations: creating, modifying, querying data.
1. Use ";" (semicolon) as separator if you need to show list of usernames in a string. For example, pattern "&lt;last name>, &lt;first name>" is very popular one for large enterprises because surname is more selective then a name. Example: "Zhmur, Artur; Dureiko, Sergey".
1. Place "Number" or reference name field top left on the form layout; first on the list layout.
1. Use email subaddressing to give out alternatives to your instance email address. Format: :email "+" :tag  "@" :domain. The text of the tag can be used to configure simple and reliable filter for inbound email action. Example: dev100824+security_incident@service-now.com.

## Performance
1. Specify fields to return by a database view. Include only fields which you need.
1. Check widgets performance using ServiceNow provided [script](https://hi.service-now.com/kb_view.do?sysparm_article=KB0744521).
1. Be aware that workflow activities are executed synchronously after manual action (like approval). This can significantly impact form submission time. Consider 1 second timer after manual step as a remedy.

## Tricks & tools
1. Use "- b" + Enter in the Navigator filter to quickly find and open "Scripts - Background".
1. Use [SN Utils](https://www.arnoudkooi.com/) browser plugin. Features: node switching, technical field names and many more.
1. In navigation menu use uppercase "table_name.LIST" to open a list in a separate browser tab.
1. Override (only if necessary, this may hit instance from performance perspective) list row count per page: https://dev100824.service-now.com/incident_list.do?sysparm_force_row_count=100
1. Open large tables (for example sys_audit) without data: https://dev100824.service-now.com/incident_list.do?sysparm_filter_only=true

## Further reading
1. [High Performance Browser Networking](https://hpbn.co/)
