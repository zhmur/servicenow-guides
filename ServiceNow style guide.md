## Syntax, names, labels, defaults

1. Use singular name for the tables. Example: "incident".
1. Use plural name for the slushbacket (_List_ type) fields. Examples: "u_regions", "u_countries".
1. Capitalize each word of the table labels. Examples: "Catalog Task", "Support Group".
1. Capitalize only first letter of the field labels. Abbreviations can be considered as an exception. Examples: "Due date", "JWT token".
1. Use ";" (semicolon) as separator if you need to show list of usernames in a string. For example, pattern "&lt;last name>, &lt;first name>" is very popular one for large enterprises because surname is more selective then a name. Example: "Zhmur, Artur; Pereiras, Michael".
1. Use "var_ as" prefix for variables in catalog items. It helped once to avoid issues when variable sets included to dotwalking.

## Maintenance
1. Put old item name to sc_cat_item.meta field in case of catalog item rename. If item was used in production at least once in last 3 months. For popular items consider keeping old name in the sc_cat_item.name for some time. So that users will have time to adapt.

## Deployments
1. Use batch update sets whenever possible. Even if you have only 2 update sets for productionizing, go ahead and select one of them as a parent.
1. Do not back out update sets unless absolutely necessary.

## Programming, code, style
1. Use ES5 [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) for all JavaScript code.
1. By default use full table name for GlideRecord class variables. Example: `var incident = new GlideRecord('incident')`.
1. Global business rule is a Legacy/Dead approach. Never create one.
1. A business rule must call an API to perform an action. The API is implemented in script includes. The code in business rules must be simple.
1. A script include method must tend to be context independent. Calls to _current_, _previous_, _worklow.scratchpad_, _current.variables_ objects should be exceptional. The objects should not be passed as parameters.
1. To debug a script always use "gs.debug" or "gs.info" instead of "gs.log" as gs.log is not working in scoped app.

## Patterns
1. Consider a script include for each table to implement data manipulations: creating, modifying, querying data.

## Performance
1. Specify fields to return by a database view.
1. Check widgets performance using ServiceNow provided [script](https://hi.service-now.com/kb_view.do?sysparm_article=KB0744521).

## Tricks
1. Use "- b" + Enter in the Navigator filter to quickly find and open "Scripts - Background".
1. In navigation menu use uppercase "table_name.DO" or "table_name.LIST" to open a form or list in a separate browser tab.
