# ServiceNow style guide
## Syntax, names, labels, defaults
1. Follow [Update sets naming convention](Update%20sets%20naming%20convention).
1. Use singular name for tables. Example: "incident".
1. Capitalize each word of the table labels. Examples: "Catalog Task", "Support Group".
1. Capitalize only first letter of the field labels. Abbreviations can be considered as an exception. Examples: "Due date", "JWT token". __WARNING:__ please check "New fields" section of the [Performance considerations](Performance%20considerations.md#new-fields) document.
1. Use plural name for the slushbacket (_List_ type) fields. Examples: "u_regions", "u_countries".
1. For catalog variable names use same rules as for field’s name. It is not necessary to use prefix "u_".
1. Place reference name or "Number" field top left on the form layout; first on the list layout.
1. Use ";" (semicolon) as separator if you need to show list of usernames in a string. For example, pattern "&lt;last name>, &lt;first name>" is very popular one for large enterprises because surname is more selective then a name. Example: "Zhmur, Artur; Dureiko, Sergey".

## Programming, code, style
1. Use ES5 [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) for all JavaScript code.
1. By default use full table name for GlideRecord class variables. Example: `var incident = new GlideRecord('incident');`.
1. Use "ID" suffix for variables containing GlideRecord sys_id in scripts. Example: `var userID = '';`.
1. Global business rules have no conditions or table restrictions and load on every page in the system. Do not use them.
1. A business rule must call an API to perform an action. The API is implemented in script includes. The code in business rules must be simple.
1. A script include method must tend to be context independent. Calls to _current_, _previous_, _worklow.scratchpad_, _current.variables_ objects should be exceptional.
1. To debug a script always use `gs.debug` and `gs.info` instead of `gs.log` as it doesn't work in scoped apps.
1. If a business rule, workflow, scheduled job/whatever script updates visible field on a record (especially task.state), always put a comment/work note into the target record which allows identifying the code which updated it.
1. Never use direct Table API for integrations, use Import Set API instead. It allows better control of an imported data and even disable if needed.
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

## Performance
1. Specify fields to return by a database view. Include only fields which you need.
1. Check widgets performance using ServiceNow provided [script](https://hi.service-now.com/kb_view.do?sysparm_article=KB0744521).
1. Be aware that workflow activities are executed synchronously after manual action (like approval). This can significantly impact form submission time. Consider 1 second timer after manual step as a remedy.
1. It is a good practice to keep scheduled jobs and imports disabled in development instance. It significantly increases instance performance which leads to quick response and means no "instance-slow" delay during development cycle. 

## Tricks
1. Use "- b" + Enter in the Navigator filter to quickly find and open "Scripts - Background".
1. Use [SN Utils](https://www.arnoudkooi.com/) browser plugin. Features: node switching, technical field names and many more.
1. Plus sign ("+") in an email address can be very useful. Everything between "+" and "@" will be ignored during email routing. For details check [RFC 822](http://www.faqs.org/rfcs/rfc822.html).
1. In navigation menu use uppercase "table_name.LIST" to open a list in a separate browser tab.
1. Override (only if necessary) list rows per page limit with URL parameter: ?sysparm_force_row_count=10000.
1. Open extremely large tables (like sys_audit) with filters only with URL parameter: ?sysparm_filter_only=true.

## Further reading
1. [High Performance Browser Networking](https://hpbn.co/)
